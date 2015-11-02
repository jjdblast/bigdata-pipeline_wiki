# Internet-based Installation

## Building a New Docker Image

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

## Load the Pipeline Docker Image 
* At this point, you should have boot2docker and/or Docker installed on your laptop.
* Next, we'll load and run the Pipeline Docker Image

### MacOS X or Windows or Linux
```
local-macosx-or-windows-or-linux$ docker pull fluxcapacitor/pipeline
``` 

# USB-based Installation (THIS ONLY APPLIES DURING A TRAINING CLASS WHERE YOU'VE BEEN GIVEN A USB!!)

## Building a New Docker Image
* For more info, check out this [doc](https://github.com/fluxcapacitor/pipeline/wiki/Build-Docker-Image).

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

## Load the Pipeline Docker Image 
* At this point, you should have boot2docker and/or Docker installed on your laptop.
* Next, we'll load and run the Pipeline Docker Image

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

## Verify the Pipeline Docker Image was Loaded Successfully
### MacOS X or Windows or Linux
```
local-macosx-or-windows-or-linux$ docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
fluxcapacitor/pipeline   latest              b62e5d6e385f        36 hours ago        3.653 GB
```
