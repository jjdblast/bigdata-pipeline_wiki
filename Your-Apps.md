## Sample Apps and Libarries

* Flux Capacitor comes with many sample applications to help you get started.
* The root of these samples is `$MYAPPS_HOME`.
* Some are Spark Apps that need to be submitted to a Spark Cluster using `submit-spark`.
* Some are Standalone Apps with their own `main()` method
* Some are packages meant to be imported and used by Spark Apps, Standalone Apps, Notebooks, etc.
* Below is a high-level description of each of these examples:
```
root@docker$ cd $MYAPPS_HOME
root@docker$ ll
datasource/ <-- Sample DataSource like spark-csv, spark-avro, etc
feeder/     <-- Demo Feeder App feeds Ratings from CSV to Kafka
nlp/        <-- Sample Spark and Stanford CoreNLP Text Analytics
project/    <-- SBT-specific foldder (ignore for now)
streaming/  <-- Sample Spark Streaming Apps using Kafka, Cassandra, ElasticSearch, Redis, Twitter Algebird
tungsten/   <-- Sample Tungsten Scala Code to Demo Project Tungsten and Mechanical Sympathy 

## Sample Notebooks
* Flux Capacitor comes with many sample notebooks to help you get started.
* The root of these samples is `$NOTEBOOKS_HOME`.
* You can view these notebooks through Apache Zeppelin by navigating your browser to `http://127.0.0.1:38080`.
```
root@docker:~# cd $NOTEBOOKS_HOME
root@docker:~/pipeline/notebooks# ll
spark-notebook/ <-- Spark Notebook Example Notebooks
zeppelin/       <-- Apache Zeppelin Example Notebooks
jupyter/        <-- iPython/Jupyter Example Notebooks
```
