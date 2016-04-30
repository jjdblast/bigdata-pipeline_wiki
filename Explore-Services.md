## Explore Service Inside Docker Container
### Kafka Native
```
kafka-topics --zookeeper 127.0.0.1:2181 --list
_schemas
item_ratings
item_ratings_geo
...
```

### Spark Job Submit Script
```
spark-submit --class org.apache.spark.examples.SparkPi --master spark://127.0.0.1:7077 $SPARK_EXAMPLES_JAR 10 
15/11/19 13:21:34 INFO DAGScheduler: Job 0 finished: reduce at SparkPi.scala:36, took 3.483164 s
...
Pi is roughly 3.14134   <--- Value of Pi using Monte Carlo Simulation with 10 iters
...
15/11/19 13:21:34 INFO SparkUI: Stopped Spark web UI at http://172.17.0.2:4040
```

### Spark Job Submit REST API
```
rest-submit-sparkpi-job.sh
...
{
  "action" : "CreateSubmissionResponse",
  "message" : "Driver successfully submitted as driver-20151122184642-0000",
  "serverSparkVersion" : "1.6.1",
  "submissionId" : "driver-20151122184642-0000",
  "success" : true
}
```

### Spark Shell
We've created a separate `pipeline-spark-shell.sh` script for the sole purpose of pre-configuring `--jars` and `--packages` to include things like the following:

* MySQL JDBC Connector jar used to access the MySQL Hive Metastore 
* Any custom file readers like [spark-csv](https://github.com/databricks/spark-csv)

Notes:  

* You can certainly use the regular `spark-sql.sh` that comes with Spark, but this will not have the `--jars` and `--packages` configured as described above.
```
pipeline-spark-shell.sh
...
scala> 1 + 1
res0: Int = 2
```
* Hit `Ctrl-C` to exit

### PySpark Shell
We've created a separate `pipeline-pyspark-shell.sh` script for the sole purpose of pre-configuring `--jars` and `--packages` to include things like the following:
* MySQL JDBC Connector jar used to access the MySQL Hive Metastore 
* Any custom file readers like [spark-csv](https://github.com/databricks/spark-csv)

Notes:  
* You can certainly use the regular `pyspark.sh` that comes with Spark, but this will not have the `--jars` and `--packages` configured as described above.
```
pipeline-pyspark-shell.sh
...
>>> 1 + 1
2
```
* Hit `Ctrl-D` to exit

### Spark SQL Shell
We've created a separate `pipeline-spark-sql.sh` script for the sole purpose of pre-configuring `--jars` and `--packages` to include things like the following:

* MySQL JDBC Connector jar used to access the MySQL Hive Metastore 
* Any custom file readers like [spark-csv](https://github.com/databricks/spark-csv)

Notes: 

* You can certainly use the regular `spark-sql.sh` that comes with Spark, but this will not have the `--jars` and `--packages` configured as described above.
```
spark-sql> show tables;
15/11/19 13:22:44 INFO DAGScheduler: Job 0 finished: processCmd at CliDriver.java:376, took 1.626844 s
datings_genders	false   <------   datings_genders table registered with Hive (isTemporary == false)
datings_ratings	false   <------   datings_ratings table registered with Hive (isTemporary == false)
Time taken: 2.179 seconds, Fetched 2 row(s)
15/11/19 13:22:44 INFO CliDriver: Time taken: 2.179 seconds, Fetched 2 row(s)
```
* Hit `Ctrl-C` to exit

### Cassandra
```
cqlsh
cqlsh> use advancedspark;

cqlsh:advancedspark> select userid, rating, timestamp from item_ratings;
...

cqlsh> describe advancedspark;
CREATE KEYSPACE advancedspark WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '1'} AND durable_writes = true;
...
```
* Type `exit` to exit
```
cqlsh:advancedspark> exit;
```

### ZooKeeper
```
zookeeper-shell 127.0.0.1:2181
...
WATCHER::

WatchedEvent state:SyncConnected type:None path:null
<enter>  <-- Hit Enter a Few Times
<enter>
...
  
```
* Type `quit` to quit
```
quit
```

### MySQL
* We use MySQL to back the Hive Metastore Service
* Serious production deployments should use MySQL (or Postgres) behind the Hive Metastore Service
* The default, file-based Derby implementation causes many concurrency/locking issues beyond a single connection.
```
mysql -u root -p 
Enter password:  password
...
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| hive_metastore     |  <---  Hive Metastore Database
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.01 sec)
```

* Type `exit` to exit
```
mysql> exit
```
### Hive Metastore Service
* Make sure the Hive Metastore Service is running.
* This services uses the Hive Metastore Database shown above.
```
netstat -an | grep 9083
tcp        0      0 0.0.0.0:9083            0.0.0.0:*               LISTEN 
```

### Redis
```
redis-cli
127.0.0.1:6379> ping
PONG
```

* Type `exit` to exit
```
127.0.0.1:6379> exit
```

### JDBC ODBC Hive ThriftServer
* Start the [Hive ThriftServer](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients) Service
```
start-hive-thriftserver.sh
```

* Verify that the following 2 new processes have been added:
```
jps -l
3641 org.apache.spark.executor.CoarseGrainedExecutorBackend 
3047 org.apache.spark.deploy.SparkSubmit  

root@docker:~/pipeline# jps -l
...
4177 org.apache.spark.deploy.SparkSubmit                     <-- Long-running Spark Submit Process for Hive ThriftServer
4284 org.apache.spark.executor.CoarseGrainedExecutorBackend  <-- Long-running Executor for Hive ThriftServer
...
```

* Login and Run the Following to Query with the Beeline Hive Client 
```
beeline -u jdbc:hive2://127.0.0.1:10000 -n hiveuser -p ''
Connecting to jdbc:hive2://127.0.0.1:10000
...
Connected to: Spark SQL (version 1.5.1)
Driver: Spark Project Core (version 1.5.1)
Transaction isolation: TRANSACTION_REPEATABLE_READ
Beeline version 1.5.1 by Apache Hive
0: jdbc:hive2://127.0.0.1:10000> SHOW TABLES;
+-----------------+--------------+--+
|    tableName    | isTemporary  |
+-----------------+--------------+--+
| dating_genders  | false        |
| dating_ratings  | false        |
| movie_links     | false        |
| movie_ratings   | false        |
| movie_tags      | false        |
| movies          | false        |
+-----------------+--------------+--+
```
```
0: jdbc:hive2://127.0.0.1:10000> SELECT gender, count(gender) as num_records_per_gender FROM dating_genders GROUP BY gender;
+---------+--------------------+--+
| gender  | number_of_records  |
+---------+--------------------+--+
| F       | 61365              |
| M       | 76441              |
| U       | 83164              |
+---------+--------------------+--+
3 rows selected (0.881 seconds)
```

![Apache Zeppelin Notebooks](http://advancedspark.com/img/zeppelin-hive-thriftserver.png)

* Type `quit` to exit
```
0: jdbc:hive2://127.0.0.1:10000> quit
```

* Stop the long-running Hive Thrift Server frees up CPU cores for more Spark exploration
```
stop-sparksubmitted-job.sh
```

* Verify that the 2 processes identified above for the Hive ThriftServer have been removed with `jps -l`.
```
jps -l
...
737 org.elasticsearch.bootstrap.Elasticsearch
738 org.jruby.Main
1987 org.apache.zeppelin.server.ZeppelinServer
2391 org.apache.spark.deploy.history.HistoryServer
1529 org.apache.zookeeper.server.quorum.QuorumPeerMain
4545 sun.tools.jps.Jps
2123 org.apache.spark.deploy.master.Master
2243 org.apache.spark.deploy.worker.Worker
1973 kafka.Kafka
2556 io.confluent.kafkarest.Main
2555 io.confluent.kafka.schemaregistry.rest.Main
2547 org.apache.hadoop.util.RunJar                    
```

### Presto + Kafka
* Requires `start-presto-service.sh` (which may already be running)
* Run `pipeline-presto-kafka.sh` and query a live Kafka Topic
```
pipeline-presto-kafka.sh
...
presto:default> select _key, _message from item_ratings;
 _key  |  _message  
-------+------------
 33057 | 33057,3,1  
 33057 | 33057,9,1  
 33057 | 33057,8,1  
 33057 | 33057,14,1 
 33057 | 33057,16,1 
(5 rows)

Query 20160403_043056_00020_jtkti, FINISHED, 1 node
Splits: 2 total, 2 done (100.00%)
0:00 [5 rows, 47B] [147 rows/s, 1.35KB/s]
```
* Type `quit` to exit
```
presto:default> quit
```

### Presto + Hive/SparkSQL
* Requires `start-presto-service.sh` (may already be running)
* Run `pipeline-presto-hive.sh` and query a Hive-friendly table 
_(ie. non-Hive-friendly tables use SerDe's like `com.databricks.spark.csv`)_
```
presto:default> select * from movies_hive_friendly limit 10;
  id  |               title                |                    tags                     
------+------------------------------------+---------------------------------------------
 NULL | title                              | genres                                      
    1 | Toy Story (1995)                   | Adventure|Animation|Children|Comedy|Fantasy 
    2 | Jumanji (1995)                     | Adventure|Children|Fantasy                  
    3 | Grumpier Old Men (1995)            | Comedy|Romance                              
    4 | Waiting to Exhale (1995)           | Comedy|Drama|Romance                        
    5 | Father of the Bride Part II (1995) | Comedy                                      
    6 | Heat (1995)                        | Action|Crime|Thriller                       
    7 | Sabrina (1995)                     | Comedy|Romance                              
    8 | Tom and Huck (1995)                | Adventure|Children                          
    9 | Sudden Death (1995)                | Action                                      
(10 rows)

Query 20160403_160506_00010_wq24r, FINISHED, 1 node
Splits: 2 total, 2 done (100.00%)
0:01 [2.46K rows, 114KB] [4.51K rows/s, 209KB/s]
```

### Titan
* Requires `start-titan-service.sh`
```
root@docker$ gremlin.sh $MYAPPS_HOME/titan/item-ratings.gremlin
         \,,,/
         (o o)
-----oOOo-(3)-oOOo-----
plugin activated: aurelius.titan
plugin activated: tinkerpop.server
plugin activated: tinkerpop.utilities
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/root/titan-1.0.0-hadoop1/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/root/titan-1.0.0-hadoop1/lib/logback-classic-1.1.2.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
00:55:52 INFO  org.apache.tinkerpop.gremlin.hadoop.structure.HadoopGraph  - HADOOP_GREMLIN_LIBS is set to: /root/titan-1.0.0-hadoop1/lib
plugin activated: tinkerpop.hadoop
plugin activated: tinkerpop.tinkergraph
==>standardtitangraph[cassandrathrift:[127.0.0.1]]
==>/root/pipeline/datasets/graph/item_ratings.csv
gremlin> 
```
* `ctrl-c` to exit

## Test Services Outside Docker Container
**DO NOT TYPE `exit` from DOCKER AS THIS WILL STOP YOUR CONTAINER**

* Instead, use the combo of `CTRL-P CTRL-Q` to exit the Docker Container
```
ctrl-p ctrl-q
```

* Use `curl`, `wget`, or your browser to verify the following URLs

### Apache2 HTTP Server - HOME PAGE - ALWAYS START HERE IF LOST
```
http://<ip>
```

### Kafka REST API Proxy
```
http://<ip>:36042/topics
```

### Redis REST API Proxy (Webdis)
```
http://<ip>/redis/PING
{"PING":[true,"PONG"]}
```

### Zeppelin Notebook Web UI
```
http://<ip>:8080
```

### iPython/Jupyter/Tensorflow Notebook Web UI
```
http://<ip>:8754
```

### Spark Master Admin Web UI
* Note that the links *within* this page may be wonky due to absolute paths and incorrectly-assumed IP addresses within the UI itself.
(ie. `<ip>:4040`, <ip>:4041, etc) 
```
http://127.0.0.1:6060
```

### Spark History Server UI
* Requires `start-history-server.sh`
```
http://<ip>:5050
```

### Spark Worker Admin Web UI
```
http://<ip>:6061
```

### ElasticSearch REST API
* Show the current ElasticSearch indexes
```
http://<ip>:9200/_cat/indices?v
```

* Query the `advancedspark` index and `item_ratings` document type
```
http://<ip>:9200/advancedspark/item_ratings/_search?q=*&pretty
```

### Kibana and Logstash
* Main Kibana landing page
```
http://<ip>:5601
```

* ElasticSearch Graph Plugin (graph data through Kibana)
```
http://<ip>:5601/app/graph
```

* ElasticSearch Sense Plugin (run queries through Kibana)
```
http://<ip>:5601/app/sense
```

### Ganglia
```
http://<ip>/ganglia
```

### NiFi
```
http://<ip>:6969/nifi/
```

### AirFlow
```
http://<ip>:5060/
```

### Presto
```
http://<ip>:7060
```

### Titan
* Requires `start-titan-service.sh`
```
http://<ip>:8182/
```

### Flask-based Recommendation/Prediction Service
```
http://<ip>:5090/predict/12663/7
```