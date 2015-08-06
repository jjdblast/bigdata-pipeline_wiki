This Spark Streaming App receives data off of the `ratings` Kafka topic and writes to the `ratings` table in Cassandra. 
```
root@[docker]$ cd $PIPELINE_HOME && ./flux-streaming-ratings.sh
```