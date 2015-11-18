# USB Builder for Docker 1.8
**Work In Progress.  This Does Not Currently Work!!**

## Prep the USB Stick
** The paths to everything are relative to the `usbstick/` path**
** Make sure you are in `usbstick/` before running any scripts**

*  Download the Dependencies 
```
./download-deps.sh
```
* Copy the Dependencies onto the USB Stick
```
TODO
```

* Pull the Docker image from DockerHub
**This assumes you already have Docker installed**
```
$local-laptop$ docker pull fluxcapacitor/pipeline
$local-laptop$ docker save fluxcapacitor/pipeline > fluxcapacitor-pipeline.tar
```

# Install Docker Locally using usbstick
If you do not have virtualbox installed already it should be on the usbstick.  Please install that first so you don't need to download it from the internet as bandwidth is limited.

##OSX
Run this pkg 

```sh
osx/DockerToolbox-1.8.0a.pkg
```

After you have installed the docker binaries using the .pkg file you can create the docker-machine you need.

This script will do everything for you.

```sh
./install-pipeline.sh
```

After the script creates a docker-machine called fluxcapacitor-pipeline with 5gb of ram allocated you will need to setup your shell environment by running 

```sh
eval "$(docker-machine env fluxcapacitor-pipeline)"
```

If you need the external IP address of your docker-machine to access services on it you can use this command.

```sh
docker-machine ip fluxcapacitor-pipeline
```

Another example of using it in a script might be.  Since I have a bash alias to open chrome. This opens the spark notebook in crhome.

```sh
alias openchrome='/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --kiosk'
openchrome http://$(docker-machine ip fluxcapacitor-pipeline):39000
```

###Of note

The installer will work offline but will throw an error at the end.

## VPN USERs

* There is a known issue with the Cisco AnyConnect client screwing up the networking in VirtualBox.  
* The workaround that has worked best for me is to use OpenConnect instead of AnyConnect.  
* There is a brew formula for OpenConnect
```
brew install openconnect
```
OpenConnect requires that you have the [OSX tap/tun](http://tuntaposx.sourceforge.net/) driver installed beforehand.

[Install OpenConnect instructions](https://gist.github.com/moklett/3170636)

## Windows
```
DockerToolbox-1.8.0a.exe
```

## Linux
```
curl -sSL https://get.docker.com/ | sh
```