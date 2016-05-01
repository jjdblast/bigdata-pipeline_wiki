* This repo comes with many example applications to help you get started.
* You are encouraged to start with one of these examples when building your own custom apps or libraries
* The root of these examples is `$MYAPPS_HOME`.
* Some are Spark Apps that need to be submitted to a Spark Cluster using `submit-spark`.
* Some are Standalone Apps with their own `main()` method
* Some are packages meant to be imported and used by Spark Apps, Standalone Apps, Notebooks, etc.
* Below is a high-level description of each of these examples separated into different paths within `$MYAPPS_HOME`
```
root@docker$ cd $MYAPPS_HOME
root@docker$ ll
airflow/
akka/
flink/  
jupyter/
nifi/
serving/    
spark/
tensorflow/
titan/
zeppelin/
```

### Airflow
* [airflow](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/airflow)
* AirFlow Workflow DAGs

### Akka/Feeder
* [akka/feeder](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/akka/feeder)
* Akka-based App that Feeds Ratings from CSV into a Kafka Topic
* Simulates Users Posting Ratings to a Kafka Topic
 
### Flink Streaming
* [flink/streaming](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/flink/streaming) 
* Flink Streaming 

### Jupyter
* [juptyer](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/jupyter)
* Jupyter/iPython notebooks including PySpark, TensorFlow, SciKit-Learn, NetworkX, Matplotlib and various other examples from around the world wide web (aka www)

### Spark Core
* [spark/core](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/spark/core)
* Project Tungsten
* Mechanical Sympathy 
* CPU Cache Aware Algorithms
* Thread Context Switch Aware Algorithms
* Efficient Sorting with Sequential and Off-Heap Data
* Using Linux Perf to Analyze and Compare Algorithms

### Spark ML/MLlib, GraphX, and CoreNLP
* [spark/ml](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/spark/ml) 
* Machine Learning
* Graph Processing
* Text Analytics and NLP

### Spark SQL
* [spark/sql](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/spark/sql)
* Custom In-memory DataSource API Implementation 
* Custom Tungsten-Friendly UDF Participating in Catalyst Optimizations

### Spark Streaming
* [spark/streaming](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/spark/streaming)
* Read data from Kafka
* Store data in Cassandra, ElasticSearch, Redis
* Calculate distinct count using Redis HyperLogLog and Twitter Algebird HyperLogLog
* Calculate count and heavy hitters using Twitter Algebird CountMin Sketch

### NiFi
* [nifi](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/nifi) 
* NiFi Data Flows

### Serving/Prediction ML Layer 
* [serving](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/serving) 
* ML Serving Layer

### Apache Zeppelin 
* [zeppelin](https://github.com/fluxcapacitor/pipeline/tree/master/myapps/zeppelin)
* Python and Scala-based notebooks including many new and existing examples from around the web