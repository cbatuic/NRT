# NRT

### Steps
1. Create another consumer script.
  ```python
  # Setup PostgreSQL and PgAdmin
  from kafka import KafkaConsumer
  import psycopg2
  from psycopg2 import sql
  
  # Connect to PostgreSQL database
  def connect_to_db():
      return psycopg2.connect(
          dbname='kafka_messages',
          user='your_user',
          password='your_password',
          host='localhost',
          port='5432'
      )
  
  def save_message_to_db(message):
      try:
          conn = connect_to_db()
          cursor = conn.cursor()
          cursor.execute(
              sql.SQL("INSERT INTO messages (message) VALUES (%s)"),
              [message]
          )
          conn.commit()
          cursor.close()
          conn.close()
          print(f"Saved message: {message}")
      except Exception as e:
          print(f"Error saving message: {e}")
  
  # Kafka Consumer
  consumer = KafkaConsumer(
      'test-topic', 
      bootstrap_servers='localhost:9092',
      group_id='test-group',
      value_deserializer=lambda x: x.decode('utf-8')
  )
  
  # Consume and save
  for message in consumer:
      save_message_to_db(message.value)
  
  ```     
2. Run consumer script.
