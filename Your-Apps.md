```
root@docker$ cd $MYAPPS_HOME
root@docker$ ll
total 32
drwxr-xr-x 19 root root 4096 Nov 21 12:14 ./
drwxr-xr-x 20 root root 4096 Nov 22 16:33 ../
drwxr-xr-x  7 root root 4096 Nov 21 12:25 datasource/ <-- Sample DataSource like spark-csv, spark-avro, etc
drwxr-xr-x  8 root root 4096 Nov 21 12:29 feeder/     <-- Demo Feeder App feeds Ratings from CSV to Kafka
drwxr-xr-x  5 root root 4096 Nov 21 12:31 nlp/        <-- Sample Spark and Stanford CoreNLP Text Analytics
drwxr-xr-x  3 root root 4096 Nov 21 12:20 project/    <-- SBT-specific foldder (ignore for now)
drwxr-xr-x  9 root root 4096 Nov 21 16:38 streaming/  <-- Sample Spark Streaming Apps using Kafka, Cassandra, ElasticSearch, Redis, Twitter Algebird
drwxr-xr-x  8 root root 4096 Nov 22 16:07 tungsten/   <-- Sample Tungsten Scala Code to Demo Project Tungsten and Mechanical Sympathy 

