# Installing Docker
* We have not yet upgraded to Docker 1.8.  
* boot2docker + Docker 1.7 works for now, so we're sticking with it in the short-term.

**If you have been provided a USB with Docker and the Docker image, please follow [these](https://github.com/fluxcapacitor/pipeline/wiki/Setup-from-USB-Docker-1.7) instructions.**

### MacOS X and Windows
* Download and install boot2docker v1.7.1 for MacOS X from [here](https://github.com/boot2docker/osx-installer/releases/tag/v1.7.1) or Windows from [here](https://github.com/boot2docker/windows-installer/releases/tag/v1.7.1)

**This will install everything needed to run Docker on MacOS X or Windows including VirtualBox, boot2docker, and Docker**

* Initialize and start the boot2docker VM that contains the Docker daemon as follows:

**If you are planning to run a large-memory or large-disk Docker Container, you may want to bump up `--memory` and `--disksize`**

```
local-macosx-or-windows$ boot2docker --memory=8192 --disksize=20000 --lowerip=127.0.0.1 --upperip=127.0.0.1 init

local-macosx-or-windows$ vboxmanage modifyvm "boot2docker-vm" --natpf1 "docker,tcp,127.0.0.1,2376,,2376"
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

local-macosx-or-windows$ boot2docker up
local-macosx-or-windows$ docker version
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

### Linux
* Run the Docker installation script downloaded from `get.docker.com`

**This will install everything needed to run Docker on Linux**
```
local-linux$ wget -qO- https://get.docker.com/ | sh
```
* Add your user to the `docker` group to avoid having to 
```
local-linux$ sudo usermod -aG docker ubuntu
```
* **Log out and log back in or the changes will not take effect**

# Load the Pipeline Docker Image 

**At this point, you should have installed everything needed to run Docker**

* Next, we'll load and run the Pipeline Docker Image

### MacOS X or Windows or Linux
```
local-macosx-or-linux-or-windows$ docker pull fluxcapacitor/pipeline
```