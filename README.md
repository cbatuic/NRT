# NRT

### Steps
1. Create a Kafka topic. ```>kafka-topics.bat --create --topic test-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1```
2. View list of all topics.```>kafka-topics.bat --list --bootstrap-server localhost:9092```
3. Create producer script.
   ```python
    # pip install kafka-python
    from kafka import KafkaProducer
    
    def send_message():
        # Kafka producer setup
        producer = KafkaProducer(
            bootstrap_servers='localhost:9092',  # Replace with your Kafka broker address
            value_serializer=lambda v: str(v).encode('utf-8')  # Ensures the message is serialized as bytes
        )
    
        # Send the message to the 'test-topic' topic
        producer.send('test-topic', value="Hello, World!")
    
        # Block until all async messages are sent
        producer.flush()
        print("Message sent: Hello, World!")
    
        # Close the producer
        producer.close()
    
    if __name__ == "__main__":
        send_message()
   ```
4. Create consumer script.
   ```python
  from kafka import KafkaConsumer
  
  def consume_messages():
      # Kafka consumer setup
      consumer = KafkaConsumer(
          'test-topic',  # The topic to consume from
          bootstrap_servers='localhost:9092',  # Replace with your Kafka broker address
          group_id='test-group',  # Consumer group ID
          value_deserializer=lambda x: x.decode('utf-8')  # Deserialize bytes to string
      )
  
      # Poll for messages and print them
      print("Listening for messages...")
      for message in consumer:
          print(f"Received message: {message.value}")
  
  if __name__ == "__main__":
      consume_messages()
   ```
5. Run consumer script to start listening. ```>python consumer.py```
6. Run producer script to send message. ```>python producer.py```
