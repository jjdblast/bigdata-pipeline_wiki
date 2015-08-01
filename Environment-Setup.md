## Setup the Flux Environment

### boot2docker and Docker
Install latest boot2docker (1.7+) from [the boot2docker website](http://boot2docker.io/)

* [TODO:  Add boot2docker for Mac and Windows to the USB.]
* [TODO:  Add VirtualBox 4.3.30 for Mac and Windows to the USB.] 

Notes:
* This is needed on MacOS X to simulate a Linux VM (using VirtualBox)
* If you have Virtual Box already installed, it's best if you could remove it (assuming you're not using it!)
boot2docker expects a certain version of VirtualBox, otherwise things get ugly
* Installing boot2docker will also install Docker

Initialize boot2docker with enough memory (~16GB) and disk space (~30GB)
```
macosx-laptop$ boot2docker --memory=16384 --disksize=30000 init
macosx-laptop$ boot2docker up
``` 

ssh into boot2docker and check that the disk space and memory are near the chosen settings
```
macosx-laptop$ boot2docker ssh

boot2docker$ df -h
boot2docker$ cat /proc/meminfo
```

Notes:
* The settings above are for the boot2docker VirtualBox Linux VM only - not the Docker Container itself.
* At this point, you have both boot2docker (VirtualBox Linux VM) and docker installed.
* If you need to change these settings at some point later, you'll need to do the following
```
macosx-laptop$ boot2docker stop
macosx-laptop$ boot2docker destroy
macosx-laptop$ boot2docker <new settings> init
macosx-laptop$ boot2docker up
```

Notes:
* Using MacOS X, you should `exit` boot2docker back to your local laptop as everything should be run from there
```
boot2docker$ exit
```
* At this point the boot2docker VirtualBox VM docker daemon will be running
* The docker daemon within the boot2docker VirtualBox VM will broker all Docker calls from your local laptop to Docker

## Download the Docker Image (~2GB) from the DockerHub Registry

TODO:  Change this command to pull from the USB repository?

```
macosx-laptop$ docker pull fluxcapacitor/pipeline
```


### Verify that the docker image has been downloaded
```
macosx-laptop$ docker images
```

## Run a Docker Container with the Image

A Docker Container is a running instance (ie. EC2 instance) of a static Docker image (EC2 AMI)
```
macosx-laptop$ docker run -it -m 12g -v ~/pipeline/notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 fluxcapacitor/pipeline bash
```
At this point, you are inside of the Docker Container.

From a different terminal, check that the Docker Container (instance) is running
```
...different terminal outside of the Docker Container..

macosx-laptop$ docker ps
```

## Update to the Latest Pipeline Scripts and Data
You only need to do this the first time you login to the Docker Container instance - or whenever there are changes that need to be sync'd.
```
root@[docker]$ cd ~/pipeline
root@[docker]$ git reset --hard && git pull
root@[docker]$ chmod a+rx flux-*.sh
```

## Configure the Environment
### Source flux-setenv.sh
```
root@[docker]$ . ~/pipeline/flux-setenv.sh
```

### Configure the various tools
```
root@[docker]$ ~/pipeline/flux-config.sh
```

## Start the Pipeline Services 
```
root@[docker]$ ./flux-start.sh

[...wait for the start scripts to settle...]

root@[docker]$ tail -f ./nohup.out
```

Before continuing, make sure the output of `jps -l` looks something like the following:
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

## Initialize the Pipeline Data
### Cassandra, Kafka, and Hive
```
root@[docker]$ ./flux-create.sh
root@[docker]$ tail -f ./nohup.out
```
Notes:
* This script may throw errors during the `DROP TABLE IF EXISTS` if the tables do no exist.  You can safely ignore those.


## Test from Inside the Docker Container

### Spark Submit
```
root@[docker]docker$ spark-submit --class org.apache.spark.examples.SparkPi --master spark://127.0.0.1:7077 $SPARK_EXAMPLES_JAR 10 
```

### Cassandra
```
root@[docker]$ cqlsh
cqlsh> use pipeline;
cqlsh:pipeline> select * from gender;

id | gender 
---+-------

(0 rows)
```

### ZooKeeper
```
root@[docker]$ zookeeper-shell 127.0.0.1:2181
```

### MySQL
```
root@[docker]$ mysql -u root -p
Enter password: password
```

## Find the Host IP
Find the IP of the boot2docker host from your MacOs X laptop (not within boot2docker or the Docker Container)
```
macosx-laptop$ boot2docker ip
<public-ipv4>
```
Notes:  
* This is most-likely `192.168.59.103`
* The "host" in this case is actually the boot2docker VirtualBox VM - not your local laptop

## Test from Outside the Docker Container (and Outside boot2docker)
Use your browser to test the following URLs using the IP found from the previous step if it differs:

### Apache2 HTTP Server
```
http://192.168.59.103:30080
```

### Kafka REST API Proxy
```
http://192.168.59.103:34042/topics
```

### Kafka Native
```
./kafka-topics --zookeeper 192.168.59.103:32181 —list
```

### Apache Zeppelin Web UI
```
http://192.168.59.103:38080
```

### Apache Spark Master Admin Web UI
```
http://192.168.59.103:36060
```

### Apache Spark Worker Admin Web UI
```
http://192.168.59.103:36061
```

### Tachyon Web UI
```
http://192.168.59.103:39999
```

### ElasticSearch REST API
```
http://192.168.59.103:39200/_cat/indices?v
```

### Spark Notebook
```
http://192.168.59.103:39000
```

### Kibana and Logstash
```
http://192.168.59.103:35601
```

### Ganglia
```
http://192.168.59.103:80/ganglia
```

## JDBC/ODBC Integration (Tableau, MicroStrategy, Beeline, etc)
The ThriftServer should already be running on port 30000 outside the Docker container (port 10000 inside the Docker container.)

### Tableau Integration
Connect Tableau to SparkSQL using the following properties
```
Server:  192.168.59.103
Port:  30000
Username:  hiveuser
Password:  <empty>
Schema:  Default
Table:  <Your Spark SQL Table> 
```

### Beeline
Run the following to test with Beeline
```
$macosx-laptop$ $SPARK_HOME/bin/beeline
beeline> !connect jdbc:hive2://192.168.59.103:30000 hiveuser ''
SELECT * FROM gender LIMIT 100
```

** IF ANY OF THE ABOVE DON'T WORK, PLEASE CREATE A GITHUB ISSUE AND/OR EMAIL help@fluxcapacitor.com FOR A QUICK RESPONSE.**


## Stopping the Pipeline Services
The following must be done within the Docker Container.
```
root@[docker]$ ./flux-stop.sh
```
Note:  Sometimes the a couple of the Service do not shutdown.
You'll need to use `jps -l` and `kill` these processes manually until we make these start/stop scripts more robust.
```
root@[docker]:~/pipeline# jps
4856 org.jruby.Main  <-- kill (Kibana)
10059 Jps <-- do not kill (innocent jps process)
3770 Main <-- kill (not sure)
5201 org.apache.zookeeper.server.quorum.QuorumPeerMain <-- kill (ZooKeeper)
13394 -- process information unavailable <-- kill (not sure)

root@[docker]:~/pipeline# kill -9 5201
root@[docker]:~/pipeline# kill -9 4856
root@[docker]:~/pipeline# kill -9 3770 
root@[docker]:~/pipeline# kill -9 13394 
```

## Stopping the Docker Container
From outside the Docker Container either ec2-linux or macosx-laptop, do the following
```
macosx-laptop$ docker ps
[copy the container-instance-id]
macosx-laptop$ docker stop [container-instance-id]
```

## Stopping boot2docker 
The following will stop the boot2docker VirtualBox VM docker daemon.
```
macosx-laptop$ boot2docker stop
```

## Removing boot2docker 
You can also tear down boot2docker completely as follows
```
boot2docker destroy
```

## Help Me!
Feel free to email help@fluxcapacitor.com for help.  We love questions.
