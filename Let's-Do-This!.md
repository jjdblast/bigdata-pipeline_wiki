Welcome to the pipeline wiki!

# Getting Started
## Install latest boot2docker (1.7+) 
If you have Virtual Box already installed, it's best if you could remove it (assuming you're not using it!)
boot2docker expects a certain version of Virtual Box, otherwise things won't work.

## Initialize boot2docker with enough memory (10-12GB minimum)
Units are Megabytes
`boot2docker init -m 12288`

## Run boot2docker and ssh into it
```
boot2docker start
boot2docker ssh
```

## Persistent and Non-Persistent Container Directories
Consider everything you do in a boot2docker or docker session to be scoped to the life of just that session.

The following is a path that will persist after a boot2docker restart:
`/var/lib/boot2docker`

The following is a path that will persist after a Docker restart:
`/var/lib/docker`

## Create or Download the Flux Capacitor Docker Image
### Option 1:  Create an Image from the Dockerfile Provided in this Github Repo
This will take about 10-15 mins to build - and lots of internet traffic - as dependent binaries and libraries will be retrieved from the internet.

(You can also pull the image from the Docker Hub Repository detailed in Option 2 below.)

```
git clone https://github.com/fluxcapacitor/pipeline.git
cd pipeline
docker build -t fluxcapacitor/pipeline .
```
Note:  If you run out of memory while building the image, you need re-initialize boot2docker with more memory (4-6GB) per a previous step.

### Option 2:  Download the Image (3-4GB) from the Docker Hub Repo
```docker pull fluxcapacitor/pipeline```

## Run Docker Container with the Image and Get a Bash Prompt within the Container
```
docker run -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36379:6739 -p 36081:6081 -p 37474:7474 -p 38787:8787 -p 35601:5601 -it fluxcapacitor/pipeline bash
```

## Update the Pipeline Scripts to the Latest
```
cd ~/pipeline
git reset --hard && git pull
chmod 777 flux-*.sh
```

## Start the Pipeline Services 
```
./flux-start-all.sh
tail -f ./nohup.out
```
Notes
* Sometimes the Kafka Rest Proxy service does not start cleanly due to dependency race conditions.  
Use the following to start it manually:
```
nohup kafka-rest-start ~/pipeline/config/kafka-rest/kafka-rest.properties &
tail -f ./nohup.out
```
* Zeppelin has been explicitly configured to run on port 38080 versus the default of 8080.  We do this because Zeppelin automatically starts a Web Socket connection on http port + 1 (38081 in this case).  If we use the default port 8080, and map port 8080 to 38080 from the `docker run` command, Zeppelin will create a Web Socket connection 8081 instead of 38081 because it is not aware of the Docker mapping.

## Initialize the Pipeline Data
Before initializing, check that the processes are all running as expected using the following:
```
jps -l
```
The output of jps should look something like the following:
```
1972 -- process information unavailable <-- Either ElasticSearch or Cassandra
2151 kafka.Kafka <-- Kafka Server
3134 io.confluent.kafkarest.Main <-- Kafka Rest Proxy
2402 org.apache.zeppelin.server.ZeppelinServer <-- Zeppelin WebApp
430 org.apache.zookeeper.server.quorum.QuorumPeerMain <-- ZooKeeper
2380 org.apache.spark.deploy.master.Master <-- Spark Master
2328 org.apache.spark.deploy.SparkSubmit <-- Spark Submit Daemon
2439 org.apache.spark.deploy.worker.Worker <-- Spark Worker
3618 sun.tools.jps.Jps <-- This jps process
2152 io.confluent.kafka.schemaregistry.rest.Main <-- Kafka Schema Registry
2123 -- process information unavailable <-- Either ElasticSearch or Cassandra
```
Note that the "process information unavailable" message appears to be an OpenJDK [bug](https://bugs.openjdk.java.net/browse/JDK-8075773).


## Initialize Pipeline Data 
```
./flux-init-all.sh
tail -f ./nohup.out
```

## Ports
In order to reduce the likelihood of port collisions on your local machine, we've mapped the relatively-common container service ports to relatively-uncommon ports in the >30000 range below.

```
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
Neo4j (7474):  37474
RStudio Server (8787):  38787
Kibana (5601):  35601
```

## Test the Services and Pipeline Data
Notes:
* From within the running Docker container, use the relatively-common <30000 ports above.
* Use the equivalent relatively-uncommon >30000 ports above from **outside** the Docker container - either still within boot2docker or outside completely on your local laptop.
* If you're outside completely on your local laptop, you'll need to get the IP of your boot2docker VM using the following:
```
local-laptop$ boot2docker ip
```
Use the boot2docker IP above plus the >30000 port instead of localhost and <30000 below.

```
# Kafka REST API
curl 'http://localhost:4042/topics'

# Apache Zeppelin Web UI
curl 'http://localhost:8080'

# Apache Spark Master Admin Web UI
curl 'http://localhost:6060'

# Apache Spark Worker Admin Web UI
curl 'http://localhost:6061'

# Tachyon Web UI
curl 'http://localhost:19999'

# Redis
redis-cli

# Neo4j
curl 'localhost:7474'

# JDBC/ODBC Hive ThriftServer
~/spark-1.4.1-bin-hadoop2.6/bin/beeline
beeline> !connect jdbc:hive2://localhost:10000 hiveuser ''

# Cassandra
cqlsh
cqlsh> use sparkafterdark;
cqlsh:sparkafterdark> select * from real_time_likes;

 fromuserid | touserid | batchtime
------------+----------+-----------

(0 rows)

# ZooKeeper
zookeeper-shell localhost:2181

# ElasticSearch
curl 'localhost:9200/_cat/indices?v'

# Spark Submit
[TODO] ./bin/spark-submit --class org.apache.spark.examples.SparkPi --master spark://localhost:7077 ~/spark-1.4.0-bin-hadoop2.6/lib/spark-examples-1.4.0-hadoop2.6.0.jar 10 

# RStudio Server
curl 'localhost:7575'

# Spark Notebook
curl 'localhost:9000'
```

## Stop the Pipeline Services
```
./flux-stop-all.sh
```
Note:  Sometimes the Zookeeper Service does not shutdown.
You'll need to use `jps` and `kill` the process manually.

## Help Me!
Feel free to email help@fluxcapacitor.com for help.  We love questions.

## Disclaimers
* A complete end-to-end hasn't been fully tested, yet.
* I'm putting this out so others can test and enhance.