This Spark Streaming App receives data off of the `ratings` Kafka topic and writes all data to the `ratings` table in Cassandra and Redis. 
```
root@docker$ cd $PIPELINE_HOME/myapps && ./flux-start-streaming-ratings-exact.sh
...Starting Spark Streaming App...
```