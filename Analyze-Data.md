## Command Line
* Query Cassandra Directly Within the Docker Container
```
root@[docker]$ cqlsh

cqlsh> USE pipeline; SELECT fromuserid, touserid, rating, batchtime FROM ratings LIMIT 10;

 fromuserid | touserid | rating   | batchtime
------------+----------+----------+-----------
          1 |      133 | 24671840 |         8
          1 |      720 | 24671840 |         6
          1 |      971 | 24671840 |        10
          1 |     1095 | 24673840 |         7
          1 |     1616 | 24673840 |        10
          1 |     1978 | 24673840 |         7
          1 |     2145 | 24673840 |         8
          1 |     2211 | 24673840 |         8
          1 |     3751 | 24673840 |         7
          1 |     4062 | 24673840 |         3

(10 rows)
```

## Using Notebooks for Ad Hoc Data Analysis

### Apache Zeppelin
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):38080
```

### Spark-Notebook
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):39000
```

## JDBC ODBC Hive ThriftServer
### Beeline CLI - Inside of Docker
* Hive ThriftServer is running on port 10000 inside of Docker
```
root@[docker]$ beeline -u jdbc:hive2://127.0.0.1:10000 -n hiveuser -p ''
0: jdbc:hive2://127.0.0.1:10000> SELECT id, gender FROM gender LIMIT 100;
```

### Tableau - Outside of Docker
* Hive ThriftServer is running on port 30000 outside of Docker
* Connect Tableau to SparkSQL using the following properties
```
Server:  Result of `boot2docker ip` (ie. 192.168.59.103)
Port:  30000
Username:  hiveuser
Password:  <empty>
Schema:  Default
Table:  <Your Spark SQL Table> 
```
