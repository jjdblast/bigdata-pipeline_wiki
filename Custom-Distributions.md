# Custom Distributions
* Not really *custom*, just with many options turned on
* This section helps remind us how we built specific versions of our tools (Spark, Zeppelin, etc)

## Spark
* Tachyon
* Hadoop 
* Hive
* Hive ThriftServer
* Ganglia
* Kinesis
* netlib-java (Java Wrapper around optimized Fortran BLAS Linear Algebra libs)

Details are [here](http://spark.apache.org/docs/latest/building-spark.html).

### Build Commands (Very Long...)
Clone the branch/tag as follows:
```
git clone --branch 'v1.6.0' --single-branch git@github.com:apache/spark.git v1.6.0
```
Make sure you have [install R](https://www.digitalocean.com/community/tutorials/how-to-set-up-r-on-ubuntu-14-04) before running the following command:
```
export MAVEN_OPTS="-Xmx16g -XX:MaxPermSize=512M -XX:ReservedCodeCacheSize=512m" && ./make-distribution.sh --name fluxcapacitor --tgz --with-tachyon --skip-java-test -Phadoop-2.6 -Dhadoop.version=2.6.0 -Psparkr -Phive -Phive-thriftserver -Pspark-ganglia-lgpl -Pkinesis-asl -Pnetlib-lgpl -DskipTests
```

## Zeppelin Master
* Hadoop 2.6.0 (Set this insize zeppelin-env.sh)
* Spark 1.6.. (Set this version inside zeppelin-env.sh)

Details are [here](https://github.com/apache/incubator-zeppelin).

### Build Command
```
git clone -b master --single-branch git@github.com:apache/incubator-zeppelin.git master
```
```
# We're not passing these at build time because we're configuring them within conf/zeppelin-env.sh
#   -Pspark-1.6 -Phadoop-2.6 -Pcassandra-spark-1.5 -Dhadoop.version=2.6.0 -Dspark.version=1.5.2 

mvn clean package -Pbuild-distr -Ppyspark -DskipTests -Drat.skip=true
```