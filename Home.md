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

## Option 2:  Download the Image (2-3GB) from the Docker Hub Repo
```docker pull fluxcapacitor/pipeline```

## Run Docker Container with the Image and Get a Bash Prompt within the Container
```
docker run -p 30080:80 -p 32181:2181 -p 38082:8082 -p 39042:9042 -p 39092:9092 -p -p 39160:9160 39200:9200 -p 37070:7070 -p 37077:7077 -p 38080:8080 -p 38081:8081 -p 38090:8090 -it fluxcapacitor/pipeline bash
```

## Ports
In order to reduce the likelihood of port collisions on your local machine, I've mapped the somewhat-common container service ports to uncommon ports in the `30000` range as follows:
```
Apache Httpd (80):  30080
Apache ZooKeeper (2181):  32181
Apache Kafka Rest Proxy (8082):  38082
Apache Kafka (9092):  39092
Apache Cassandra (9042, 9160):  39042, 39160
ElasticSearch (9200):  39200
Apache Zeppelin (7070):  37070
Apache Spark Master (7077):  37077
Apache Spark Master Admin UI (8080):  38080
Apache Spark Worker Admin UI (8081):  38081
```

## Accessing Services Outside of the Container
You'll need to use the 30000+ ports listed above to access services outside of the Docker container.

## Start the Pipeline Services 

```
cd pipeline
flux-start-all.sh
```

## Initialize the Pipeline Data

```
flux-init-all.sh
```

## Stop the Pipeline Services

```
flux-stop-all.sh
```

## Disclaimer
The end-to-end hasn't been tested, yet.
I'm putting this out so others can test and enhance.