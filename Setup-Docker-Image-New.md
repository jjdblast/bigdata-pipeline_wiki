# Installing Docker
**YOU MUST BE USING DOCKER 1.10+ OR THESE STEPS MAY NOT WORK.**

**ALSO, PLEASE KILL ANY VPN DAEMON PROCESSES OR THESE STEPS MAY NOT WORK.**

### Download and Install Docker Toolbox
* [Mac OS X](https://docs.docker.com/mac/)
* [Windows](https://docs.docker.com/windows/)
* [Linux](https://docs.docker.com/linux/)

### Create VM Environment (VirtualBox) 
* Feel free to adjust the cpu, disk, and memory settings as appropriate.
* Also, if you have been provided a USB, please modify the `--virtualbox-boot2docker-url` param value to include to point to the `file://<usb-drive>/<path-to-boot2docker.iso>` on the USB.
```
create --driver virtualbox --virtualbox-hostonly-cidr "127.0.0.1/24" --virtualbox-cpu-count "8" --virtualbox-disk-size "100000" --virtualbox-memory "8096" --virtualbox-boot2docker-url "" pipeline-vm
```

### Setup Local Environment
* To connect the Docker client to the Docker daemon, please export the `DOCKER_` vars below.
* You may want to include these in your `.profile', `.bashrc', or `.bash_profile` file as appropriate for your environment.
```
export DOCKER_HOST=tcp://127.0.0.1:2376
export DOCKER_CERT_PATH=/Users/<username>/.boot2docker/certs/boot2docker-vm
export DOCKER_TLS_VERIFY=1
```
* Run the following:
```
sudo usermod -a -G docker $USER
exit
```
* Check that the version is Docker 1.10+
```
local-macosx-or-windows$ docker version
```

**Troubleshooting**
* If you see an error similar to one of the following:
```
Cannot connect to the Docker daemon. Is 'docker -d' running on this host?
...
An error occurred trying to connect: Post https://127.0.0.1:2376/v1.19/images/load: dial tcp 127.0.0.1:2376: i/o timeout
```
* There is likely a firewall (VPN) preventing the connection to Docker.
* Try shutting down the VPN and restarting your system with a clean start (and no VPN).

### Pull Latest fluxcapacitor/pipeline Docker Image
```
local-macosx-or-linux-or-windows$ docker pull fluxcapacitor/pipeline
```