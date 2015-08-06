### Stop the Pipeline Services
* The following must be done within the Docker Container.
```
root@[docker]$ ./flux-stop.sh
```

### Stop the Docker Container
* From outside the Docker Container either ec2-linux or macosx-laptop, do the following
```
macosx-laptop$ docker ps

...copy the [container-instance-id]...

macosx-laptop$ docker stop [container-instance-id]
```

### Stop boot2docker 
* The following will stop the boot2docker VirtualBox VM docker daemon.
```
macosx-laptop$ boot2docker stop
```

### [Optional] Remove boot2docker 
* You can also tear down boot2docker completely
* This is very destructive and not recommended if you continue to explore this project 
```
maxosx-laptop$ boot2docker destroy
```