## Example Apps and Libraries
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
core/ <-- Example DataSource like spark-csv, spark-avro, etc
feeder/     <-- Demo Feeder App feeds Ratings from CSV to Kafka
ml/        <-- Example Spark and Stanford CoreNLP Text Analytics
sql/    <-- SBT-specific foldder (ignore for now)
streaming/  <-- Example Spark Streaming Apps using Kafka, Cassandra, ElasticSearch, Redis, Twitter Algebird
<-- Example Tungsten Scala Code to Demo Project Tungsten and Mechanical Sympathy 
```

## Example Notebooks
* Flux Capacitor comes with many example notebooks to help you get started.
* You are encouraged to start with one of these examples when building your own notebooks
* The root of these examples is `$NOTEBOOKS_HOME`.
* This root folder has been mapped from the Docker Container (Guest) through to the Host filesystem using the `-v` VOLUME parameter on the `docker run` command.  
This prevents data loss if the Docker Container crashes or exited without saving the files.
* You can view these notebooks through Apache Zeppelin by navigating your browser to `http://127.0.0.1:38080`.
* Below is a description of each of the paths within `$NOTEBOOKS_HOME`
```
root@docker:~# cd $NOTEBOOKS_HOME
root@docker:~/pipeline/notebooks# ll
spark-notebook/ <-- Example Spark-Notebook Notebooks
zeppelin/       <-- Example Apache Zeppelin Notebooks
jupyter/        <-- Example iPython/Jupyter Notebooks
```