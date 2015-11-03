# Custom Distributions
* Not really *custom*, just with many options turned on
* This section helps remind us how we built specific versions of our tools (Spark, Zeppelin, Spark Notebook, etc)

## Spark 1.5.1
* Tachyon 7.1
* Hadoop 2.6
* Hive
* Hive ThriftServer
* Ganglia
* Kinesis

### Build Command (Very Long...)
```
export MAVEN_OPTS="-Xmx16g -XX:MaxPermSize=512M -XX:ReservedCodeCacheSize=512m" && ./make-distribution.sh --name fluxcapacitor --tgz --with-tachyon --skip-java-test -Phadoop-2.6 -Dhadoop.version=2.6.0 -Phive -Phive-thriftserver -Pspark-ganglia-lgpl -Pkinesis-asl -DskipTests
```

## Zeppelin 0.6.0
* Hadoop 2.6.0
* Spark 1.5.1

### Build Command
```
mvn clean package -Pspark-1.5.1 -Dhadoop.version=2.6.0 -Phadoop-2.6 -DskipTests
```

## Spark-Notebook 0.6.0
* Tachyon ?
* Hadoop 2.6
* Hive
* Parquet
* (Whatever else Andy did)

### Build Command
TODO