## Ports
In order to reduce the likelihood of port collisions on the host machine, we've mapped the relatively-common container service ports to relatively-uncommon ports in the >30000 range below.

```
Service (Inside Docker Container Port):  Outside Docker Container Port

Apache Httpd (80):  30080
Apache Kafka Rest Proxy (4042):  34042
Apache Cassandra (9160, 9042):  39160, 39042
ElasticSearch (9200):  39200
Apache Spark Master (7077):  37077
Apache Zeppelin (38080, 38081):  38080, 38081
Apache Spark Master Admin UI (6060):  36060
Apache Spark Worker Admin UI (6061):  36061
Apache ZooKeeper (2181):  32181
Apache Spark JDBC/ODBC Hive ThriftServer (10000):  30000
Apache Hadoop (50070, 50090):  30070, 30090
Apache Kafka (9092):  39092
Apache Spark REST API (6066):  36066
Spark Notebook (9000):  39000
Tachyon (19999):  39999
Apache Kafka Schema Registry:  (6081):  36081
Kibana (5601):  35601
Apache Spark ThriftServer Admin UI (4040): 34040
Redis (6379): 36379
```

## Apache Zeppelin Ports
* Explicitly configured to run on port 38080 in the Docker container versus the default of 8080.  
* We do this because Apache Zeppelin automatically starts a Web Socket connection on http port + 1; where + 1 is relative to the Docker Container port (38081, in this case).  
* If we use the default port 8080, and map port 8080 to 38080 from the `docker run` command, Zeppelin will create a Web Socket connection of 8080 + 1 = 8081 instead of 38081 because it is not aware of the `docker run` mapping.

