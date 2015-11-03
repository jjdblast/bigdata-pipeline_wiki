# Installing Docker
* We have not yet upgraded to Docker 1.8.  
* boot2docker + Docker 1.7 works for now, so we're sticking with it in the short-term.
**If you have been provided a USB with Docker and the Docker image, please follow [these](https://github.com/fluxcapacitor/pipeline/wiki/Install-Docker-and-Load-the-Docker-Image-from-USB) instructions.**

### MacOS X and Windows
* Download and install boot2docker v1.7.1 for MacOS X from [here](https://github.com/boot2docker/osx-installer/releases/tag/v1.7.1) or Windows from [here](https://github.com/boot2docker/windows-installer/releases/tag/v1.7.1)
**This will install everything including VirtualBox, boot2docker, and Docker**
* Initialize and start the boot2docker VM that contains the Docker daemon as follows:
**If you are planning to run a large-memory or large-disk Docker Container, you may want to bump up `--memory` and `--disksize`**
```
local-macosx-or-windows$ boot2docker --memory=8192 --disksize=20000 init
local-macosx-or-windows$ boot2docker up
```

### Linux
* Run the Docker installation script downloaded from `get.docker.com`
**This will install everything you need to run Docker on Linux**
```
local-linux$ wget -qO- https://get.docker.com/ | sh
```
* Add your user to the `docker` group to avoid having to 
```
local-linux$ sudo usermod -aG docker ubuntu
```
* **Log out and log back in or the changes will not take effect**

# Load the Pipeline Docker Image 
* At this point, you should have boot2docker and/or Docker installed on your laptop.
* Next, we'll load and run the Pipeline Docker Image

### MacOS X or Windows or Linux
```
local-macosx-or-windows-or-linux$ docker pull fluxcapacitor/pipeline
``` 

# Verify the Pipeline Docker Image was Loaded Successfully
### MacOS X or Windows or Linux
```
local-macosx-or-windows-or-linux$ docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
fluxcapacitor/pipeline   latest              b62e5d6e385f        36 hours ago        3.653 GB
```

# Run the Docker Container using the Loaded Image
* **THE RUN COMMAND IS VERY LONG GIVEN ALL OF THE `-p` PORT MAPPINGS.  BE SURE TO COPY/PASTE ALL OF IT!**
* If running on a large-memory host, you may want to increase the `-m 8g` value to something higher.
* If you plan to mimic my live presentation demo, you may want to change the `-p 30080:80` http port mapping to `-p 80:80`.
** Obviously, you still need to setup CNAME's and customize `config/apache2` and `config/fluxcapacitor` Apache Httpd configs.
* Contact me with any questions or create a Github issue and I'll answer as quickly as possible.

### MacOS X or Linux
```
local-macosx-or-linux$ docker run -it --privileged --name pipeline -h docker -m 8g -v ~/pipeline/notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 36379:6379 -p 38888:8888 -p 34321:54321 -p 38099:8099 fluxcapacitor/pipeline bash
```
Note:  Ignore this message if you see it.
```
WARNING: Your kernel does not support swap limit capabilities, memory limited without swap.
```

### Windows
```
local-windows$ docker run -it --privileged --name pipeline -h docker -m 8g -v %USERPROFILE%\notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 36379:6379 -p 38888:8888 -p 34321:54321 -p 38099:8099 fluxcapacitor/pipeline bash
```