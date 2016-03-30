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
* USB
```
local-laptop$ docker-machine create --driver virtualbox --virtualbox-hostonly-cidr "192.69.69.1/24" --virtualbox-cpu-count "8" --virtualbox-disk-size "100000" --virtualbox-memory "8096" --virtualbox-boot2docker-url "file:///<usb-drive>/<path-to-boot2docker.iso>" pipeline-vm
```
* Internet
```
local-laptop$ docker-machine create --driver virtualbox --virtualbox-hostonly-cidr "192.69.69.1/24" --virtualbox-cpu-count "8" --virtualbox-disk-size "100000" --virtualbox-memory "8096" pipeline-vm
```

* [**Mac and Windows Only**] Setup NAT Rules 

This step bridges your local laptop at `127.0.0.1` to the VirtualBox VM created by `docker-machine` called `pipeline-vm`.
```
local-laptop$ <pipeline-home-dir>/bin/docker-setup-nat-rules.sh
```

**Troubleshooting:  If you see the following error:**
```
Error with pre-create check: "VirtualBox is configured with multiple host-only adapters with the same IP
```
**You may need to run the following commands until you remove the conflicting virtualbox network adapters**
```
# ONLY RUN THIS IF YOU SEE THE ERROR ABOVE
VBoxManage hostonlyif remove vboxnet0
VBoxManage hostonlyif remove vboxnet1
VBoxManage hostonlyif remove vboxnet2
VBoxManage hostonlyif remove vboxnet3
...
<until you have removed all existing vboxnet's>
```
**Then re-run the appropriate `docker-machine create` command above.**

**Troubleshooting:  If you see the following error:**
```
Host already exists: "pipeline-vm"
```
**Then run the following**
```
local-laptop$ docker-machine kill pipeline-vm
local-laptop$ docker-machine rm pipeline-vm
Are you sure? (y/n): y
Successfully removed pipeline-vm
```

### Setup Local Environment
* Run the following
```
local-laptop$ docker-machine env pipeline-vm
local-laptop$ eval $(docker-machine env pipeline-vm)
```
* Add the EXPORTs (from the commands above) to your `.profile', `.bashrc`, or `.bash_profile` file as appropriate for your environment.
```
local-laptop$ export DOCKER_TLS_VERIFY=1
local-laptop$ export DOCKER_HOST=tcp://192.69.69.100:2376
local-laptop$ export DOCKER_CERT_PATH=<user-home-dir>/.docker/machine/machines/pipeline-vm
local-laptop$ export DOCKER_MACHINE_NAME="pipeline-vm"
```

* [**Linux-Only**] Run the following:
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