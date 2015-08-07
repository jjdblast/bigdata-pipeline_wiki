## From USB
* Copy `/pipeline` folder to your home directory
```
macosx-laptop$ cp -R /Volumes/USB20FD ~/
```
* Install boot2docker
```
macosx-laptop$ cd ~/pipeline
macosx-laptop$ open Boot2Docker-1.7.1.pkg
```
Setup boot2docker completely using installation wizard.
* Setup and Start the Pipeline Docker Container
```
macosx-laptop$ boot2docker --iso=boot2docker.iso --memory=8192 --disksize=20000 init
macosx-laptop$ boot2docker up
macosx-laptop$ docker load < fluxcapacitor-pipeline.tar
[TODO:  Add Tag]
macosx-laptop$ docker pull fluxcapacitor/pipeline
``` 

* Verify that the Docker Image is Loaded
```
macosx-laptop$ docker images
```
* Run the Docker Container
**THIS A VERY LONG COMMAND.  BE SURE TO COPY/PASTE ALL OF IT!**
```
macosx-laptop$ docker run -it -m 8g -v ~/pipeline/notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 36379:6379 fluxcapacitor/pipeline bash
```

## From Internet
* Download and Install latest boot2docker (1.7+) from [the boot2docker website](http://boot2docker.io/)
* This will install everything including VirtualBox, boot2docker, and Docker
* Initialize boot2docker as follows
```
macosx-laptop$ boot2docker --memory=8192 --disksize=20000 init
``` 
* Export Docker Variables to your Terminals
```
eval "$(boot2docker shellinit)"
``` 
* Load Docker Image from the Internet (DockerHub Registry)
```
macosx-laptop$ docker pull fluxcapacitor/pipeline
```
* Verify that the Docker Image is Loaded
```
macosx-laptop$ docker images
```
* Run the Docker Container
**THIS A VERY LONG COMMAND.  BE SURE TO COPY/PASTE ALL OF IT!**
```
macosx-laptop$ docker run -it -m 8g -v ~/pipeline/notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 36379:6379 fluxcapacitor/pipeline bash
```