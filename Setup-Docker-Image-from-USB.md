# USB-based Installation
**THIS ONLY APPLIES DURING A TRAINING CLASS WHERE YOU'VE BEEN GIVEN A USB!!**

**OTHERWISE, FOLLOW [THESE](https://github.com/fluxcapacitor/pipeline/wiki/Setup-Docker-Image) INSTRUCTIONS.**

# Installing Docker
**YOU MUST BE USING DOCKER 1.7.1 OR THIS WILL NOT WORK.**

**ALSO, PLEASE KILL ANY VPN DAEMON PROCESSES OR THIS WILL LIKELY NOT WORK.**

## MacOS X or Windows 
### Copy USB `fluxcapacitor` folder to Laptop Home Directory 
* MacOS X, Linux:  `~`
* Windows:  `%USERPROFILE%`

**Remove the USB**

### Setup boot2docker
* If VirtualBox or boot2docker has been installed already, this will cause a problem.
* The best route is to uninstall VirtualBox and boot2docker before proceeding

Run one of the following (MacOS and Windows Only):
* MacOS
```
local-macosx$ open ~/fluxcapacitor/docker/mac/Boot2Docker-1.7.1.pkg
``` 
* Windows
```
windows> %USERPROFILE%\fluxcapacitor\docker\win\docker-install.exe
```

**This will install everything needed to run Docker including VirtualBox, boot2docker, and Docker**

### Start boot2Docker

**If you are planning to run a large-memory or large-disk Docker Container, you may want to bump up `--memory` and `--disksize`**

* Mac OS
```
local-macosx$ cd ~
local-macosx$ boot2docker --iso=fluxcapacitor/docker/boot2docker.iso --memory=8192 --disksize=20000 --lowerip=127.0.0.1 --upperip=127.0.0.1 init

local-macosx$ vboxmanage modifyvm "boot2docker-vm" --natpf1 "docker,tcp,127.0.0.1,2376,,2376"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "apache,tcp,127.0.0.1,30080,,30080"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "34042,tcp,127.0.0.1,34042,,34042"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "39160,tcp,127.0.0.1,39160,,39160"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "39042,tcp,127.0.0.1,39042,,39042"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "39200,tcp,127.0.0.1,39200,,39200"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "37077,tcp,127.0.0.1,37077,,37077"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "38080,tcp,127.0.0.1,38080,,38080"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "38081,tcp,127.0.0.1,38081,,38081"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "36060,tcp,127.0.0.1,36060,,36060"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "36061,tcp,127.0.0.1,36061,,36061"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "32181,tcp,127.0.0.1,32181,,32181"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "38090,tcp,127.0.0.1,38090,,38090"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "30000,tcp,127.0.0.1,30000,,30000"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "30070,tcp,127.0.0.1,30070,,30070"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "30090,tcp,127.0.0.1,30090,,30090"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "39092,tcp,127.0.0.1,39092,,39092"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "36066,tcp,127.0.0.1,36066,,36066"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "39000,tcp,127.0.0.1,39000,,39000"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "39999,tcp,127.0.0.1,39999,,39999"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "35601,tcp,127.0.0.1,35601,,35601"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "37979,tcp,127.0.0.1,37979,,37979"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "38989,tcp,127.0.0.1,38989,,38989"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "34040,tcp,127.0.0.1,34040,,34040"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "36379,tcp,127.0.0.1,36379,,36379"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "38888,tcp,127.0.0.1,38888,,38888"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "34321,tcp,127.0.0.1,34321,,34321"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "38099,tcp,127.0.0.1,38099,,38099"
vboxmanage modifyvm "boot2docker-vm" --natpf1 "37777,tcp,127.0.0.1,37777,,37777"

local-macosx$ boot2docker up
local-macosx$ eval "$(boot2docker shellinit)"
local-macosx$ docker version
```
**Troubleshooting**

If you see an error similar to one of the following:
```
Cannot connect to the Docker daemon. Is 'docker -d' running on this host?
```
```
An error occurred trying to connect: Post https://127.0.0.1:2376/v1.19/images/load: dial tcp 127.0.0.1:2376: i/o timeout
```
There is likely a firewall (VPN) preventing the connection to Docker.

Try shutting down the VPN and restarting your system with a clean start (and no VPN).

* Windows:
```
local-windows> cd %USERPROFILE%
local-windows> boot2docker --iso=fluxcapacitor\docker\boot2docker.iso --memory=8192 --disksize=20000 --lowerip=127.0.0.1 --upperip=127.0.0.1 init
local-windows> boot2docker up
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
``` 

### Windows 
```
local-windows> docker load < %USERPROFILE%\fluxcapacitor\pipeline\fluxcapacitor-pipeline.tar
``` 