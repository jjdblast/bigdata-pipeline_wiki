# Real-time Rating Simulator

## Prerequisites
* ZooKeeper is running on 'localhost:2181'
* Kafka is running on 'localhost:9200' 
* ~/flux-setenv.sh has been sourced for things like $PIPELINE_HOME
* Data file is located at '$PIPELINE_HOME/datasets/dating/ratings.dat' 

## Run
```
$PIPELINE_HOME/../sbt/bin/sbt run
```
