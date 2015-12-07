# Custom Distributions
* Not really *custom*, just with many options turned on
* This section helps remind us how we built specific versions of our tools (Spark, Zeppelin, Spark Notebook, etc)

## Spark 1.6.x
* Tachyon 8.2
* Hadoop 2.6.0
* Hive
* Hive ThriftServer
* Ganglia
* Kinesis

### Build Command (Very Long...)
```
git clone -b branch-1.6 --single-branch git@github.com:apache/spark.git branch-1.6
```
```
export MAVEN_OPTS="-Xmx16g -XX:MaxPermSize=512M -XX:ReservedCodeCacheSize=512m" && ./make-distribution.sh --name fluxcapacitor --tgz --with-tachyon --skip-java-test -Phadoop-2.6 -Dhadoop.version=2.6.0 -Phive -Phive-thriftserver -Pspark-ganglia-lgpl -Pkinesis-asl -DskipTests
```

## Zeppelin 0.6.0
* Hadoop 2.6.0
* Spark 1.6.x

### Build Command
```
mvn clean package install -Pspark-1.6.0 -Dhadoop.version=2.6.0 -Phadoop-2.6 -DskipTests
```

## Spark-Notebook 0.6.0
* Tachyon ?
* Hadoop 2.6.0
* Hive
* Parquet
* (Whatever else Andy did)

### Build Command
TODO