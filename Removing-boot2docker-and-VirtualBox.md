## Removing boot2docker
```
#!/bin/bash

# Stop boot2docker processes
boot2docker stop
boot2docker delete

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