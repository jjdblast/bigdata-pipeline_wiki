## Cloud Instance
* Instance Type:  GCE n1-highmem-8 instance
* 8 Cores, 50GB RAM, 100GB SSD
* **MAKE SURE ALL PORTS ARE OPEN ON THE FIREWALL OF THE CLOUD INSTANCE!!**

## Logging Into Your Instance 
### If you've been provided a `.pem` download link, download it now
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

* SSH into the instance 
```
ssh -i ~/.ssh/pipeline-training-gce.pem pipeline-training@<your-cloud-instance-public-ip>
```

* Setup some local env vars
```
echo "alias docker='sudo docker'" > ~/.bash_aliases
source ~/.bash_aliases
```

* Pull down the latest Docker image (if it's not already there)
```
sudo docker images
sudo docker pull fluxcapacitor/pipeline
```

* Run the following command to start up the Docker container
* Note:  Review the [Docker]() file to make sure your host does not already listen on the EXPOSEd ports
```
sudo docker run -it --name pipeline --net=host -m 48g fluxcapacitor/pipeline bash
```

* Run the following commands inside of the Docker container
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
* At the end of the workshop, save your Docker container as follows:
```
root@docker$ cd ~ && docker export --output="pipeline.tar" pipeline
```
* This creates a 10 GB Docker image based on your live, running container

* Download the newly-created Docker image as follows:
```
local-laptop$ scp -i ~/pipeline-training-gce.pem pipeline-training@<your-cloud-instance-public-ip>:pipeline.tar ~/pipeline.tar
```