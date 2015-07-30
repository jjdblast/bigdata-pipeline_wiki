# Spark 1.4.1

## Contains the Following Components
* Tachyon
* Hadoop 2.6
* Hive
* Hive ThriftServer
* Ganglia
* Kinesis

## Command to Build
```
export MAVEN_OPTS="-Xmx16g -XX:MaxPermSize=512M -XX:ReservedCodeCacheSize=512m" && ./make-distribution.sh --name fluxcapacitor --tgz --with-tachyon --skip-java-test -Phadoop-2.6 -Dhadoop.version=2.6.0 -Phive -Phive-thriftserver -Pspark-ganglia-lgpl -Pkinesis-asl -DskipTests
```