# Custom Distributions
* Not really *custom* - more about enabling various options at build-time

## Spark
* Tachyon
* Hadoop 
* Hive
* Hive ThriftServer
* Ganglia
* netlib-java (Java wrapper around Fortran BLAS libs)

Details are [here](http://spark.apache.org/docs/latest/building-spark.html).

### Build Commands (Very Long...)
* Clone the branch/tag as follows:
```
git clone --branch 'v1.6.1' --single-branch git@github.com:apache/spark.git v1.6.1
```
* Make sure you have installed R on [Mac OSX](https://cran.r-project.org/bin/macosx/) or [Linux](https://www.digitalocean.com/community/tutorials/how-to-set-up-r-on-ubuntu-14-04) before running the following command:

* Modify the `<protobuf.version>` to `2.6.1` in the main `pom.xml` file at the root of the Spark project
```
export MAVEN_OPTS="-Xmx8g -XX:ReservedCodeCacheSize=512m" && ./dev/make-distribution.sh --name fluxcapacitor --tgz --with-tachyon --skip-java-test -Phadoop-2.6 -Dhadoop.version=2.6.0 -Psparkr -Phive -Phive-thriftserver -Pspark-ganglia-lgpl -Pnetlib-lgpl -Dscala-2.10 -DskipTests -Dmaven.wagon.http.ssl.insecure=true -Dmaven.wagon.http.ssl.allowall=true -Dmaven.wagon.http.ssl.ignore.validity.dates=true
```

## Zeppelin Master

Details are [here](https://github.com/apache/incubator-zeppelin).

### Build Command
Clone the repo as follows:

```
git clone -b master --single-branch git@github.com:apache/incubator-zeppelin.git master
```

Build the distribution
```
mvn clean package -Pbuild-distr -Ppyspark -DskipTests -Drat.skip=true -Dmaven.wagon.http.ssl.insecure=true -Dmaven.wagon.http.ssl.allowall=true -Dmaven.wagon.http.ssl.ignore.validity.dates=true
```