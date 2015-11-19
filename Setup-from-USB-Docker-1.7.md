# USB-based Installation
**THIS ONLY APPLIES DURING A TRAINING CLASS WHERE YOU'VE BEEN GIVEN A USB!!**

**OTHERWISE, FOLLOW [THESE](https://github.com/fluxcapacitor/pipeline/wiki/Start-Docker) INSTRUCTIONS.**

# Installing Docker
## MacOS X or Windows 
### Copy USB `fluxcapacitor` folder to Laptop Home Directory 
* MacOS X, Linux:  `~`
* Windows:  `%USERPROFILE%`

**Remove the USB**

### Setup boot2Docker
* MacOS:  `~/fluxcapacitor/docker/mac/Boot2Docker-1.7.1.pkg` 
* Windows:  `%USERPROFILE%\fluxcapacitor\docker\win\docker-install.exe`

**This will install everything needed to run Docker including VirtualBox, boot2docker, and Docker**

### Start boot2Docker

**If you are planning to run a large-memory or large-disk Docker Container, you may want to bump up `--memory` and `--disksize`**

* Mac OS:
```
local-macosx$ boot2docker --iso=~/fluxcapacitor/docker/boot2docker.iso --memory=8192 --disksize=20000 init
local-macosx$ boot2docker up
```
* Windows:
```
local-windows$ boot2docker --iso=%USERPROFILE%\fluxcapacitor\docker\boot2docker.iso --memory=8192 --disksize=20000 init
local-windows$ boot2docker up
```

## Linux
### Copy USB Contents to Home Directory 
* Linux:  `~`

**Remove the USB**

### Setup Docker
* **This will install everything needed to run Docker on Linux**
```
local-linux$ ~/fluxcapacitor/docker/linux/docker-install-linux.sh
```
* Add your user to the `docker` group to avoid having to 
```
local-linux$ sudo usermod -aG docker ubuntu
```
* **Log out and log back in or the changes will not take effect**

# Load the Pipeline Docker Image 
* At this point, you should have installed everything needed to run Docker
* Next, we'll load prepare the Pipeline Docker Image for use

### MacOS X or Linux
```
local-macosx-or-linux$ docker load < ~/fluxcapacitor/pipeline/fluxcapacitor-pipeline.tar
local-macosx-or-linux$ docker pull fluxcapacitor/pipeline
``` 
* Allow any terminal to run Docker commands
```
eval "$(boot2docker shellinit)"
``` 

### Windows 
```
local-windows$ docker load < %USERPROFILE%\fluxcapacitor\pipeline\fluxcapacitor-pipeline.tar
``` 

### Start Docker [Here](https://github.com/fluxcapacitor/pipeline/wiki/Start-Docker)