## Examples
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
core/        
feeder/      
ml/         
sql/        
streaming/
```

### Spark Core
* Project Tungsten
* Mechanical Sympathy 
* CPU Cache Aware Algorithms
* Thread Context Switch Aware Algorithms
* Efficient Sorting with Sequential and Off-Heap Data
* Using Linux Perf to Analyze and Compare Algorithms
* [core]()

### Feeder
* Akka-based App that Feeds Ratings from CSV into Kafka
* Simulates Users Posting Ratings to a Kafka Topic
* [feeder]()

### Spark ML/MLlib, GraphX, and CoreNLP
* Machine Learning
* Graph Processing
* Text Analytics and NLP
* [ml]() 

### Spark SQL
* Custom In-memory DataSource API Implementation 
* Custom Tungsten-Friendly UDF Participating in Catalyst Optimizations
* [sql]()

### Spark Streaming
* Read data from Kafka
* Store data in Cassandra, ElasticSearch, Redis
* Calculate distinct count using Redis HyperLogLog and Twitter Algebird HyperLogLog
* Calculate count and heavy hitters using Twitter Algebird CountMin Sketch
* [streaming]()

## Example Notebooks
* Flux Capacitor comes with many example notebooks to help you get started.
* You are encouraged to start with one of these examples when building your own notebooks
* The root of these examples is `$NOTEBOOKS_HOME`.
* This root folder has been mapped from the Docker Container (Guest) through to the Host filesystem using the `-v` VOLUME parameter on the `docker run` command.  
* This prevents data loss if the Docker Container crashes or exited without saving the files.
* You can view these notebooks through Apache Zeppelin by navigating your browser to `http://127.0.0.1:38080`.
* You can view these notebooks through Apache Jupyter by navigating your browser to `http://127.0.0.1:38754`.
* Below is a description of each of the paths within `$NOTEBOOKS_HOME`
```
root@docker:~# cd $NOTEBOOKS_HOME
root@docker:~/pipeline/notebooks# ll
zeppelin/       
jupyter/        
```

### Apache Zeppelin 
* [zeppelin]()

### Jupyter/iPython
* [jupyter]()
 