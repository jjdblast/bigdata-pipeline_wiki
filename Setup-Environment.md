## Install VirtualBox, boot2docker, and Docker from USB or Internet
### From USB
* Install `<USB_DRIVE>/pipeline/Boot2Docker-1.7.1.pkg` (MacOS X) or `docker-install.exe` (Windows)
* Copy `<USB_DRIVE>/pipeline/boot2docker.iso` from the USB to `/Users/<user-name>/pipeline/` on your laptop.
* Initialize boot2docker as follows
```
macosx-laptop$ boot2docker --iso=/Users/<user-name>/pipeline/boot2docker.iso --memory=8192 --disksize=20000 init
``` 

### From Internet
* Download and Install latest boot2docker (1.7+) from [the boot2docker website](http://boot2docker.io/)
* This will install everything including VirtualBox, boot2docker, and Docker
* Initialize boot2docker as follows
```
macosx-laptop$ boot2docker --memory=8192 --disksize=20000 init
``` 

## Export Docker Variables to your Terminals
```
eval "$(boot2docker shellinit)"
``` 

## Notes on boot2docker
* boot2docker is needed on MacOS X to simulate a Linux VM using VirtualBox
* boot2docker will install Docker as well as a fresh version of VirtualBox if one doesn't exist
* boot2docker is a tiny Linux VM that runs a Docker daemon
* boot2docker will send all Docker calls from your macosx-laptop$ to the Docker daemon running within the Linux VM
* Once you setup and run `boot2docker up`, all commands can be run directly from your macosx-laptop$ terminal
* The init settings above are for the boot2docker VirtualBox VM only - not the Docker Container.  Those are specific on a subsequent `docker run`
* At this point, you have both boot2docker, VirtualBox, and Docker installed.


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
**THIS A VERY LONG COMMAND.  BE SURE TO COPY/PASTE ALL OF IT!**
```
macosx-laptop$ docker run -it -m 8g -v ~/pipeline/notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 36379:6379 fluxcapacitor/pipeline bash
```

## Setup and Start the Pipeline Services
* At this point, you are inside of the Docker Container.
* Source the following script to setup and start the pipeline services.
```
root@[docker]$ . ~/pipeline/flux-setup.sh
```
###--->  Don't **^** Forget the Dot `.` Above!  <---

### Verify the Following:
* The output of `export` contains `$PIPELINE_HOME` among many other new exports
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