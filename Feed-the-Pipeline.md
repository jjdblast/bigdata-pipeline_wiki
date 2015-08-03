# Real-time Rating Simulator

## Prerequisites
* ZooKeeper is running on 'localhost:2181'
* Kafka is running on 'localhost:9200' 
* ~/flux-setenv.sh has been sourced for things like $PIPELINE_HOME
* Data file is located at '$PIPELINE_HOME/datasets/dating/ratings.csv' 

## Run Spark Streaming
[TODO]

Something along these lines (need to test and finish this)

Create the jar to be used for `spark-submit`

```
$PIPELINE_HOME/sbt assembly
```
This creates `/root/pipeline/target/scala-2.10/feedsimulator_2.10-1.0.jar`

Submit the Spark Streaming job

Note:  This is currently broken as we need to build an uber jar with the spark-kafka library and such (see build.sbt file)
```
spark-submit --class com.fluxcapacitor.pipeline.spark.streaming.StreamingRatings $PIPELINE_HOME/target/scala-2.10/feedsimulator_2.10-1.0.jar
```

## Run Feeder
```
$PIPELINE_HOME/flux-feed.sh
Starting Feeder
[info] Set current project to FeedSimulator (in build file:/root/pipeline/)
[warn] Multiple main classes detected.  Run 'show discoveredMainClasses' to see the list

Multiple main classes detected, select one to run:

 [1] com.fluxcapacitor.pipeline.akka.feeder.FeederMain
 [2] com.fluxcapacitor.pipeline.spark.streaming.StreamingRatings

Enter number: *1* <-- Select 1
```