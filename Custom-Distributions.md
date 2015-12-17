# Custom Distributions
* Not really *custom*, just with many options turned on
* This section helps remind us how we built specific versions of our tools (Spark, Zeppelin, etc)

## Spark 1.5.2
* Tachyon 0.7.1
* Hadoop 2.6.0
* Hive
* Hive ThriftServer
* Ganglia
* Kinesis

Details are [here](http://spark.apache.org/docs/latest/building-spark.html).

### Build Command (Very Long...)
```
git clone -b branch-1.5 --single-branch git@github.com:apache/spark.git branch-1.5
```
```
export MAVEN_OPTS="-Xmx16g -XX:MaxPermSize=512M -XX:ReservedCodeCacheSize=512m" && ./make-distribution.sh --name fluxcapacitor --tgz --with-tachyon --skip-java-test -Phadoop-2.6 -Dhadoop.version=2.6.0 -Phive -Phive-thriftserver -Pspark-ganglia-lgpl -Pkinesis-asl -DskipTests
```

## Zeppelin 0.6.0
* Hadoop 2.6.0
* Spark 1.5.2

Details are [here](https://github.com/apache/incubator-zeppelin).

### Build Command
```
git clone -b master --single-branch git@github.com:apache/incubator-zeppelin.git master
```
```
mvn clean package install -Pspark-1.5 -Pspark.version=1.5.2 -Phadoop-2.6 -Phadooop.version=2.6.0 -Pcassandra-spark-1.5 -DskipTests
```