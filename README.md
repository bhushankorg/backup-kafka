# Kafka-python Scripts

### Handy commands for setting up Kafka cluster locally
- Change directory to kafka folder
> cd [kafka_2.13-3.4.0](kafka_2.13-3.4.0)
- Start Zookeeper:
> bin/zookeeper-server-start.sh config/zookeeper.properties 
- Start Kafka server
> bin/kafka-server-start.sh config/server.properties
- Create a Kafka topic
> bin/kafka-topics.sh --create --topic <topic_name> --bootstrap-server localhost:9092
- Create a Kafka Producer
>  bin/kafka-console-consumer.sh --topic <topic_name> --from-beginning --bootstrap-server localhost:9092
- Create a kafka consumer
> bin/kafka-console-producer.sh --topic <topic_name> --bootstrap-server localhost:9092