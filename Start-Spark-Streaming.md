Start various Spark Streaming Apps
* Receive data from the `item_ratings` Kafka topic
* Process, aggregate, transform, and enrich the raw data
* Write the output to various data stores

Note:  The examples below are are broken down by Small, Medium, and Large Batch Intervals

## Store Raw Data (Small Batch Interval)
### Cassandra
```
root@docker$ cd $MYAPPS_HOME/streaming && ./start-streaming-ratings-cassandra.sh
...Starting Spark Streaming App...
```
### ElasticSearch  
```
root@docker$ cd $MYAPPS_HOME/streaming && ./start-streaming-ratings-elasticsearch.sh
...Starting Spark Streaming App...
```
### Parquet (File-based, Parquet Columnar File Format)
```
root@docker$ cd $MYAPPS_HOME/streaming && ./start-streaming-ratings-parquet.sh
...Starting Spark Streaming App...
```
### Redis (Exact and Approximate HyperLogLog) 
```
root@docker$ cd $MYAPPS_HOME/streaming && ./start-streaming-ratings-redis.sh
...Starting Spark Streaming App...
```
```
root@docker$ cd $MYAPPS_HOME/streaming && ./start-streaming-ratings-redis-hll.sh
...Starting Spark Streaming App...
```
### In-Memory Twitter Algebird (Approximate HyperLogLog and CountMin Sketch) 
```
root@docker$ cd $MYAPPS_HOME/streaming && ./start-streaming-ratings-algebird-hll.sh
...Starting Spark Streaming App...
```
```
root@docker$ cd $MYAPPS_HOME/streaming && ./start-streaming-ratings-algebird-cms.sh
...Starting Spark Streaming App...
```

## Aggregate and Store Data (Medium Batch Interval)
### Top K Items by Rating Count
```
root@docker$ cd $MYAPPS_HOME/streaming && ./start-streaming-ratings-topk-items-by-rating-count.sh
...Starting Spark Streaming App...
```
### Hourly Rollups (TODO)
### Spark Streaming 1.6 TTL Sessionization Feature (TODO)
 
## ML Model Training (Large Batch Interval)
### Incremental Model Training on New Data
```
root@docker$ cd $MYAPPS_HOME/streaming && ./start-streaming-ratings-train-als-incremental.sh
...Starting Spark Streaming App...
```
### Batch Training on All Data
```
root@docker$ cd $MYAPPS_HOME/streaming && ./start-streaming-ratings-train-als-batch.sh
...Starting Spark Streaming App...
```