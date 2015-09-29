## Test from Inside the Docker Container
### Kafka Native
```
root@docker$ kafka-topics --zookeeper 127.0.0.1:2181 --list
_schemas
likes
ratings
```

### Spark Submit
```
root@docker$ cd ~/pipeline && spark-submit --class org.apache.spark.examples.SparkPi --master spark://127.0.0.1:7077 $SPARK_EXAMPLES_JAR 10 
```

### Spark SQL
```
root@docker$ cd ~/pipeline && spark-sql --jars $MYSQL_CONNECTOR_JAR
...
spark-sql> show tables;
```

### Cassandra
```
root@docker$ cqlsh
cqlsh> use fluxcapacitor;

cqlsh:fluxcapacitor> select fromuserid, touserid, rating, batchtime from ratings;

 fromuserid | touserid | rating | batchtime
------------+----------+--------+-----------

(0 rows)

cqlsh:fluxcapacitor> select fromuserid, touserid, batchtime from likes;

 fromuserid | touserid | batchtime
------------+----------+-----------

(0 rows)

cqlsh> describe fluxcapacitor;
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

mysql>
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
3641 org.apache.spark.executor.CoarseGrainedExecutorBackend <-- Long-running Executor for ThriftServer
3047 org.apache.spark.deploy.SparkSubmit  <-- Long-running Spark Submit Process for ThriftServer
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

* **Make sure that you stop the Hive Thrift Server before continuing as this process occupies Spark CPU cores which may cause CPU starvation later in your exploration**:
```
root@docker$ cd ~/pipeline && $SPARK_HOME/sbin/flux-spark-submitted-job.sh
```
* Verify that the 2 processes identified above for the Hive ThriftServer have been removed with `jps -l`.

### Spark JobServer

[Spark Job Server](http://github.com/spark-jobserver/spark-jobserver) provides a REST service for sharing and managing Spark jobs.

* Start the JobServer
* Note:  This uploads two jars for testing
```
cd ~/pipeline && ./flux-start-jobserver.sh
```

* From within the Docker container
```
root@docker$ curl localhost:8099/jars
```
* Verify  you should see two JSON elements, one for "test" and one for "streaming".

* Run the Test Job
```
curl -d "input.string = a b c a b see" 'localhost:8099/jobs?appName=test&classPath=spark.jobserver.WordCountExample&sync=true'
```
* Verify the results look similar to this
```
{
  "status": "OK",
  "result": {
    "b": 2,
    "a": 2,
    "see": 1,
    "c": 1
  }
}
```
* **Make sure that you stop the Spark Job Server before continuing as this process occupies 2 Spark CPU cores which may cause starvation later in your exploration**:
```
root@docker$ cd ~/pipeline && $SPARK_HOME/sbin/stop-sparksubmitted-job.sh
```
* Verify that the 2 processes identified above for the Hive ThriftServer have been removed with `jps -l`.


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

### TODO:  iPython Notebook
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):38888
```

### Spark Notebook
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):39000
```

### TODO:  H2O Flow
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null)/flow:34321
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