Welcome to the Flux Capacitor Data Pipeline Wiki!

# Getting Started
## Install latest boot2docker (1.7+) 
If you have Virtual Box already installed, it's best if you could remove it (assuming you're not using it!)
boot2docker expects a certain version of Virtual Box, otherwise things won't work.

## Initialize boot2docker with enough memory (~8GB) and disk space (~50GB)
Units are Megabytes
`boot2docker --memory=8192 --disksize=50000 init` 

If you need to change these settings at some point later, you'll need to do the following:
```
boot2docker stop
boot2docker destroy
boot2docker <new settings> init
```

## Run boot2docker and ssh into it
```
boot2docker up
boot2docker ssh
```

## Make sure the init config changes worked

Check disk space is near the chosen setting

```
df -h
```

Check memory is near the chosen setting

```
cat /proc/meminfo
```

## Persistent and Non-Persistent Container Directories
Consider everything you do in a boot2docker or docker session to be scoped to the life of just that session.

The following is a path that will persist after a boot2docker restart:
`/var/lib/boot2docker`

The following is a path that will persist after a Docker restart:
`/var/lib/docker`

## Create or Download the Flux Capacitor Docker Image
### Option 1:  Create an Image from the Dockerfile Provided in this Github Repo
This will take about 10 mins to build - and lots of internet traffic - as dependent binaries and libraries will be retrieved from the internet.

(You can also pull the image from the Docker Hub Repository detailed in Option 2 below.)

```
git clone https://github.com/fluxcapacitor/pipeline.git
cd pipeline
docker build -t fluxcapacitor/pipeline .
```
Note:  If you run out of memory or disk space while building or running the image, you need to re-initialize boot2docker per the steps above.

### Option 2:  Download the Image (~2.5GB) from the Docker Hub Repo
```
docker pull fluxcapacitor/pipeline
```

## Run Docker Container with the Image and Get a Bash Prompt within the Container

Setup and Run the Docker Container.

```
docker run -it -m 8g -v ~/pipeline/notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36379:6739 -p 36081:6081 -p 37474:7474 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 31337:1337 fluxcapacitor/pipeline bash
```

Notes: 
* The `-v ~/pipeline/notebooks` maps persistent path inside the Docker Container.  This way, you won't lose changes to your Apache Zeppelin or Spark-Notebook notebooks while running inside Docker.
* Apache Zeppelin is explicitly configured to run on port 38080 in the Docker container versus the default of 8080.  We do this because Apache Zeppelin automatically starts a Web Socket connection on http port + 1; where + 1 is relative to the Docker Container port (38081, in this case).  If we use the default port 8080, and map port 8080 to 38080 from the `docker run` command, Zeppelin will create a Web Socket connection of 8080 + 1 = 8081 instead of 38081 because it is not aware of the `docker run` mapping.
* If you are running this on an AWS EC2 instance through Docker, you'll need to do the following after you login:
```
sudo su -
```

## Update the Pipeline Scripts to the Latest
```
cd ~/pipeline
git reset --hard && git pull
chmod a+rx flux-*.sh
```

## Start the Pipeline Services 
```
./flux-start-all.sh
tail -f ./nohup.out
```

## Initialize the Pipeline Data
Before initializing, check that the processes are all running as expected using the following:
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

## Populate Sample Data 
The following script will setup and populate sample data for Cassandra, Kafka, and Hive.

Note:  This will throw errors when running the `DROP TABLE IF EXISTS` calls.  You can safely ignore those.
```
./flux-create-data.sh
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
Neo4j Admin UI (7474):  37474
Kibana (5601):  35601
Netflix-Hystrix WebSocket Stream (8989):  38989
Netflix-Hystrix Dashboard (7979):  37979
Apache Spark ThriftServer Admin UI (4040): 34040
Neo4j CLI (1337):  31337
```

## Test from Inside the Docker Container
```
Spark Submit
~/spark-1.4.1-bin-hadoop2.6/bin/spark-submit --class org.apache.spark.examples.SparkPi --master spark://127.0.0.1:7077 ~/spark-1.4.1-bin-hadoop2.6/lib/spark-examples-1.4.1-hadoop2.6.0.jar 10 

# Cassandra
cqlsh
cqlsh> use sparkafterdark;
cqlsh:sparkafterdark> select * from real_time_likes;

 fromuserid | touserid | batchtime
------------+----------+-----------

(0 rows)

# ZooKeeper
zookeeper-shell 127.0.0.1:2181

# MySQL
mysql -u root -p
Enter password: password

## Test the Services from Outside the Docker Container (local browser)

First, get the IP of your boot2docker VM from your local laptop (not within boot2docker or the Docker Container) using the following:

```
local-laptop$ boot2docker ip
192.168.59.103
```

Test from Outside the Docker Container (ie. your local laptop, but not within boot2docker).
```
# Apache2 HTTP Server
http://192.168.59.103:30080

# Kafka REST API
http://192.168.59.103:34042/topics

# Apache Zeppelin Web UI
http://192.168.59.103:38080

# Apache Spark Master Admin Web UI
http://192.168.59.103:36060

# Apache Spark Worker Admin Web UI
http://192.168.59.103:36061

# Tachyon Web UI
http://192.168.59.103:39999

# ElasticSearch REST API
http://192.168.59.103:39200/_cat/indices?v

# Spark Notebook
http://192.168.59.103:39000

# Neo4j CLI
neo4j-shell -host 192.168.59.103 -port 31337

# Kibana and Logstash
http://192.168.59.103:35601


## JDBC/ODBC Integration (Tableau, MicroStrategy, Beeline, etc)
The ThriftServer should already be running on port 30000 outside the Docker container (port 10000 inside the Docker container.)

### Tableau Integration
Connect Tableau to SparkSQL using the following properties
* Server:  192.168.59.103
* Port:  30000
* Username:  hiveuser
* Password:  <empty>
* Schema:  Default
* Table:  <Your Spark SQL Table> 

### Beeline
Run the following commands outside the Docker Container (otherwise, use 127.0.0.1:10000 inside Docker Container):

```
~/spark-1.4.1-bin-hadoop2.6/bin/beeline
beeline> !connect jdbc:hive2://192.168.59.103:30000 hiveuser ''
```

## Stop the Pipeline Services
```
./flux-stop-all.sh
```
Note:  Sometimes the Zookeeper Service does not shutdown.
You'll need to use `jps` and `kill` the process manually.

## Help Me!
Feel free to email help@fluxcapacitor.com for help.  We love questions.