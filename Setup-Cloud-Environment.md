## Cloud Instance
* The following are the minimum requirements for your Cloud Instance:
**8 Cores, 50GB RAM, 100GB SSD**

## Firewall and Cloud Instance Security Groups
* Make sure all ports are open on your Cloud Instance
* We will narrow this later, but for now it's easier to just open them all

## Logging Into Your Instance 
* Use SSH or Putty (Windows) to log into the instance 
```
ssh -i ~/.ssh/<your-pem-file> <your-username>@<your-cloud-instance-public-ip>
```

* Setup some local env vars
```
echo "alias docker='sudo docker'" > ~/.bash_aliases
source ~/.bash_aliases
```

* Pull down the latest Docker image
(This will take a few mins, please be patient)
```
sudo docker pull fluxcapacitor/pipeline
```

* Run the following command to start up the Docker container
* The assumption is that this is a fresh Cloud Instance with nothing else running on it.
* This Docker container will bind to many ports including port 80, so make sure even `apache2` is disabled before running this command
```
sudo docker run -it --name pipeline --net=host -m 48g fluxcapacitor/pipeline bash
```

* Run the following commands *inside of the Docker container*
```
root@docker$ cd $PIPELINE_HOME && git pull && source $CONFIG_HOME/bash/pipeline.bashrc && $SCRIPTS_HOME/setup/RUNME_ONCE_LARGE.sh
```

* Wait a few mins for initialization to complete!

## Explore Your Environment
* Navigate to the main demo URL in your browser
```
http://<your-cloud-instance-public-ip>
```
* Click around on the navigation links at the top and familiarize yourself with the environment

## Saving Your Work
* You can save your live Docker container as follows:
```
root@docker$ cd ~ && docker export --output="pipeline.tar" pipeline
```
* This creates an 11 GB Docker image based on a snapshot of your live, running container

* Download the newly-created Docker image to your local home directory as follows:
```
local-laptop$ scp -i ~/<your-pem-file.pem> <your-username>@<your-cloud-instance-public-ip>:pipeline.tar ~/pipeline.tar
```