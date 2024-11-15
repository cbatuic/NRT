# NRT

### Steps
1. Create new topic. ```C:\kafka\bin\windows>kafka-topics.bat --create --topic temperature-topic --bootstrap-server localhost:9092 --partitions
1 --replication-factor 1```
2. Create producer script to generate temperature data.
```python
# kafka_produce.py
from kafka import KafkaProducer
import json
import time
import random
from datetime import datetime

# Configuration
KAFKA_TOPIC = "temperature-topic"  # Kafka topic
KAFKA_SERVER = "localhost:9092"  # Kafka server address (adjust as necessary)

# Create a Kafka producer instance
producer = KafkaProducer(
    bootstrap_servers=[KAFKA_SERVER],
    value_serializer=lambda v: json.dumps(v).encode('utf-8')  # Serialize data as JSON
)

# Function to generate random temperature between 18 and 25 degrees Celsius
def generate_random_temperature():
    return round(random.uniform(18.0, 25.0), 2)

# Function to get the current timestamp
def get_timestamp():
    return datetime.now().strftime("%m-%d-%Y_%H-%M-%S")

# Function to send temperature data to Kafka
def send_temperature_data():
    current_time = get_timestamp()
    temperature = generate_random_temperature()
    
    # Create the message
    message = {
        "timestamp": current_time,
        "temperature": temperature,
    }
    
    # Send the data to Kafka
    producer.send(KAFKA_TOPIC, value=message)
    print(f"Sent to Kafka: {message}")

# Main function to run the producer
if __name__ == "__main__":
    while True:
        send_temperature_data()
        time.sleep(5)  # Sleep for 5 seconds before sending the next data
```   
3. Create consumer script to listen to temperature topic.
```python
# kafka_consume.py
from kafka import KafkaConsumer
import json

# Configuration
KAFKA_TOPIC = "temperature-topic"
KAFKA_SERVER = "localhost:9092"

# Create a Kafka consumer instance
consumer = KafkaConsumer(
    KAFKA_TOPIC,
    bootstrap_servers=[KAFKA_SERVER],
    value_deserializer=lambda m: json.loads(m.decode('utf-8'))  # Deserialize JSON messages
)

# Consume messages from the topic and print them
if __name__ == "__main__":
    print(f"Consuming messages from topic: {KAFKA_TOPIC}")
    for message in consumer:
        print(f"Received message: {message.value}")
```
4. Run consumer script.
5. Run producer script.
6. Modify consumer script and include data persistence.
```python
#CREATE TABLE temperature_data (
#    id SERIAL PRIMARY KEY,
#    timestamp TIMESTAMP NOT NULL,
#    temperature FLOAT NOT NULL
#);

from kafka import KafkaConsumer
import psycopg2
import json
from psycopg2 import sql

# Configuration
KAFKA_TOPIC = "temperature-topic"  # Kafka topic
KAFKA_SERVER = "localhost:9092"    # Kafka server address
DB_NAME = "temperature_db"         # PostgreSQL database name
DB_USER = "your_user"              # PostgreSQL username
DB_PASSWORD = "your_password"      # PostgreSQL password
DB_HOST = "localhost"              # PostgreSQL host
DB_PORT = "5432"                  # PostgreSQL port (default)

# Kafka consumer setup
consumer = KafkaConsumer(
    KAFKA_TOPIC,  # Topic to consume from
    bootstrap_servers=[KAFKA_SERVER],  # Kafka server(s)
    group_id='temperature-consumer-group',  # Consumer group ID
    value_deserializer=lambda x: json.loads(x.decode('utf-8'))  # Deserialize JSON messages
)

# PostgreSQL connection function
def connect_to_db():
    try:
        conn = psycopg2.connect(
            dbname=DB_NAME,
            user=DB_USER,
            password=DB_PASSWORD,
            host=DB_HOST,
            port=DB_PORT
        )
        return conn
    except Exception as e:
        print(f"Error connecting to database: {e}")
        return None

# Function to save data to PostgreSQL
def save_temperature_to_db(timestamp, temperature):
    try:
        # Connect to the database
        conn = connect_to_db()
        if conn:
            cursor = conn.cursor()
            
            # Insert the data into the database
            cursor.execute(
                sql.SQL("INSERT INTO temperature_data (timestamp, temperature) VALUES (%s, %s)"),
                [timestamp, temperature]
            )
            
            # Commit the transaction
            conn.commit()
            cursor.close()
            conn.close()
            print(f"Saved temperature data to DB: {timestamp}, {temperature}°C")
        else:
            print("Database connection failed.")
    except Exception as e:
        print(f"Error saving data to DB: {e}")

# Function to process each message from Kafka and save it to the database
def process_messages():
    for message in consumer:
        # Extract the temperature data from the Kafka message
        data = message.value
        timestamp = data['timestamp']
        temperature = data['temperature']
        
        # Save the temperature data to the database
        save_temperature_to_db(timestamp, temperature)

if __name__ == "__main__":
    print("Kafka consumer started. Waiting for messages...")
    process_messages()
```
