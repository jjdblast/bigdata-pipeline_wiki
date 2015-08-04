## Troubleshooting Common Errors

### Running out of disk space
* It's likely that you have old, unused containers from each `docker run` command
* These don't get garbage collected automatically as Docker assumes you may want to start them again
* Use the following command to clean them out
```
docker rm `docker ps -aq`
```

NOTE: If docker fills up the root partition of your VM, then docker daemon might not start, in which case running any docker commands will tell you that the daemon is not running.  

1. Confirm out of disk space using `df -l`
2. Blow away the Docker working dirs `sudo rm -rf /var/lib/docker`
3. Pull the pipeline image again and start over.

### Machine "boot2docker-vm" does not exist.
* Run the following to repair your busted boot2docker:
```
macosx-laptop$ sudo /Library/Application\ Support/VirtualBox/LaunchDaemons/VirtualBoxStartup.sh restart
```
* More docs [here](https://github.com/boot2docker/boot2docker#boot2docker-up-doesnt-work-osx)

### Are you trying to connect to a TLS-enabled daemon without TLS?  
* Make sure you've run the following
```
eval $(boot2docker shellinit)
```
You should also add the following exports the following in your MacOS X `~/.bash_profile`
```
export DOCKER_TLS_VERIFY=1
export DOCKER_HOST=tcp://192.168.59.103:2376
export DOCKER_CERT_PATH=/Users/[YOUR_USERNAME]/.boot2docker/certs/boot2docker-vm
```
