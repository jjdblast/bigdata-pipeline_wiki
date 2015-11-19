## Test from Inside the Docker Container
### Kafka Native
```
root@docker$ kafka-topics --zookeeper 127.0.0.1:2181 --list
_schemas
ratings
```

### Spark Submit
```
root@docker$ cd ~/pipeline && spark-submit --class org.apache.spark.examples.SparkPi --master spark://127.0.0.1:7077 $SPARK_EXAMPLES_JAR 10 
15/11/19 13:21:34 INFO DAGScheduler: Job 0 finished: reduce at SparkPi.scala:36, took 3.483164 s
Pi is roughly 3.14134  <-------   The value of Pi based on Monte Carlo Simulation of 10 iterations
15/11/19 13:21:34 INFO SparkUI: Stopped Spark web UI at http://172.17.0.2:4040
```

### Spark SQL
```
root@docker$ cd ~/pipeline && spark-sql --jars $MYSQL_CONNECTOR_JAR
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

Connecting to 127.0.0.1:2181
Welcome to ZooKeeper!
JLine support is disabled

WATCHER::

WatchedEvent state:SyncConnected type:None path:null

<-- Hit Enter a Few Times

quit  <-- Type quit to Exit
```

### MySQL
```
root@docker$ mysql -u root -p 
Enter password:  password

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 47
Server version: 5.5.44-0ubuntu0.14.04.1 (Ubuntu)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

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
root@docker$ cd ~/pipeline && ./flux-start-hive-thriftserver.sh
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

Run the following to query with the Beeline Hive client 
```
root@docker$ beeline -u jdbc:hive2://127.0.0.1:10000 -n hiveuser -p ''
0: jdbc:hive2://127.0.0.1:10000> SELECT id, gender FROM genders LIMIT 10;
+-----+---------+
| id  | gender  |
+-----+---------+
| 1   | F       |
| 2   | F       |
| 3   | U       |
| 4   | F       |
| 5   | F       |
| 6   | F       |
| 7   | F       |
| 8   | M       |
| 9   | M       |
| 10  | M       |
+-----+---------+
```

* **Make sure that you STOP the Hive Thrift Server before continuing as this process occupies Spark CPU cores which may cause CPU starvation later in your exploration**:
```
root@docker$ cd ~/pipeline && ./flux-stop-sparksubmitted-job.sh
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

## Test from Outside boot2docker and the Docker Container
* **DO NOT TYPE `exit` AS THIS WILL STOP YOUR CONTAINER**
* Launch a new macosx-laptop$ terminal
* Run the commands below to verify your setup
* `open` opens a browser on a Mac
* The IP of boot2docker (and therefore your Docker Container) is as follows
```
macosx-laptop$ boot2docker ip
```

### Apache2 HTTP Server
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):30080
```

### Kafka REST API Proxy
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):34042/topics
```

### Apache Zeppelin Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):38080
```

### iPython Notebook
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):37777
```

### Spark Notebook
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):39000
```

### Apache Spark Master Admin Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):36060
```

### Apache Spark Driver Admin Web UI
* Jobs, Stages, Tasks
* Environment Variables
* Event Timeline
* Streaming Tab
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):34040
```

### Apache Spark Worker Admin Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):36061
```

### Tachyon Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):39999
```

### ElasticSearch REST API
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):39200/_cat/indices?v
```

### Kibana and Logstash
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):35601
```

### Ganglia
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):30080/ganglia
```