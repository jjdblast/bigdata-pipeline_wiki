## Command Line
### Cassandra's cqlsh CLI
* Query Cassandra directly inside of Docker
```
root@[docker]$ cqlsh

cqlsh> USE fluxcapacitor; SELECT fromuserid, touserid, rating, batchtime FROM ratings LIMIT 10;

 fromuserid | touserid | batchtime|    rating
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

### Beeline's HiveQL CLI
* Query the Hive ThriftServer directly inside of Docker
```
root@[docker]$ beeline -u jdbc:hive2://127.0.0.1:10000 -n hiveuser -p ''
0: jdbc:hive2://127.0.0.1:10000> SELECT id, gender FROM gender LIMIT 100;
```

### Tableau Integration
* Query the Hive ThriftServer outside of Docker on port 30000
* Connect Tableau to Spark SQL using the following
```
Select Spark SQL Integration 
Server:  Result of `boot2docker ip` (ie. 192.168.59.103)
Port:  30000
Username:  hiveuser
Password:  <empty>
Schema:  Default
Table:  <Your Spark SQL Table> 
```


## Using Notebooks for Ad Hoc Data Analysis

### Apache Zeppelin
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):38080
```

### Spark-Notebook
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):39000
``