# Installing Docker
**YOU MUST BE USING DOCKER 1.10+ OR THESE STEPS MAY NOT WORK.**

**ALSO, PLEASE KILL ANY VPN DAEMON PROCESSES OR THESE STEPS MAY NOT WORK.**

### Install Docker Toolbox from USB (if available) or Internet:
* USB: Install `.exe` (Windows) or `.pkg` (Mac) as appropriate

* Internet: [Mac OS X](https://docs.docker.com/mac/) or [Windows](https://docs.docker.com/windows/) or [Linux](https://docs.docker.com/linux/)

### Create VM Environment from USB (preferred) or Internet (huge download)
* USB
```
local-laptop$ docker-machine create --driver virtualbox --virtualbox-hostonly-cidr "192.69.69.1/24" --virtualbox-cpu-count "8" --virtualbox-disk-size "100000" --virtualbox-memory "8096" --virtualbox-boot2docker-url "file:///<usb-drive>/<path-to-boot2docker.iso>" pipeline-vm
```
* Internet
```
local-laptop$ docker-machine create --driver virtualbox --virtualbox-hostonly-cidr "192.69.69.1/24" --virtualbox-cpu-count "8" --virtualbox-disk-size "100000" --virtualbox-memory "8096" pipeline-vm
```

### Setup NAT Rules [**Mac OSX and Windows Only**]
* This step bridges your local laptop at `127.0.0.1` to the VirtualBox VM created by `docker-machine` called `pipeline-vm`.
* ONLY RUN THIS ON MAC OSX OR WINDOWS
* Run the following script available in this same github repo
```
local-mac-or-windows$ <pipeline-home-in-github>/bin/docker/docker-setup-nat-rules.[sh|bat]
```

### Setup Local Environment
* Run the following
```
local-laptop$ docker-machine env pipeline-vm
local-laptop$ eval $(docker-machine env pipeline-vm)
```
* Add the following EXPORTs (from the commands above) as environment variables
```
# Add these to .profile, .bashrc, or .bash_profile as appropriate for your environment
export DOCKER_TLS_VERIFY=1
export DOCKER_HOST=tcp://192.69.69.100:2376
export DOCKER_CERT_PATH=<user-home-dir>/.docker/machine/machines/pipeline-vm
export DOCKER_MACHINE_NAME="pipeline-vm"
```

* [**Linux-Only**] Run the following:
```
# Only run this for Linux environments
local-linux-only$ sudo usermod -a -G docker $USER
local-linux-only$ exit
(log back in)
```

* Check that the version is Docker 1.10+
```
local-laptop$ docker version
```

## Troubleshooting
```
Error with pre-create check: "VirtualBox is configured with multiple host-only adapters with the same IP
```
* Run the following commands (vboxnet0..n) until you remove the conflicting VirtualBox network adapters
```
# ONLY RUN THIS IF YOU SEE THE ERROR ABOVE
VBoxManage hostonlyif remove vboxnet0
VBoxManage hostonlyif remove vboxnet1
VBoxManage hostonlyif remove vboxnet2
VBoxManage hostonlyif remove vboxnet3
...
<until you have removed all existing vboxnet's>
```
* Then re-run the appropriate `docker-machine create` command above.

```
Host already exists: "pipeline-vm"
```
* Run the following and re-run the `docker-machine create` command above
```
local-laptop$ docker-machine kill pipeline-vm
local-laptop$ docker-machine rm pipeline-vm
Are you sure? (y/n): y
Successfully removed pipeline-vm
```

```
Cannot connect to the Docker daemon. Is 'docker -d' running on this host?
...
Warning: failed to get default registry endpoint from daemon (Cannot connect to the Docker daemon. Is the docker daemon running on this host?). Using system default: https://index.docker.io/v1/
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```
* Make sure that you have run all of the EXPORTs listed above and try again
* Otherwise, there may be a firewall (VPN) preventing the connection to Docker.
* Try shutting down the VPN and restarting your system with a clean start (and no VPN).

```
VBoxManage: error: Context: "LockMachine(a->session, LockType_Write)" at line 493 of file VBoxManageModifyVM.cpp
VBoxManage: error: The machine 'pipeline-vm' is already locked for a session (or being unlocked)
VBoxManage: error: Details: code VBOX_E_INVALID_OBJECT_STATE (0x80bb0007), component MachineWrap, interface IMachine, callee nsISupports
```
* Run the following
```
local-laptop$ docker-machine stop pipeline-vm
```
* Re-run the `docker-setup-nat-rules.sh` script 
* Restart the `pipeline-vm`
```
local-laptop$ docker-machine start pipeline-vm
```