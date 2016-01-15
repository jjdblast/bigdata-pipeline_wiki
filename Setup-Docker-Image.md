# Installing Docker
**YOU MUST BE USING DOCKER 1.7.1 OR THIS WILL NOT WORK.**

**ALSO, PLEASE KILL ANY VPN DAEMON PROCESSES OR THIS WILL LIKELY NOT WORK.**
* We have not yet upgraded to Docker 1.8.
* boot2docker + Docker 1.7 works for now, so we're sticking with it in the short-term.

**If you have been provided a USB with Docker and the Docker image, please follow [these](https://github.com/fluxcapacitor/pipeline/wiki/Setup-Docker-Image-from-USB) instructions.**

### MacOS X and Windows
* Download and install boot2docker v1.7.1 for MacOS X from [here](https://github.com/boot2docker/osx-installer/releases/tag/v1.7.1) or Windows from [here](https://github.com/boot2docker/windows-installer/releases/tag/v1.7.1)

Note:  This will install everything needed to run Docker on MacOS X or Windows including VirtualBox, boot2docker, and Docker

* Initialize and start the boot2docker VM that contains the Docker daemon as follows:

Note:  If you are planning to run a large-memory or large-disk Docker Container, you may want to bump up `--memory` and `--disksize`
```
local-macosx-or-windows$ boot2docker --memory=8192 --disksize=50000 --lowerip=127.0.0.1 --upperip=127.0.0.1 init
```
* If you see the following error:
```
Virtual machine boot2docker-vm already exists
```
* You can delete the existing `boot2docker-vm` as follows (if you're comfortable deleting it):
```
boot2docker delete boot2docker-vm
``` 
* Setup the NAT routes between boot2docker (VirtualBox VM) and local host per [this]
(https://github.com/docker/docker/issues/4007#issuecomment-34573044) link.
```
local-macosx$ <pipeline-directory>/bin/docker-setup-nat-rules.sh
...Updating NAT routes between boot2docker (VirtualBox VM) and localhost...
```
TODO:  @Windows Users - please document what works for you in this case.  Thanks!
```
local-windows$ //c/<pipeline-directory>/bin/docker-setup-nat-rules.sh
...Updating NAT routes between boot2docker (VirtualBox VM) and localhost...
```

* Start boot2docker and test if Docker is working properly
```
local-macosx-or-windows$ boot2docker start
Waiting for VM and Docker daemon to start...
........................ooooooooooo
Started.
Writing /Users/cfregly/.boot2docker/certs/boot2docker-vm/ca.pem
Writing /Users/cfregly/.boot2docker/certs/boot2docker-vm/cert.pem
Writing /Users/cfregly/.boot2docker/certs/boot2docker-vm/key.pem

To connect the Docker client to the Docker daemon, please set:
    export DOCKER_HOST=tcp://127.0.0.1:2376
    export DOCKER_CERT_PATH=/Users/cfregly/.boot2docker/certs/boot2docker-vm
    export DOCKER_TLS_VERIFY=1

Or run: `eval "$(boot2docker shellinit)"`
```
* Run the following:
```
eval "$(boot2docker shellinit)"
Writing /Users/cfregly/.boot2docker/certs/boot2docker-vm/ca.pem
Writing /Users/cfregly/.boot2docker/certs/boot2docker-vm/cert.pem
Writing /Users/cfregly/.boot2docker/certs/boot2docker-vm/key.pem
```
* Check the version
```
local-macosx-or-windows$ docker version
Client version: 1.7.1
```

**Troubleshooting**

If you see an error similar to one of the following:
```
Cannot connect to the Docker daemon. Is 'docker -d' running on this host?
...
An error occurred trying to connect: Post https://127.0.0.1:2376/v1.19/images/load: dial tcp 127.0.0.1:2376: i/o timeout
```
* There is likely a firewall (VPN) preventing the connection to Docker.
* Try shutting down the VPN and restarting your system with a clean start (and no VPN).
* Make sure you've restarted boot2docker (`stop` and `up`) after setting up the NAT routes above. 

### Linux
* Run the Docker installation script downloaded from `get.docker.com`

**This will install everything needed to run Docker on Linux**
```
local-linux$ sudo apt-get update
local-linux$ sudo apt-get -y install linux-image-extra-$(uname -r)
local-linux$ sudo sh -c "wget -qO- https://get.docker.io/gpg | apt-key add -"
local-linux$ sudo sh -c "echo deb http://get.docker.io/ubuntu docker main\ > /etc/apt/sources.list.d/docker.list"
local-linux$ sudo apt-get update
local-linux$ sudo apt-get -y install lxc-docker
local-linux$ sudo usermod -a -G docker $USER
local-linux$ exit
```
* **Log out and log back in or Docker will not work properly**

# Load the Pipeline Docker Image 

**At this point, you should have installed everything needed to run Docker**

* Next, we'll load and run the Pipeline Docker Image from the DockerHub Registry

### MacOS X or Windows or Linux
```
local-macosx-or-linux-or-windows$ docker pull fluxcapacitor/pipeline
```