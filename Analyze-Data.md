## Command Line
### Cassandra's cqlsh CLI
* Query Cassandra directly inside of Docker
```
root@[docker]$ cqlsh

cqlsh> USE fluxcapacitor; SELECT fromuserid, touserid, rating, batchtime FROM ratings LIMIT 10;

 fromuserid | touserid |     batchtime |    rating
------------+----------+---------------+-----------
          1 |      133 | 1443343748000 |         8
          1 |      720 | 1443343748000 |         6
          1 |      971 | 1443343748000 |        10
          1 |     1095 | 1443343748000 |         7
          1 |     1616 | 1443343748000 |        10
          1 |     1978 | 1443343748000 |         7
          1 |     2145 | 1443343748000 |         8
          1 |     2211 | 1443343748000 |         8
          1 |     3751 | 1443343748000 |         7
          1 |     4062 | 1443343748000 |         3

(10 rows)
```

### Beeline's HiveQL CLI
* Query the Hive ThriftServer directly inside of Docker
```
root@[docker]$ beeline -u jdbc:hive2://127.0.0.1:10000 -n hiveuser -p ''
0: jdbc:hive2://127.0.0.1:10000> SELECT id, gender FROM genders LIMIT 100;
```

### Tableau Integration
* Query the Hive ThriftServer from *outside* of Docker on port `30000`
* Connect Tableau to Spark SQL using the following
```
Select Spark SQL Integration 
Server:  127.0.0.1
Port:  30000
Username:  hiveuser
Password:  <empty>
Schema:  Default
Table:  <Your Spark SQL Table> 
```
Notes:
* Tableau creates a single SQLContext within Spark
* All data passes through this single SQLContext
* This is a well-known bottleneck and encourages you to do your heavy transformations and aggregations behind Spark - and not within your visualization client (Tableau, MicroStrategy, etc).
* The less data that is sent across the network, the better.
* This SQLContext gives you full access to permanent tables that are created (ie. DataFrame.saveAsTable("ratings_perm")
* Temp tables are not accessible since they are only specific to the SQLContext that they are created in

## Using Notebooks for Ad Hoc Data Analysis
### Apache Zeppelin
```
http://127.0.0.1:38080
```
### Spark-Notebook
```
http://127.0.0.1:39000
```
### iPython/Jupyter Notebook (TODO)

Notes:
* All notebooks are in `~/pipeline/notebooks/...` and are mounted from Docker through to the Host OS at `~/pipeline/notebooks/... using the `docker run -v` volume mapping command.