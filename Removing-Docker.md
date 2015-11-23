### Stop Docker
```
macosx-laptop$ docker stop
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
maxosx-laptop$ boot2docker delete
```
```
#!/bin/bash
# Remove boot2docker executable
sudo rm /usr/local/bin/boot2docker

# Remove boot2docker ISO and socket files
sudo rm -rf ~/.boot2docker
sudo rm -rf /usr/local/share/boot2docker

# Remove Docker executable
sudo rm /usr/local/bin/docker

# Remove keys
sudo rm -rf ~/.ssh/id_boot2docker*
```

## Removing VirtualBox
1. Double-click on the VirtualBox dmg
2. Double-click the uninstall tool