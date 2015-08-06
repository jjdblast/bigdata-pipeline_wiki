## Submit the Spark Streaming App
* This Spark Streaming app receives data off of the `ratings` Kafka topic and writes to the `ratings` table in Cassandra. 
```
root@[docker]$ cd $PIPELINE_HOME && ./flux-stream-ratings.sh
```