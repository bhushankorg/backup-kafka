from confluent_kafka import avro
from confluent_kafka.avro import AvroProducer
import time

avro_producer_config = {
    "bootstrap.servers": "localhost:9092",
    "schema.registry.url": "http://localhost:8081"
}

value_schema_str = """
{
    "namespace": "com.example.avro",
    "type": "record",
    "name": "User",
    "fields": [
        {"name": "name", "type": "string"},
        {"name": "age", "type": "int"}
    ]
}
"""

value_schema = avro.loads(value_schema_str)

avro_producer = AvroProducer(avro_producer_config, default_value_schema=value_schema)

for i in range(10):
    record_value = {"name": "Person", "age": i*10}
    avro_producer.produce(topic='avroMessages', value=record_value)
    time.sleep(2)
    avro_producer.flush()

print("Messages published successfully!")



from confluent_kafka.avro import AvroConsumer
from confluent_kafka.avro.serializer import SerializerError

# Set Kafka topic and schema registry URL
topic = 'avroMessages'
schema_registry_url = 'http://0.0.0.0:8081'

# Set Avro consumer configuration
consumer_config = {
    'bootstrap.servers': 'localhost:9092',
    'group.id': 'avro-consumer-group',
    'schema.registry.url': schema_registry_url
}

# Create Avro consumer
consumer = AvroConsumer(consumer_config)

# Subscribe to Kafka topic
consumer.subscribe([topic])

# Read and print messages from Kafka topic
while True:
    try:
        msg = consumer.poll(1.0)
        if msg is None:
            continue
        if msg.error():
            raise SerializerError(msg.error())
        print(msg.value())
    except SerializerError as e:
        print("Message deserialization failed for {}: {}".format(msg, e))
        break

# Close Avro consumer
consumer.close()
