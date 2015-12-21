## Example Spark Applications, Streaming Jobs, Libraries, and Low-Level JVM/Linux Code Samples
* Flux Capacitor comes with many example applications to help you get started.
* You are encouraged to start with one of these examples when building your own custom apps or libraries
* The root of these examples is `$MYAPPS_HOME`.
* Some are Spark Apps that need to be submitted to a Spark Cluster using `submit-spark`.
* Some are Standalone Apps with their own `main()` method
* Some are packages meant to be imported and used by Spark Apps, Standalone Apps, Notebooks, etc.
* Below is a high-level description of each of these examples separated into different paths within `$MYAPPS_HOME`:
```
root@docker$ cd $MYAPPS_HOME
root@docker$ ll
[core](http://advancedspark.com)/       <-- Example Project Tungsten/Mechanical Sympathy Low-Level CPU Cache, Thread, Sort, and Linux Perf Examples
feeder/     <-- Demo Feeder App feeds Ratings from CSV to Kafka
ml/         <-- Example Spark ML Machine Learning, GraphX Graph Processing, and Stanford CoreNLP Text Analytics
sql/        <-- Example DataSource like spark-csv, spark-avro, etc
streaming/  <-- Example Spark Streaming Apps using Kafka, Cassandra, ElasticSearch, Redis, Twitter Algebird
```

## Example Notebooks
* Flux Capacitor comes with many example notebooks to help you get started.
* You are encouraged to start with one of these examples when building your own notebooks
* The root of these examples is `$NOTEBOOKS_HOME`.
```
root@docker:~# cd $NOTEBOOKS_HOME
root@docker:~/pipeline/notebooks# ll
zeppelin/       <-- Example Apache Zeppelin Notebooks
jupyter/        <-- Example iPython/Jupyter Notebooks
```
* This root folder has been mapped from the Docker Container (Guest) through to the Host filesystem using the `-v` VOLUME parameter on the `docker run` command.  
* This prevents data loss if the Docker Container crashes or exited without saving the files.
* You can view these notebooks through Apache Zeppelin by navigating your browser to `http://127.0.0.1:38080`.
* You can view these notebooks through Apache Jupyter by navigating your browser to `http://127.0.0.1:37777`.
* Below is a description of each of the paths within `$NOTEBOOKS_HOME`
