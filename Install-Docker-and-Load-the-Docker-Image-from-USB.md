# USB-based Installation
* **THIS ONLY APPLIES DURING A TRAINING CLASS WHERE YOU'VE BEEN GIVEN A USB!!**

# Installing Docker
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

# Load the Pipeline Docker Image 
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
