## Test Services Inside Docker Container
### Kafka Native
```
root@docker$ kafka-topics --zookeeper 127.0.0.1:2181 --list
_schemas
ratings
```

### Spark Job Submit Script
```
root@docker$ spark-submit --class org.apache.spark.examples.SparkPi --master spark://127.0.0.1:7077 $SPARK_EXAMPLES_JAR 10 
15/11/19 13:21:34 INFO DAGScheduler: Job 0 finished: reduce at SparkPi.scala:36, took 3.483164 s
...
Pi is roughly 3.14134   <--- Value of Pi using Monte Carlo Simulation with 10 iters
...
15/11/19 13:21:34 INFO SparkUI: Stopped Spark web UI at http://172.17.0.2:4040
```

### Spark Job Submit REST API
```
root@docker$ rest-submit-sparkpi-job.sh
...
{
  "action" : "CreateSubmissionResponse",
  "message" : "Driver successfully submitted as driver-20151122184642-0000",
  "serverSparkVersion" : "1.5.1",
  "submissionId" : "driver-20151122184642-0000",
  "success" : true
}
```

### Spark Shell
We've created a separate `pipeline-spark-shell.sh` script for the sole purpose of pre-configuring `--jars` and `--packages` to include things like the following:

* MySQL JDBC Connector jar used to access the MySQL Hive Metastore 
* Any custom file readers like [spark-csv](https://github.com/databricks/spark-csv)

Notes:  

* You can certainly use the regular `spark-shell.sh` that comes with Spark.  
* The $PATH env variable includes the regular Spark scripts.
```
root@docker$ pipeline-spark-shell.sh
...
spark-sql> show tables;
...
15/11/19 13:22:44 INFO DAGScheduler: Job 0 finished: processCmd at CliDriver.java:376, took 1.626844 s
genders	false   <------   genders table registered with Hive (isTemporary == false)
ratings	false   <------   ratings table registered with Hive (isTemporary == false)
Time taken: 2.179 seconds, Fetched 2 row(s)
15/11/19 13:22:44 INFO CliDriver: Time taken: 2.179 seconds, Fetched 2 row(s)
```

### Spark SQL Shell
We've created a separate `pipeline-spark-sql.sh` script for the sole purpose of pre-configuring `--jars` and `--packages` to include things like the following:

* MySQL JDBC Connector jar used to access the MySQL Hive Metastore 
* Any custom file readers like [spark-csv](https://github.com/databricks/spark-csv)

Notes: 

* You can certainly use the regular `spark-sql.sh` that comes with Spark.  
* The $PATH env variable includes the regular Spark scripts.
```
root@docker$ pipeline-spark-sql.sh
...
spark-sql> show tables;
15/11/19 13:22:44 INFO DAGScheduler: Job 0 finished: processCmd at CliDriver.java:376, took 1.626844 s
genders	false   <------   genders table registered with Hive (isTemporary == false)
ratings	false   <------   ratings table registered with Hive (isTemporary == false)
Time taken: 2.179 seconds, Fetched 2 row(s)
15/11/19 13:22:44 INFO CliDriver: Time taken: 2.179 seconds, Fetched 2 row(s)
```

### Cassandra
```
root@docker$ cqlsh
cqlsh> use fluxcapacitor;

cqlsh:fluxcapacitor> select fromuserid, touserid, rating, batchtime from ratings;

 fromuserid | touserid | rating | batchtime
------------+----------+--------+-----------

(0 rows)

cqlsh> describe fluxcapacitor;
CREATE KEYSPACE fluxcapacitor WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '1'}  AND durable_writes = true;
...

cqlsh:fluxcapacitor> exit;
```

### ZooKeeper
```
root@docker$ zookeeper-shell 127.0.0.1:2181
...
WATCHER::

WatchedEvent state:SyncConnected type:None path:null

  <-- Hit Enter a Few Times

quit  <-- Type quit to Exit
```

### MySQL
We use MySQL for the Hive Metastore instead of the default, file-based Derby because we were noticing concurrency/locking issues when running both queries through the Hive ThriftServer as well as regular Spark jobs.

Any serious production use case should use MySQL for the Hive Metastore and not rely on the default, file-based Derby implementation.
```
root@docker$ mysql -u root -p 
Enter password:  password
...
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| hive_metastore     |  <---  Hive Metastore
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.01 sec)
```

### Redis
```
root@docker$ redis-cli
127.0.0.1:6379> ping
PONG
```
### JDBC ODBC Hive ThriftServer
Start the [Hive ThriftServer](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients) Service
```
root@docker$ start-hive-thriftserver.sh
```
Verify that the following 2 new processes have been added:

```
root@docker$ jps -l
3641 org.apache.spark.executor.CoarseGrainedExecutorBackend 
3047 org.apache.spark.deploy.SparkSubmit  

root@docker:~/pipeline# jps -l
737 org.elasticsearch.bootstrap.Elasticsearch
738 org.jruby.Main
1987 org.apache.zeppelin.server.ZeppelinServer
2243 org.apache.spark.deploy.worker.Worker
2408 play.core.server.NettyServer
2123 org.apache.spark.deploy.master.Master
4177 org.apache.spark.deploy.SparkSubmit                     <-- Long-running Spark Submit Process for ThriftServer
4370 sun.tools.jps.Jps
1973 kafka.Kafka
2391 org.apache.spark.deploy.history.HistoryServer
1529 org.apache.zookeeper.server.quorum.QuorumPeerMain
2555 io.confluent.kafka.schemaregistry.rest.Main
4284 org.apache.spark.executor.CoarseGrainedExecutorBackend  <-- Long-running Executor for ThriftServer
2556 io.confluent.kafkarest.Main
```

Login and Run the Following to Query with the Beeline Hive Client 

* Note:  If you are outside of Docker, you need to use port `30000` instead of `10000` given the way we've mapped the Docker external-internal ports 
```
root@docker:~/pipeline# beeline -u jdbc:hive2://127.0.0.1:10000 -n hiveuser -p ''
Connecting to jdbc:hive2://127.0.0.1:10000
...
Connected to: Spark SQL (version 1.5.1)
Driver: Spark Project Core (version 1.5.1)
Transaction isolation: TRANSACTION_REPEATABLE_READ
Beeline version 1.5.1 by Apache Hive
0: jdbc:hive2://127.0.0.1:10000> SELECT gender, count(gender) as number_of_records FROM dating_genders GROUP BY gender;
+---------+--------------------+--+
| gender  | number_of_records  |
+---------+--------------------+--+
| F       | 61365              |
| M       | 76441              |
| U       | 83164              |
+---------+--------------------+--+
3 rows selected (0.881 seconds)
```

* **Make sure that you STOP the Hive Thrift Server before continuing as this process occupies Spark CPU cores which may cause CPU starvation later in your exploration**:
```
root@docker$ stop-sparksubmitted-job.sh
```
* Verify that the 2 processes identified above for the Hive ThriftServer have been removed with `jps -l`.
```
root@docker:~/pipeline# jps -l
737 org.elasticsearch.bootstrap.Elasticsearch
4545 sun.tools.jps.Jps
738 org.jruby.Main
1987 org.apache.zeppelin.server.ZeppelinServer
2243 org.apache.spark.deploy.worker.Worker
1973 kafka.Kafka
2391 org.apache.spark.deploy.history.HistoryServer
2408 play.core.server.NettyServer
1529 org.apache.zookeeper.server.quorum.QuorumPeerMain
2555 io.confluent.kafka.schemaregistry.rest.Main
2123 org.apache.spark.deploy.master.Master
2556 io.confluent.kafkarest.Main
```

## Test Services Outside Docker Container
**DO NOT TYPE `exit` AS THIS WILL STOP YOUR CONTAINER**
* Instead, use the combo of `CTRL-P CTRL-Q` to exit the Docker Container
* Launch a new terminal
* If using MacOSX or Windows, make sure the boot2docker IP is `127.0.0.1`  
```
macosx-or-windows-laptop$ boot2docker ip
127.0.0.1
```
* Use `curl`, `wget`, or your browser to verify the following URLs

### Apache2 HTTP Server
```
http://127.0.0.1:30080
```

### Kafka REST API Proxy
```
http://127.0.0.1:34042/topics
```

### Redis REST API Proxy (Webdis)
```
http://127.0.0.1/redis/PING
{"PING":[true,"PONG"]}
```

### Apache Zeppelin Web UI
```
http://127.0.0.1:38080
```

### Apache Spark Master Admin Web UI
* Note that the links *within* this page may be wonky due to absolute paths and incorrectly-assumed IP addresses within the UI itself.
* You'll need to replace the broken links with `127.0.0.1` and make sure all ports have `3` prepended 
(ie. `127.0.0.1:34040`, etc) 
```
http://127.0.0.1:36060
```

### Apache Spark Worker Admin Web UI
```
http://127.0.0.1:36061
```

### ElasticSearch REST API
```
http://127.0.0.1:39200/_cat/indices?v
```

### Kibana and Logstash
```
http://127.0.0.1:35601
```

### Ganglia
```
http://127.0.0.1:30080/ganglia
```