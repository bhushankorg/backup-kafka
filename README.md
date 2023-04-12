from pyspark.sql import SparkSession
from pyspark.sql.functions import from_json
from pyspark.sql.types import StructType, StructField, StringType, IntegerType

spark = SparkSession.builder \
    .appName("KafkaJSONConsumer") \
    .getOrCreate()

schema = StructType([
    StructField("name", StringType(), True),
    StructField("age", IntegerType(), True)
])

df = spark \
  .readStream \
  .format("kafka") \
  .option("kafka.bootstrap.servers", "your_connection_string") \
  .option("subscribe", "your_topic_name") \
  .option("kafka.sasl.mechanisms", "PLAIN") \
  .option("kafka.security.protocol", "SASL_SSL") \
  .option("kafka.sasl.username", "$ConnectionString") \
  .option("kafka.sasl.password", "your_password_key") \
  .option("kafka.request.timeout.ms", "60000") \
  .option("kafka.session.timeout.ms", "30000") \
  .load()

df = df.selectExpr("CAST(value AS STRING)")

parsed_df = df \
    .select(from_json("value", schema).alias("json")) \
    .select("json.*")

parsed_df.printSchema()

query = parsed_df \
  .writeStream \
  .format("console") \
  .option("truncate", "false") \
  .start()

query.awaitTermination()
