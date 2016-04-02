## Common Errors

### The name "/pipeline" is already in use by container
* You have likely exited out of the running Docker Container that you initially created and trying to re-run the `docker run ...` command
* Instead, you should use `docker attach pipeline` to attach to the existing Docker Container
* You may need to start the container using `docker start pipeline` and then run `docker attach pipeline` and click `<enter>` a few times.
* If you have to start the container, you will likely need to run the following when you re-attach:  `source ~/.profile` to setup the path and environment variables 
* (We will make this less annoying in the future.)

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

* Verify the number of CPU cores matches what you expect (at least 4 CPU cores or things won't work right)
Note:  Knowing this number will help you troubleshoot Spark problems later as you may hit Spark Job resource starvation issues if you run too many long-running jobs at the same time (ie. Hive ThriftServer, Spark Streaming, Spark Job Server).
```
root@docker$ lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                8   <----------- Number of CPU Cores
On-line CPU(s) list:   0-7
Thread(s) per core:    2
Core(s) per socket:    4
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 62
Stepping:              4
CPU MHz:               2500.094
BogoMIPS:              5000.18
Hypervisor vendor:     Xen
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              25600K
NUMA node0 CPU(s):     0-7
```

* Check the Spark Admin UI to see if a Job is pending/waiting for resources
```
macosx-linux-windows-laptop$ open http://192.69.69.1:36060
```

### Help Me!
Feel free to email help@fluxcapacitor.com for help.  We'd love to help!