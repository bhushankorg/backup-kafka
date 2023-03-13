Kafka workflow for creating another cluster


zookeeper start
bin/zookeeper-server-start.sh config/zookeeper.properties

kafka server start
bin/kafka-server-start.sh config/server.properties

List the running server ids
bin/zookeeper-shell.sh localhost:2181 ls /brokers/ids

check server logs
kafka_logs/server_logs

Create logs for all new 3 broker servers
kafka_logs/server_logs_1
kafka_logs/server_logs_2
kafka_logs/server_logs_3

Create and edit new server brokers in config
In order to edit the new config files 
1. Change broker id to unique id
2. Edit location of log directory
3. Change listeners portNumber

cd desktop/kafka_2.13-3.4.0
bin/kafka-server-start.sh config/server.properties
bin/kafka-server-start.sh config/server1.properties
bin/kafka-server-start.sh config/server2.properties
bin/kafka-server-start.sh config/server3.properties

Create a topic
bin/kafka-topics.sh --create --topic demo_testing2 --bootstrap-server localhost:9092,localhost:9093,localhost:9094 --replication-factor 1 --partitions 5

Delete Kafka topic
bin/kafka-topics.sh --delete --topic demo_testing2 --bootstrap-server localhost:9092,localhost:9093,localhost:9094 --replication-factor 1 --partitions 5

Create a producer
bin/kafka-console-producer.sh --topic demo_testing2 --bootstrap-server localhost:9092,localhost:9093,localhost:9094

Create a consumer
bin/kafka-console-consumer.sh --topic demo_testing2 --from-beginning --bootstrap-server localhost:9092,localhost:9093,localhost:9094

bin/kafka-topics.sh --create --topic target-topic --bootstrap-server localhost:9093
bin/kafka-console-consumer.sh --topic target-topic --bootstrap-server localhost:9093
bin/kafka-console-consumer.sh --topic target-topic --from-beginning --bootstrap-server localhost:9093

./kafka-topics.sh --create --bootstrap-server localhost:9093,localhost:9094 --replication-factor 2 --partitions 2 --topic mirrormakerPOC

./kafka-topics.sh --create --bootstrap-server localhost:9095,localhost:9096 --replication-factor 2 --partitions 2 --topic mirrormakerPOC

./kafka-mirror-maker.sh --consumer.config ../config/sourceCluster1Consumer.config --num.streams 1 --producer.config ../config/targetClusterProducer.config --whitelist=".*"


