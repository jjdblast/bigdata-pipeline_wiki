# USB-based Installation
* **THIS ONLY APPLIES DURING A TRAINING CLASS WHERE YOU'VE BEEN GIVEN A USB!!**

# Installing Docker
### MacOS X or Windows 
* Copy the contents of the USB to the **home** directory (Linux: `~`, Windows: `%USERPROFILE%` on your local machine

**Remove (and return or pass on) the USB to make sure you're not running anything off of the USB from this point on**

* Run `~/Boot2Docker-1.7.1.pkg` or `%USERPROFILE%\docker-install.exe` to complete the installation of boot2docker

**This will install everything needed to run Docker including VirtualBox, boot2docker, and Docker**

* Initialize and start the boot2docker VM that contains the Docker daemon as follows:

**If you are planning to run a large-memory or large-disk Docker Container, you may want to bump up `--memory` and `--disksize`**

* Linux:
```
local-macosx$ boot2docker --iso=~/pipeline/boot2docker.iso --memory=8192 --disksize=20000 init
local-macosx$ boot2docker up
```
* Windows:
```
local-windows$ boot2docker --iso=%USERPROFILE%\pipeline\boot2docker.iso --memory=8192 --disksize=20000 init
local-windows$ boot2docker up
```

### Linux
* Copy the contents of the USB to the `~` home directory on your local laptop

**Remove (and return or pass on) the USB to make sure you're not running anything off of the USB from this point on**

* Run the Docker installation script in your home directory 

**This will install everything needed to run Docker on Linux**

```
local-linux$ ~\docker-install-linux.sh
```
* Add your user to the `docker` group to avoid having to 
```
local-linux$ sudo usermod -aG docker ubuntu
```
* **Log out and log back in or the changes will not take effect**

# Load the Pipeline Docker Image 
* At this point, you should have installed everything needed to run Docker
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
