## EC2 Provisioning
* User Data (per instance)
```
#!/bin/bash
sudo apt-get update
sudo apt-get install curl
curl -fsSL https://get.docker.com/ | sh
curl -fsSL https://get.docker.com/gpg | sudo apt-key add -
docker run hello-world
docker pull fluxcapacitor/pipeline)
# TODO:  full docker run of container with 15 GB!
```