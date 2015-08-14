This Spark Streaming App receives data off of the `ratings` Kafka topic and writes to the `ratings` table in Cassandra. 
```
root@[docker]$ cd $PIPELINE_HOME && ./flux-spark-ratings-consumer.sh
...Starting Ratings Spark Streaming App...
```