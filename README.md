# NRT

### Steps
1. Download Java https://www.oracle.com/java/technologies/downloads
2. Download Kafka https://kafka.apache.org/downloads
3. Set environment variables %JAVA_HOME%\bin\ and %KAFKA_HOME%\bin\
4. Edit the Configure files:
  4.1)  zookeeper.peroperties = C:/kafka/tmp/zookeeper
  4.2) server.properties  = C:/kafka/tmp/kafka-logs
5. Start the zookeeper %KAFKA_HOME%\bin\windows\zookeeper-server-start.bat %KAFKA_HOME%\config\zookeeper.properties
6. Start a kafka Broker %KAFKA_HOME%\bin\windows\kafka-server-start.bat %KAFKA_HOME%\config\server.properties
7. Check the Kafka Services jps
8. Stop the zookeeper %KAFKA_HOME%\bin\windows\zookeeper-server-stop.bat
9. Stop the Kafka Service %KAFKA_HOME%\bin\windows\kafka-server-stop.bat
10. Check the Kafka Services jps
