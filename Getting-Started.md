Welcome to the Flux Capacitor Data Pipeline Wiki!

# Getting Started
## Setup the Docker Environment
### MacOS X
Install latest boot2docker (1.7+) from [the Boot2docker website](http://boot2docker.io/)

Notes:
* This is needed on MacOS X to simulate a Linux VM (using VirtualBox)
* If you have Virtual Box already installed, it's best if you could remove it (assuming you're not using it!)
boot2docker expects a certain version of VirtualBox, otherwise things get ugly
* Installing boot2docker will also install docker

Initialize boot2docker with enough memory (~16GB) and disk space (~30GB)
```
macosx-laptop$ boot2docker --memory=16384 --disksize=30000 init
``` 

If you need to change these settings at some point later, you'll need to do the following
```
macosx-laptop$ boot2docker stop
macosx-laptop$ boot2docker destroy
macosx-laptop$ boot2docker <new settings> init
```

Run boot2docker and ssh into it
```
macosx-laptop$ boot2docker up
macosx-laptop$ boot2docker ssh
```

Inside boot2docker, check that the disk space and memory are near the chosen settings
```
boot2docker$ df -h
boot2docker$ cat /proc/meminfo
```
Notes:
* The settings above are for the boot2docker VirtualBox Linux VM only - not the Docker Container itself.
* At this point, you have both boot2docker (VirtualBox Linux VM) and docker installed.

### Linux (ie. AWS EC2 Ubuntu 14.04)

Install docker (boot2docker is **not** needed)
```
ec2-linux$ wget -qO- https://get.docker.com/ | sh
```

Add your user (ie. ubuntu) to the docker group:
```
ec2-linux$ sudo usermod -aG docker ubuntu
```

Notes:
* **You need to logout and login for the ubuntu user to get added to the docker group**

## Download (or Create) the Flux Capacitor Docker Image
[MacOS X] Notes
* If you're using MacOS X, you should `exit` boot2docker back to your macosx-laptop.
* At this point the boot2docker VirtualBox VM docker daemon will be running
* The docker daemon within the boot2docker VirtualBox VM will broker all docker calls from your macosx-laptop

[EC2 Linux] Notes
* If you see the following error when running the `docker` commands below, you likely did not logout and login for the ubuntu user to be added to the docker group.  Please do this, otherwise you'll need to prepend `sudo` to all of your docker commands.
```
dial unix /var/run/docker.sock: permission denied. Are you trying to connect to a TLS-enabled daemon without TLS?
```
You did likely did not logout and login again

### [Download] the Image (~2GB) from the Docker Hub Repo
```
ec2-linux or macosx-laptop$ docker pull fluxcapacitor/pipeline
```

### [Create] an Image from the Dockerfile Provided in this Github Repo
This will take about 15 mins to build - and lots of internet traffic - as dependent binaries and libraries are retrieved from the internet.

```
ec2-linux or macosx-laptop$ git clone https://github.com/fluxcapacitor/pipeline.git
ec2-linux or macosx-laptop$ cd pipeline
ec2-linux or macosx-laptop$ docker build -t fluxcapacitor/pipeline .
```
Notes
* If you run out of memory or disk space while building or running the image, you need to re-initialize boot2docker with larger `--memory` and `--disk-size` per the steps above.

### Verify that the docker image has been downloaded or created
```
ec2-linux or macosx-laptop$ docker images
```
Notes:
* Once the docker image is in your local image repository, it no longer needs to be downloaded or created.

## Run a Docker Container with the Image

Think of a Docker Container as a running Docker instance of a static Docker image.
```
ec2-linux or macosx-laptop$ docker run -it -m 12g -v ~/pipeline/notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36379:6739 -p 36081:6081 -p 37474:7474 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 31337:1337 fluxcapacitor/pipeline bash
```
At this point, you are inside of the Docker Container.

[EC2-Linux] You'll need to do the following wgeb first login to docker using the `docker run ..something.something.. bash' command above:
```
root@[docker-container-id]$ sudo su -
```

From a different terminal, check that the Docker Container (instance) is running
```
...different terminal outside of the Docker Container...
ec2-linux or macosx-laptop$ docker ps
```
Notes: 
* The `-v ~/pipeline/notebooks` maps persistent path inside the Docker Container.  This way, you won't lose changes to your Apache Zeppelin or Spark-Notebook notebooks while running inside Docker.
* Consider everything - both in-memory or on-disk - inside the Docker Container to be scoped to the life of just that Container.
* **If the Docker Container crashes or you exit the bash session, you will lose data if you're not using VOLUMES (-v)!**
* If you're creating notebooks or other assets that you want to persist, make sure to commit/push them to git or otherwise save them outside of the ephemeral Docker Container.
* Apache Zeppelin is explicitly configured to run on port 38080 in the Docker container versus the default of 8080.  We do this because Apache Zeppelin automatically starts a Web Socket connection on http port + 1; where + 1 is relative to the Docker Container port (38081, in this case).  If we use the default port 8080, and map port 8080 to 38080 from the `docker run` command, Zeppelin will create a Web Socket connection of 8080 + 1 = 8081 instead of 38081 because it is not aware of the `docker run` mapping.

## Default Persistent Paths 

The following paths will be shared between the Host (ec2-linux or macosx-laptop) and the Docker Container:
### [MacOS X Only] boot2docker Persistent Path
```
/var/lib/boot2docker
```

### Docker Persistent Path
```
/var/lib/docker
```

## Update to the Latest Pipeline Scripts and Data
You only need to do this the first time you login to the Docker Container instance - or whenever there are changes.
```
root@[docker-container-id]$ cd ~/pipeline
root@[docker-container-id]$ git reset --hard && git pull
root@[docker-container-id]$ chmod a+rx flux-*.sh
```

## Start the Pipeline Services 
```
root@[docker-container-id]$ ./flux-start-all.sh
[...wait for the start scripts to settle...]
root@[docker-container-id]$ tail -f ./nohup.out
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
root@[docker-container-id]$ ./flux-create-data.sh
root@[docker-container-id]$ tail -f ./nohup.out
```
Notes:
* This script may throw errors during the `DROP TABLE IF EXISTS` if the tables do no exist.  You can safely ignore those.

## Ports
In order to reduce the likelihood of port collisions on your ec2-linux or macosx-laptop machine, we've mapped the relatively-common container service ports to relatively-uncommon ports in the >30000 range below.

```
Service (Inside Docker Container Port):  Outside Docker Container Port

Apache Httpd (80):  30080
Apache Kafka Rest Proxy (4042):  34042
Apache Cassandra (9160, 9042):  39160, 39042
ElasticSearch (9200):  39200
Apache Spark Master (7077):  37077
Apache Zeppelin (8080, 8081):  38080, 38081
Apache Spark Master Admin UI (6060):  36060
Apache Spark Worker Admin UI (6061):  36061
Apache ZooKeeper (2181):  32181
Apache Spark JDBC/ODBC Hive ThriftServer (10000):  30000
Apache Hadoop (50070, 50090):  30070, 30090
Apache Kafka (9092):  39092
Apache Spark REST API (6066):  36066
Spark Notebook (9000):  39000
Tachyon (19999):  39999
Redis (6379):  36379
Apache Kafka Schema Registry:  (6081):  36081
Neo4j Admin UI (7474):  37474
Kibana (5601):  35601
[TODO] Netflix-Hystrix WebSocket Stream (8989):  38989
[TODO] Netflix-Hystrix Dashboard (7979):  37979
Apache Spark ThriftServer Admin UI (4040): 34040
Neo4j CLI (1337):  31337
```

## Test from Inside the Docker Container

### Spark Submit
```
root@[docker-container-id]docker$ ~/spark-1.4.1-bin-fluxcapacitor/bin/spark-submit --class org.apache.spark.examples.SparkPi --master spark://127.0.0.1:7077 ~/spark-1.4.1-bin-fluxcapacitor/lib/spark-examples-1.4.1-hadoop2.6.0.jar 10 
```

### Cassandra
```
root@[docker-container-id]$ cqlsh
cqlsh> use sparkafterdark;
cqlsh:sparkafterdark> select * from real_time_likes;

 fromuserid | touserid | batchtime
------------+----------+-----------

(0 rows)
```

### ZooKeeper
```
root@[docker-container-id]$ zookeeper-shell 127.0.0.1:2181
```

### MySQL
```
root@[docker-container-id]$ mysql -u root -p
Enter password: password
```

## Find the IP
### [MacOS X] First, get the IP of your boot2docker VM from your macosx laptop (not within boot2docker or the Docker Container)

```
macosx-laptop$ boot2docker ip
<public-ipv4>
```

### [Linux] Use the IP of your Linux machine (ie. AWS EC2 Elastic or Dynamic IP) 
If you're on an AWS EC2 instance, you can use the following to discover the public IP:
```
ec2-linux$ curl http://169.254.169.254/latest/meta-data/
<public-ipv4>
```

## Test from Outside the Docker Container (and Outside boot2docker)

Use your browser to test the following URLs.
Notes:
* The IP below is a demo server that has been setup for Flux Capacitor
* Substitute your own IP as appropriate 

### Apache2 HTTP Server
Notes:
* My demo uses port 80 for Apache2 Httpd.  
* You will likely be using port 30080 per the port mappings described earlier.
```
http://52.27.56.210:80
```

### Kafka REST API
```
http://52.27.56.210:34042/topics
```

### Apache Zeppelin Web UI
```
http://52.27.56.210:38080
```

### Apache Spark Master Admin Web UI
```
http://52.27.56.210:36060
```

### Apache Spark Worker Admin Web UI
```
http://52.27.56.210:36061
```

### Tachyon Web UI
```
http://52.27.56.210:39999
```

### ElasticSearch REST API
```
http://52.27.56.210:39200/_cat/indices?v
```

### Spark Notebook
```
http://52.27.56.210:39000
```

### Kibana and Logstash
```
http://52.27.56.210:35601
```

### Ganglia
```
http://52.27.56.210:80/ganglia
```

### Neo4j CLI
```
ec2-linux or macosx-laptop$: neo4j-shell -host 52.27.56.210 -port 31337
```

## JDBC/ODBC Integration (Tableau, MicroStrategy, Beeline, etc)
The ThriftServer should already be running on port 30000 outside the Docker container (port 10000 inside the Docker container.)

### Tableau Integration
Connect Tableau to SparkSQL using the following properties
```
Server:  52.27.56.210
Port:  30000
Username:  hiveuser
Password:  <empty>
Schema:  Default
Table:  <Your Spark SQL Table> 
```

### Beeline
Run the following commands outside the Docker Container (otherwise, use 127.0.0.1:10000 inside Docker Container):
```
$ec2-linux or macosx-laptop$ ~/spark-1.4.1-bin-fluxcapacitor/bin/beeline
beeline> !connect jdbc:hive2://52.27.56.210:30000 hiveuser ''
```

## Stopping the Pipeline Services
The following must be done within the Docker Container.
```
root@[docker-container-id]$ ./flux-stop-all.sh
```
Note:  Sometimes the a couple of the Service do not shutdown.
You'll need to use `jps -l` and `kill` these processes manually until we make these start/stop scripts more robust.
```
root@[docker-container-id]:~/pipeline# jps
4856 org.jruby.Main  <-- kill (Kibana)
10059 Jps <-- do not kill (innocent jps process)
3770 Main <-- kill (not sure)
5201 org.apache.zookeeper.server.quorum.QuorumPeerMain <-- kill (ZooKeeper)
13394 -- process information unavailable <-- kill (not sure)

root@[docker-container-id]:~/pipeline# kill -9 5201
root@[docker-container-id]:~/pipeline# kill -9 4856
root@[docker-container-id]:~/pipeline# kill -9 3770 
root@[docker-container-id]:~/pipeline# kill -9 13394 
```

## Stopping the Docker Container
From outside the Docker Container either ec2-linux or macosx-laptop, do the following
```
$ec2-linux or macosx-laptop$ docker ps
[copy the container-instance-id]
$ec2-linux or macosx-laptop$ docker stop [container-instance-id]
```

## [MacOS X] Stopping boot2docker 
The following will stop the boot2docker VirtualBox VM docker daemon.
```
macosx-laptop$ boot2docker stop
```

You can also tear down boot2docker completely as follows
```
boot2docker destroy
```

## Help Me!
Feel free to email help@fluxcapacitor.com for help.  We love questions.

## Building the Flux Capacitor Custom Distributions
This section is to remind me how we built the custom distributions for the versions of everything that we're using (Hadoop, Tachyon, etc)

### Spark 1.4.1
* Tachyon 6.4
* Hadoop 2.6
* Hive
* Hive ThriftServer
* Ganglia
* Kinesis

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