## Test from Inside the Docker Container
### Kafka Native
```
root@[docker]$ kafka-topics --zookeeper 127.0.0.1:2181 --list
_schemas
likes
ratings
```

### Spark Submit
```
root@[docker]$ spark-submit --class org.apache.spark.examples.SparkPi --master spark://127.0.0.1:7077 $SPARK_EXAMPLES_JAR 10 
```

### Cassandra
```
root@[docker]$ cqlsh
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
root@[docker]$ zookeeper-shell 127.0.0.1:2181

Connecting to 127.0.0.1:2181
Welcome to ZooKeeper!
JLine support is disabled

WATCHER::

WatchedEvent state:SyncConnected type:None path:null
```

### MySQL
```
root@[docker]$ mysql -u root -p 
Enter password:  password
```

### Beeline Integration with JDBC ODBC Hive ThriftServer
Run the following to test with Beeline
```
root@[docker]$ beeline -u jdbc:hive2://127.0.0.1:10000 -n hiveuser -p ''
0: jdbc:hive2://127.0.0.1:10000> SELECT id, gender FROM gender LIMIT 100;
```

## Test from Outside boot2docker and the Docker Container
* **DO NOT TYPE `exit` AS THIS WILL STOP YOUR CONTAINER**
* Launch a new macosx-laptop$ terminal
* Run the commands below to verify your setup
* `open` opens a browser on a Mac
* The IP of boot2docker (and therefore your Docker container) is as follows
```
macosx-laptop$ boot2docker ip
```

### Apache2 HTTP Server
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):30080
```

### Kafka REST API Proxy
```
macosx-laptop$ curl 'http://$(boot2docker ip 2>/dev/null):34042/topics'
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):34042/topics
```

### Apache Zeppelin Web UI
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):38080'
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):38080
```

### Apache Spark Master Admin Web UI
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):36060'
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):36060
```

### Apache Spark Worker Admin Web UI
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):36061'
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):36061
```

### Tachyon Web UI
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):39999'
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):39999
```

### ElasticSearch REST API
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):39200/_cat/indices?v'
macosx-laptop$ open $(boot2docker ip 2>/dev/null):39200/_cat/indices?v
```

### Spark Notebook
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):39000'
macosx-laptop$ open $(boot2docker ip 2>/dev/null):39000
```

### Kibana and Logstash
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):35601'
macosx-laptop$ open $(boot2docker ip 2>/dev/null):35601
```

### Ganglia
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):30080/ganglia'
macosx-laptop$ open $(boot2docker ip 2>/dev/null):30080/ganglia
```

## Tableau Integration with JDBC/ODBC Hive ThriftServer
* The ThriftServer should already be running on port 30000 outside the Docker container.
* Connect Tableau to SparkSQL using the following properties
```
Server:  Result of `boot2docker ip` (ie. 192.168.59.103)
Port:  30000
Username:  hiveuser
Password:  <empty>
Schema:  Default
Table:  <Your Spark SQL Table> 
```
