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
