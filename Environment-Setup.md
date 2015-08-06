## Setup the Flux Environment

### Install VirtualBox, boot2docker, and Docker from USB or Internet
### From USB
* Install `<USB_DRIVE>/pipeline/Boot2Docker-1.7.1.pkg` (MacOS X) or `docker-install.exe` (Windows)
* Copy `/pipeline/boot2docker.iso` from the USB to `/Users/<user-name>/pipeline/` on your laptop.
* Initialize boot2docker
```
macosx-laptop$ boot2docker --iso=/Users/<user-name>/pipeline/boot2docker.iso --memory=8192 --disksize=20000 init
``` 

### From Internet
* Download and Install latest boot2docker (1.7+) from [the boot2docker website](http://boot2docker.io/)
* This will install everything including VirtualBox, boot2docker, and Docker
* Initialize boot2docker
```
macosx-laptop$ boot2docker --memory=8192 --disksize=20000 init
``` 

Notes:
* boot2docker is needed on MacOS X to simulate a Linux VM using VirtualBox
* boot2docker is a tiny Linux VM that runs a Docker daemon
* boot2docker will broker all Docker calls from your macosx-laptop$ to the Docker daemon running within the Linux VM
* Once you setup and run boot2docker, all commands can be run directly from your macosx-laptop$ terminal
* If you have VirtualBox already installed, it's best if you could remove it (assuming you're not using it!)
* boot2docker will install a fresh version of VirtualBox and Docker

### Export Docker Variables to your Terminals
```
eval "$(boot2docker shellinit)"
``` 

Notes:
* The settings above are for the boot2docker VirtualBox VM only - not the Docker Container itself.
* At this point, you have both boot2docker, VirtualBox, and Docker installed.
* If you have any errors running subsequent Docker commands, be sure you have set the environment variables specific for your setup as specified above.

## Load the Docker Image from USB or Internet

### From USB
```
macosx-laptop$ docker load < /Users/<user-name>/pipeline/fluxcapacitor-pipeline.tar
```

### Load from Internet (DockerHub Registry)
```
macosx-laptop$ docker pull fluxcapacitor/pipeline
```

## Verify that the Docker Image is Loaded
```
macosx-laptop$ docker images
```

## Run a Docker Container with the Image

A Docker Container is a running instance (ie. EC2 instance) of a static Docker image (EC2 AMI)
NOTE:  THIS A VERY LONG COMMAND - BE SURE TO COPY/PASTE ALL OF IT!
```
macosx-laptop$ docker run -it -m 8g -v ~/pipeline/notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 fluxcapacitor/pipeline bash
```
At this point, you are inside of the Docker Container.

## Configure and Start the Pipeline Services
```
root@[docker]$ . ~/pipeline/flux-setup.sh
```
###-->  Don't **^** Forget the Dot  <--

### Verify the Following:
* The output of `export` contains $PIPELINE_HOME
* The output of `jps -l` looks something like this:
```
2374 kafka.Kafka <-- Kafka Server
3764 io.confluent.kafka.schemaregistry.rest.Main <-- Kafka Schema Registry
2373 org.apache.zookeeper.server.quorum.QuorumPeerMain <-- ZooKeeper
95 -- process information unavailable <-- Either ElasticSearch or Cassandra*
3765 io.confluent.kafkarest.Main <-- Kafka Rest Proxy
3762 play.core.server.NettyServer <-- Spark-Notebook
919 -- process information unavailable <-- Either ElasticSearch or Cassandra*
3641 org.apache.spark.executor.CoarseGrainedExecutorBackend <-- Long-running Executor for ThriftServer
2435 org.apache.zeppelin.server.ZeppelinServer <-- Zeppelin WebApp
2743 org.apache.spark.deploy.master.Master <-- Spark Master
4074 sun.tools.jps.Jps <-- This jps Process
3047 org.apache.spark.deploy.SparkSubmit  <-- Long-running Spark Submit Process for ThriftServer
3599 tachyon.master.TachyonMaster <-- Tachyon Master
3718 tachyon.worker.TachyonWorker <-- Tachyon Worker
2908 org.apache.spark.deploy.worker.Worker <-- Spark Worker
```
Note that the "process information unavailable" message appears to be an OpenJDK [bug](https://bugs.openjdk.java.net/browse/JDK-8075773).


## Test from Inside the Docker Container

### Spark Submit
```
root@[docker]docker$ spark-submit --class org.apache.spark.examples.SparkPi --master spark://127.0.0.1:7077 $SPARK_EXAMPLES_JAR 10 
```

### Cassandra
```
root@[docker]$ cqlsh
cqlsh> use pipeline;

cqlsh:pipeline> select fromuserid, touserid, rating, batchtime from likes;

 fromuserid | touserid | rating | batchtime
------------+----------+--------+-----------

(0 rows)

cqlsh:pipeline> select fromuserid, touserid, batchtime from ratings;

 fromuserid | touserid | batchtime
------------+----------+-----------

(0 rows)

cqlsh> describe pipeline;
...

cqlsh:pipeline> exit;
```

### ZooKeeper
```
root@[docker]$ zookeeper-shell 127.0.0.1:2181

Connecting to 127.0.0.1:2181
Welcome to ZooKeeper!
JLine support is disabled

WATCHER::

WatchedEvent state:SyncConnected type:None path:null
```

### MySQL
```
root@[docker]$ mysql -u root -p 
Enter password:  password
```

## Test from Outside boot2docker and the Docker Container
* Keep your Docker Container running
* Launch a new terminal
* Run the following commands to test that your services have started properly
* `open` opens a browser on a Mac
* `curl` is pretty standard

### Apache2 HTTP Server
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):30080`
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):30080
```

### Kafka REST API Proxy
```
macosx-laptop$ curl 'http://$(boot2docker ip 2>/dev/null):34042/topics'
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):34042/topics
```

### Apache Zeppelin Web UI
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):38080'
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):38080
```

### Apache Spark Master Admin Web UI
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):36060'
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):36060
```

### Apache Spark Worker Admin Web UI
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):36061'
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):36061
```

### Tachyon Web UI
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):39999'
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):39999
```

### ElasticSearch REST API
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):39200/_cat/indices?v'
macosx-laptop$ open $(boot2docker ip 2>/dev/null):39200/_cat/indices?v
```

### Spark Notebook
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):39000'
macosx-laptop$ open $(boot2docker ip 2>/dev/null):39000
```

### Kibana and Logstash
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):35601'
macosx-laptop$ open $(boot2docker ip 2>/dev/null):35601
```

### Ganglia
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):30080/ganglia'
macosx-laptop$ open $(boot2docker ip 2>/dev/null):30080/ganglia
```

### Kafka Native
```
macosx-laptop$ ./kafka-topics --zookeeper $(boot2docker ip 2>/dev/null):32181 —list
```

## JDBC/ODBC Integration (Tableau, MicroStrategy, Beeline, etc)
The ThriftServer should already be running on port 30000 outside the Docker container (port 10000 inside the Docker container.)

### Tableau Integration
Connect Tableau to SparkSQL using the following properties
```
Server:  Result of `boot2docker ip` (ie. 192.168.59.103)
Port:  30000
Username:  hiveuser
Password:  <empty>
Schema:  Default
Table:  <Your Spark SQL Table> 
```

### Beeline
Run the following to test with Beeline
```
$macosx-laptop$ $SPARK_HOME/bin/beeline -u jdbc:hive2://$(boot2docker ip 2>/dev/null):30000 -n hiveuser -p ''
SELECT * FROM gender LIMIT 100
```

** IF ANY OF THE ABOVE DON'T WORK, PLEASE CREATE A GITHUB ISSUE AND/OR EMAIL help@fluxcapacitor.com FOR A QUICK RESPONSE.**