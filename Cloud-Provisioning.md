## EC2
* Minimum Instance Type:  `m4.xlarge` with 4 CPUs and 16 GB RAM

* Create an EC2 instance using the `user data` below
```
#!/bin/bash
sudo apt-get update
sudo apt-get install curl
curl -fsSL https://get.docker.com/ | sh
curl -fsSL https://get.docker.com/gpg | sudo apt-key add -
sudo usermod -a -G docker $USER
docker run hello-world
docker pull fluxcapacitor/pipeline
echo 'DOCKER_OPTS="--ip=127.0.0.1"' | sudo tee -a /etc/default/docker
sudo service docker restart
```

* Download the `pipeline-training.pem` file from [here](http://advancedspark.com/keys/pipeline-training.pem)

* Place the `pipeline-training.pem` file here
```
cp ~/Downloads/pipeline-training.pem ~/
```

* Log in to your instance as follows
```
ssh -i "~/pipeline-training.pem" ubuntu@<public-ip-address-of-ec2-instance>
```