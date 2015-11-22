# Spark Streaming Sample Apps
Start various Spark Streaming Apps that each receive data from the same `ratings` Kafka topic, process and enrich the raw data, and write the data to the respective data store.

## Store the Raw Data (Small Batch Interval)
### Cassandra
```
root@docker$ cd $MYAPPS_HOME/streaming && ./flux-start-streaming-ratings-cassandra.sh
...Starting Spark Streaming App...
```

### Parquet (File-based, Parquet Columnar File Format)
```
root@docker$ cd $MYAPPS_HOME/streaming && ./flux-start-streaming-ratings-parquet.sh
...Starting Spark Streaming App...
```

## Redis (Exact and Approximate HyperLogLog Distinct Counts) 
```
root@docker$ cd $MYAPPS_HOME/streaming && ./flux-start-streaming-ratings-redis.sh
...Starting Spark Streaming App...
```

### Twitter Algebird (Approximate HyperLogLog Distinct Counts and CountMin Sketch Counts) 
```
root@docker$ cd $MYAPPS_HOME/streaming && ./flux-start-streaming-ratings-algebird.sh
...Starting Spark Streaming App...
```

## Some ETL and Enrichment, then Store the Data (Medium Batch Interval)
### Join Ratings with Genders
```
TODO
```

## Train Collaborative Filtering Recommendations Machine Learning Model (Large Batch Interval)
### Batch Train on All Data including All Previous Data
```
TODO
```

### Incrementally Train and Combine New Data into Existing Model
```
TODO
```