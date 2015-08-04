# Real-time Rating Simulator

## Prerequisites
* ZooKeeper is running on 'localhost:2181'
* Kafka is running on 'localhost:9200' 
* You've sourced ~/.profile to inherit necessary exports
### Source ~/pipeline/config/bash/.profile 
```
root@[docker]$ . ~/.profile
```
--->>>  Don't forget **^** the dot  <<<--- has been sourced for things like $PIPELINE_HOME
* Data file is located at '$PIPELINE_HOME/datasets/dating/ratings.csv' 

## Run Spark Streaming
### Create the jar to be used for `spark-submit`
```
root@[docker]$ cd $PIPELINE_HOME
root@[docker]$ sbt assembly assembly
```
This creates `/root/pipeline/target/scala-2.10/PipelineUberJar-assembly-1.0.jar`

### Submit the Spark Streaming Job
(This is a single command.)
```
spark-submit --class com.fluxcapacitor.pipeline.spark.streaming.StreamingRatings $PIPELINE_HOME/target/scala-2.10/PipelineUberJar-assembly-1.0.jar
...Starting Spark Streaming...
```

## Run Feeder
```
$PIPELINE_HOME/flux-feed.sh
...Starting Feeder...
```