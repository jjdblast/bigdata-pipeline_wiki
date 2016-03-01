# Installing Docker
**YOU MUST BE USING DOCKER 1.10+ OR THESE STEPS MAY NOT WORK.**

**ALSO, PLEASE KILL ANY VPN DAEMON PROCESSES OR THESE STEPS MAY NOT WORK.**

### Install Docker Toolbox from USB (preferred) or Internet (large download):
* USB
Install `.exe` (Windows) or `.pkg` (Mac) as appropriate

* Internet
[Mac OS X](https://docs.docker.com/mac/)
[Windows](https://docs.docker.com/windows/)
[Linux](https://docs.docker.com/linux/)



### Create VM Environment from USB (preferred) or Internet (huge download)
* Feel free to adjust the cpu, disk, and memory settings as appropriate.
* USB
```
local-laptop$ docker-machine create --driver virtualbox --virtualbox-hostonly-cidr "192.69.69.1/24" --virtualbox-cpu-count "8" --virtualbox-disk-size "100000" --virtualbox-memory "8096" --virtualbox-boot2docker-url "file:///<usb-drive>/<path-to-boot2docker.iso>" pipeline-vm
```
* Internet
```
local-laptop$ docker-machine create --driver virtualbox --virtualbox-hostonly-cidr "192.69.69.1/24" --virtualbox-cpu-count "8" --virtualbox-disk-size "100000" --virtualbox-memory "8096" pipeline-vm
```

### Setup Local Environment
* Export all of the `DOCKER_` vars below
* You may want to include these in your `.profile', `.bashrc', or `.bash_profile` file as appropriate for your environment.
```
local-laptop$ export DOCKER_TLS_VERIFY=1
local-laptop$ export DOCKER_HOST=tcp://192.69.69.100:2376
local-laptop$ export DOCKER_CERT_PATH=/Users/<username>/.docker/machine/machines/pipeline-vm
local-laptop$ export DOCKER_MACHINE_NAME="pipeline-vm"
```
* [Linux-Only] Run the following:
```
local-laptop$ sudo usermod -a -G docker $USER
local-laptop$ exit
(log back in)
```
* Check that the version is Docker 1.10+
```
local-laptop$ docker version
```

**Troubleshooting**
* If you see an error similar to one of the following:
```
Cannot connect to the Docker daemon. Is 'docker -d' running on this host?
...
Warning: failed to get default registry endpoint from daemon (Cannot connect to the Docker daemon. Is the docker daemon running on this host?). Using system default: https://index.docker.io/v1/
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```
* Make sure that you have run all of the EXPORTs listed above and try again
* Otherwise, there may be a firewall (VPN) preventing the connection to Docker.
* Try shutting down the VPN and restarting your system with a clean start (and no VPN).