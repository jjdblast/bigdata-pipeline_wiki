# Internal Notes for Cloud Instance Provisioning
## GCE "Launch Script" (aka. AWS "User Data")
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

# End of Workshop Fun
## Build a Gigantic Spark Cluster (5TB RAM, 800 Cores)
* The following command restarts the local Spark Worker and points to a remote Spark Master
```
root@docker$ start-core-services-only-worker.sh <insert-common-master-ip-here>
```
* Check out all of the Spark Workers registered with the common Spark Master!
```
http://<common-master-ip>:6060/
```

# Misc Instructions (Ignore these please)
* If you've been provided a `.pem` download link, download it now
```
wget <insert-url-to-pem-file-here>
```

* Move the .pem into the `~/.ssh` directory:
```
mkdir -p ~/.ssh
mv ~/Downloads/pipeline-training-gce.pem ~/.ssh/
```

* Update the `.pem` file permissions:
```
chmod 600 ~/.ssh/pipeline-training-gce.pem
```

* Add the `.pem` file to your keychain (TODO:  password provided later)
```
ssh-add ~/.ssh/pipeline-training-gce.pem
Enter passphrase for /Users/<username>/pipeline-training-gce.pem
```
