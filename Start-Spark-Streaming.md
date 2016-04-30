Start various Spark Streaming Apps
* Receive data from the `item_ratings` Kafka topic
* Process, aggregate, transform, and enrich the raw data
* Write the output to various data stores
* Some of these may already be started, so please check the Spark Admin UI first before starting manually

Broken down into the following Spark Streaming Batch Intervals:
* Small (<1s)
* Medium (1-30s)
* Large (>30s)

## Store Raw Data (Small Batch Interval)
### Nifi + Kafka + Cassandra (Default Demo - Likely Already Started)
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-nifi-kafka-cassandra.sh
...Starting Spark Streaming App...
```
### Cassandra
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-cassandra.sh
...Starting Spark Streaming App...
```
### ElasticSearch  
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-elasticsearch.sh
...Starting Spark Streaming App...
```
### Parquet (File-based, Parquet Columnar File Format)
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-parquet.sh
...Starting Spark Streaming App...
```
### Redis (Exact and Approximate HyperLogLog) 
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-redis.sh
...Starting Spark Streaming App...
```
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-redis-hll.sh
...Starting Spark Streaming App...
```
### In-Memory Twitter Algebird (Approximate HyperLogLog and CountMin Sketch) 
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-algebird-hll.sh
...Starting Spark Streaming App...
```
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-algebird-cms.sh
...Starting Spark Streaming App...
```

## Aggregate and Store Data (Medium Batch Interval)
### Top K Items by Rating Count
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-topk-items-by-rating-count.sh
...Starting Spark Streaming App...
```
 
## ML Model Training (Large Batch Interval)
### Incremental ALS Model Training on New Data
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-train-als-incremental.sh
...Starting Spark Streaming App...
```
### Batch ALS Model Training on All Data
```
root@docker$ cd $MYAPPS_HOME/spark/streaming && ./start-streaming-ratings-train-als-batch.sh
...Starting Spark Streaming App...
```