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

## Create an Image from the Dockerfile provided in this repo (or download from Docker Hub)
```
git clone https://github.com/fluxcapacitor/pipeline.git
cd pipeline
docker build -t fluxcapacitor/pipeline .
```

## Run Docker Container with the Image and Get a Bash Prompt within the Container
```
docker run -p 30080:80 -p 32181:2181 -p 38082:8082 -p 39092:9092 -p 39200:9200 -p 37070:7070 -p 37077:7077 -p 38080:8080 -p 38081:8081 -p 38090:8090 -it fluxcapacitor/pipeline bash
```

## Start the Pipeline Services
```
cd pipeline
flux-start-all.sh
```

## Initialize the Pipeline Data
```
flux-init-all.sh
```

## Disclaimer
The end-to-end hasn't been tested, yet.
I'm putting this out so others can test.