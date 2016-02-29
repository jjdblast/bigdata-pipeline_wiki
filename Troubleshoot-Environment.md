## Common Errors

### Running out of disk space
* It's likely that you have old, unused containers or images lying around
* These don't get garbage collected automatically as Docker assumes you may want to start them again
* Use the following command to clean out the Docker containers
```
docker rm `docker ps -aq`
```
* Use the following command to clean out the Docker images
```
docker rmi `docker images -q`
```

### Your kernel does not support swap limit capabilities, memory limited without swap
* Just ignore this error

### java.nio.channels.ClosedChannelException
```
java.nio.channels.ClosedChannelException
	at org.apache.spark.streaming.kafka.KafkaUtils$$anonfun$createDirectStream$2.apply(KafkaUtils.scala:416)
	at org.apache.spark.streaming.kafka.KafkaUtils$$anonfun$createDirectStream$2.apply(KafkaUtils.scala:416)
	at scala.util.Either.fold(Either.scala:97)
	at org.apache.spark.streaming.kafka.KafkaUtils$.createDirectStream(KafkaUtils.scala:415)
        ...
```
* You likely have not started your services using `start-all-services.sh`.
* Or there was an issue starting your Spark Master and Worker services. 

### Caused by: org.datanucleus.exceptions.NucleusException: Attempt to invoke the "DBCP" plugin 
```
Caused by: org.datanucleus.exceptions.NucleusException: Attempt to invoke the "DBCP" plugin to create a ConnectionPool gave an error : The specified datastore driver ("com.mysql.jdbc.Driver") was not found in the CLASSPATH. Please check your CLASSPATH specification, and the name of the driver.
	...
Caused by: org.datanucleus.store.rdbms.connectionpool.DatastoreDriverNotFoundException: The specified datastore driver ("com.mysql.jdbc.Driver") was not found in the CLASSPATH. Please check your CLASSPATH specification, and the name of the driver.
```
* You are likely not specifying the JDBC connector on the System Classpath
* There should be a `$MYSQL_CONNECTOR` ENV variable set.  
* This ENV variable lets you easily specify the JDBC connector with your `spark-submit`, `spark-sql`, or `spark-shell` commands as follows:
```
spark-sql --jars $MYSQL_CONNECTOR_JAR
```
 
### Caused by: java.io.FileNotFoundException: datasets/dating/ratings.csv (No such file or directory)
```
Caused by: java.io.FileNotFoundException: datasets/dating/ratings.csv (No such file or directory)
	at java.io.FileInputStream.open(Native Method)
	at java.io.FileInputStream.<init>(FileInputStream.java:146)
	at scala.io.Source$.fromFile(Source.scala:90)
	at scala.io.Source$.fromFile(Source.scala:75)
	at scala.io.Source$.fromFile(Source.scala:53)
        ...
```
* You likely have not run `config-services-before-starting.sh` as the required datasets have not been uncompressed.

### TaskSchedulerImpl: Initial job has not accepted any resources; check your cluster UI to ensure that workers are registered and have sufficient resources
* You likely have not configured your VM environment to have enough cores to run the Spark jobs
* Check that `spark-defaults.conf` has the following:
```
spark.executor.cores=2
spark.cores.max=2
```
* Check Number of CPU Cores for your Docker Container
```
lscpus
```
* Check the Spark Admin UI to see if a Job is pending/waiting for resources
```
macosx-linux-windows-laptop$ open http://192.69.69.1:36060
```

### Help Me!
Feel free to email help@fluxcapacitor.com for help.  We'd love to help!