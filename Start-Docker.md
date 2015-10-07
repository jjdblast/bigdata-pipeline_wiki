## USB-based Installation

### MacOS X 
* Using MacOS X Finder, copy the contents of the USB to the `~` home directory on your local laptop
* Run the `~/Boot2Docker-1.7.1.pkg` file to complete the installation of boot2docker
* Initialize and start the boot2docker VM that contains the Docker daemon
```
local-macosx$ boot2docker --iso=~/pipeline/boot2docker.iso --memory=8192 --disksize=20000 init
local-macosx$ boot2docker up
```

### Windows 
* Using Windows Explorer, copy the contents of the USB to the `%USERPROFILE%` home directory on your local laptop
* Run the `%USERPROFILE%\docker-install.exe` file that was copied into your home directory from the USB and complete the installation of boot2docker.
* Initialize and start the boot2docker VM that contains the Docker daemon
```
local-windows$ boot2docker --iso=%USERPROFILE%\pipeline\boot2docker.iso --memory=8192 --disksize=20000 init
local-windows$ boot2docker up
```

### Linux
* Copy the contents of the USB to the `~` home directory on your local laptop
* Run the Docker installation script that was copied into your home directory from the USB.
```
local-linux$ ~\docker-install-linux.sh
```
* Add your user to the `docker` group to avoid having to 
```
local-linux$ sudo usermod -aG docker ubuntu
```
* **Log out and log back in or the changes will not take effect**

## Internet-based Installation

### MacOS X

* Download and install boot2docker v1.7.1 for MacOS X from [here](https://github.com/boot2docker/osx-installer/releases/tag/v1.7.1)
* This will install everything including VirtualBox, boot2docker, and Docker
* Initialize and start the boot2docker VM that contains the Docker daemon
```
local-macosx$ boot2docker --memory=8192 --disksize=20000 init
local-macosx$ boot2docker up
```

### Windows
* Download and install the latest boot2docker for Windows from [here](https://github.com/boot2docker/windows-installer/releases/tag/v1.7.1)
* This will install everything including VirtualBox, boot2docker, and Docker
* Initialize and start the boot2docker VM that contains the Docker daemon
```
local-windows$ boot2docker --memory=8192 --disksize=20000 init
local-windows$ boot2docker up
```

### Linux
* Run the Docker installation script downloaded from `get.docker.com`
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

## USB-based Installation

### MacOS X or Linux
```
local-macosx-or-linux$ docker load < ~/pipeline/fluxcapacitor-pipeline.tar
``` 
* Allow any terminal to run Docker commands
```
eval "$(boot2docker shellinit)"
``` 

### Windows 
```
local-windows$ docker load < %USERPROFILE%\pipeline\fluxcapacitor-pipeline.tar
``` 

## Internet-based installation

### MacOS X or Windows or Linux
```
local-macosx-or-windows-or-linux$ docker pull fluxcapacitor/pipeline
``` 

# Verify the Pipeline Docker Image was Loaded Successfully
## USB-based or Internet-based
### MacOS X or Windows or Linux
```
local-macosx-or-windows-or-linux$ docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
fluxcapacitor/pipeline   latest              b62e5d6e385f        36 hours ago        3.653 GB
```

# Run the Docker Container
## USB-based or Internet-based
### MacOS X or Linux
**THIS A VERY LONG COMMAND.  BE SURE TO COPY/PASTE ALL OF IT!**
```
local-macosx-or-linux$ docker run -it -h docker -m 8g -v ~/pipeline/notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 36379:6379 -p 38888:8888 -p 34321:54321 -p 38099:8099 fluxcapacitor/pipeline bash
```

Note:  Ignore this message if you see it.
```
WARNING: Your kernel does not support swap limit capabilities, memory limited without swap.
```
### Windows
**THIS A VERY LONG COMMAND.  BE SURE TO COPY/PASTE ALL OF IT!**
```
local-windows$ docker run -it -h docker -m 8g -v %USERPROFILE%\notebooks:/root/pipeline/notebooks -p 30080:80 -p 34042:4042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060 -p 36061:6061 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 36379:6379 -p 38888:8888 -p 34321:54321 -p 38099:8099 fluxcapacitor/pipeline bash
```

# Building a New Docker Image
* For more info, check out this [doc](https://github.com/fluxcapacitor/pipeline/wiki/Build-Docker-Image).