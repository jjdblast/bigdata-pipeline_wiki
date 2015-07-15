Welcome to the pipeline wiki!

# Getting Started
## Install latest boot2docker (1.7+) 
If you have Virtual Box already installed, it's best if you could remove it (assuming you're not using it!)
boot2docker expects a certain version of Virtual Box, otherwise things won't work.

## Initialize boot2docker with enough memory (4-6 GB minimum)
Units are Megabytes
`boot2docker init -m 6144`

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

## Option 1:  Create an Image from the Dockerfile Provided in this Github Repo
This will take about 10-15 mins to build - and lots of internet traffic - as dependent binaries and libraries will be retrieved from the internet.

(You can also pull the image from the Docker Hub Repository detailed in Option 2 below.)

```
git clone https://github.com/fluxcapacitor/pipeline.git
cd pipeline
docker build -t fluxcapacitor/pipeline .
```
Note:  If you run out of memory while building the image, you need re-initialize boot2docker with more memory (4-6GB) per a previous step.

## Option 2:  Download the Image (2-3GB) from the Docker Hub Repo
```docker pull fluxcapacitor/pipeline```

## Run Docker Container with the Image and Get a Bash Prompt within the Container
```
docker run -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:8080 -p 38081:8081 -p 36060:6060 -p 36061:6061 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -it fluxcapacitor/pipeline bash
```

```
cd ~
```

## Ports
In order to reduce the likelihood of port collisions on your local machine, I've mapped the somewhat-common container service ports to uncommon ports in the `30000` range below.

Note:  You'll need to use the 30000+ ports listed below to access these services outside of the Docker container.
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
```

## Update the Pipeline Scripts to the Latest
```
cd ~/pipeline
git reset --hard && git pull
chmod 777 *.sh
```

## Start the Pipeline Services 
```
./flux-start-all.sh
```

## Initialize the Pipeline Data
```
./flux-init-all.sh
```

## Test the Services and Data from *Within* the Container
```
# Kafka REST API
curl "http://localhost:4042/topics"

# Apache Zeppelin Web UI
curl "http://localhost:8080/topics"

# Apache Spark Master Admin Web UI
curl "http://localhost:6060"

# Apache Spark Worker Admin Web UI
curl "http://localhost:6061"

# JDBC/ODBC Hive ThriftServer
~/spark-1.4.0-bin-hadoop2.6/bin/beeline
beeline> !connect jdbc:hive2://localhost:10000 hive ''

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
```

## Test the Services and Data from *Outside* the Container
[TODO]

## Stop the Pipeline Services
```
./flux-stop-all.sh
```
Note:  Sometimes the Zookeeper Service does not shutdown.
You'll need to use `jps` and `kill` the process manually.

## Disclaimer
The end-to-end hasn't been tested, yet.
I'm putting this out so others can test and enhance.