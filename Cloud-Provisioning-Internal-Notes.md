# Internal Notes for Cloud Instance Provisioning
## GCE Launch Script/User Data
```
#!/bin/bash
sudo apt-get update
sudo apt-get install curl
curl -fsSL https://get.docker.com/ | sh
curl -fsSL https://get.docker.com/gpg | sudo apt-key add -
sudo usermod -a -G docker $USER
echo 'DOCKER_OPTS="--ip=127.0.0.1"' | sudo tee -a /etc/default/docker
echo "alias docker='sudo docker'" > ~/.bash_aliases
source ~/.bash_aliases
sudo service docker restart
docker pull fluxcapacitor/pipeline
```