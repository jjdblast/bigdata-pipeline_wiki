(These are here for us to remember what we did!)

## Building the Flux Capacitor Custom Distributions
This section is to remind me how we built the custom distributions for the versions of everything that we're using (Hadoop, Tachyon, etc)

### Spark 1.4.1
* Tachyon 6.4
* Hadoop 2.6
* Hive
* Hive ThriftServer
* Ganglia
* Kinesis

Command (Very Long...)
```
export MAVEN_OPTS="-Xmx16g -XX:MaxPermSize=512M -XX:ReservedCodeCacheSize=512m" && ./make-distribution.sh --name fluxcapacitor --tgz --with-tachyon --skip-java-test -Phadoop-2.6 -Dhadoop.version=2.6.0 -Phive -Phive-thriftserver -Pspark-ganglia-lgpl -Pkinesis-asl -DskipTests
```

### Zeppelin 0.5.2
* Added Custom Help Widget in Lower Right of Notebooks

### Spark-Notebook
* Tachyon
* Hadoop 2.6
* Hive
* Parquet
* (Whatever else Andy did)