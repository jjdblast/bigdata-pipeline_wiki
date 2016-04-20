## Cloud Instance
* Instance Type:  8 CPUs and 50GB RAM, 50GB SSD (GCE n1-highmem-8 instance)

## Logging Into Your Instance 
* Download the `pipeline-training-gce.pem` file (location of .pem provided later)
* Copy the `pipeline-training-gce.pem` file into your root directory:
```
cp ~/Downloads/pipeline-training-gce.pem ~/
```

* Log in to your instance as follows
```
ssh -i "~/pipeline-training-gce.pem" pipeline-training@<your-cloud-instance-public-ip>
```

* Run the following command to start up the Docker container
```
sudo docker run -it --name pipeline -h docker -m 48g -p 80:80 -p 36042:6042 -p 39160:9160 -p 39042:9042 -p 39200:9200 -p 37077:7077 -p 38080:38080 -p 38081:38081 -p 36060:6060  -p 36061:6061 -p 36062:6062 -p 36063:6063 -p 36064:6064 -p 36065:6065 -p 32181:2181 -p 38090:8090 -p 30000:10000 -p 30070:50070 -p 30090:50090 -p 39092:9092 -p 36066:6066 -p 39000:9000 -p 39999:19999 -p 36081:6081 -p 35601:5601 -p 37979:7979 -p 38989:8989 -p 34040:4040 -p 34041:4041 -p 34042:4042 -p 34043:4043 -p 34044:4044 -p 34045:4045 -p 34046:4046 -p 34047:4047 -p 34048:4048 -p 34049:4049 -p 34050:4050 -p 34051:4051 -p 34052:4052 -p 34053:4053 -p 34054:4054 -p 34055:4055 -p 34056:4056 -p 34057:4057 -p 34058:4058 -p 34059:4059 -p 34060:4060 -p 36379:6379 -p 38888:8888 -p 34321:54321 -p 38099:8099 -p 38754:8754 -p 37379:7379 -p 36969:6969 -p 36970:6970 -p 36971:6971 -p 36972:6972 -p 36973:6973 -p 36974:6974 -p 36975:6975 -p 36976:6976 -p 36977:6977 -p 36978:6978 -p 36979:6979 -p 36980:6980 -p 35050:5050 -p 35060:5060 -p 37060:7060 -p 38182:8182 -p 39081:9081 -p 38998:8998 -p 39090:9090  -p 35080:5080 -p 35090:5090 fluxcapacitor/pipeline bash
```

* Run the following commands inside of the Docker container
```
root@docker$ cd $PIPELINE_HOME && git pull && source $CONFIG_HOME/bash/pipeline.bashrc && $SCRIPTS_HOME/setup/RUNME_ONCE.sh
```

* Wait a few mins for initialization to complete!

## Do Workshop!
* [Explore](https://github.com/fluxcapacitor/pipeline/wiki/Explore-Services) the Environment
* ...many other things...

## Saving Your Work
* At the end of the workshop, save your Docker container as follows:
```
root@docker$ cd ~ && docker export --output="pipeline.tar" pipeline
```
* This creates a 10 GB Docker image based on your live, running container

* Download the newly-created Docker image as follows:
```
local-laptop$ scp -i ~/pipeline-training-gce.pem pipeline-training@<your-cloud-instance-public-ip>:pipeline.tar ~/pipeline.tar
```

## Instance Launch Scripts 
* Ignore this as we're doing this for you automatically
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
