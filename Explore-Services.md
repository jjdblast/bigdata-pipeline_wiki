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
root@docker$ spark-submit --class org.apache.spark.examples.SparkPi --master spark://127.0.0.1:7077 $SPARK_EXAMPLES_JAR 10 
```

### Cassandra
```
root@docker$ cqlsh
cqlsh> use pipeline;

cqlsh:pipeline> select fromuserid, touserid, rating, batchtime from ratings;

 fromuserid | touserid | rating | batchtime
------------+----------+--------+-----------

(0 rows)

cqlsh:pipeline> select fromuserid, touserid, batchtime from likes;

 fromuserid | touserid | batchtime
------------+----------+-----------

(0 rows)

cqlsh> describe pipeline;
...

cqlsh:pipeline> exit;
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