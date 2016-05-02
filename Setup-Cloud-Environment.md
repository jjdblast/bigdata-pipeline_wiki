## Cloud Instance
* The following are the minimum requirements for your Cloud Instance:
**8 Cores, 50GB RAM, 100GB SSD**
* Note:  We have deprecated the local laptop installation instructions given that most laptops are not equipped to handle the large memory and disk/dataset footprint of this environment.
* A Cloud Instance with the given minimum requirements can be acquired for around $8-$10 per day - and getting cheaper by the day.

## Firewall and Cloud Instance Security Groups
* Make sure all ports are open on your Cloud Instance
* We will tighten the security later, but for now it's easier to just open them all

### Google Cloud Platform Firewall Rule Modifications
* Below is a screen shot of the **FIREWALL RULES CHANGES** required to allow all traffic into your instance
* In this example, my instances are using the "default" network which is the Google default
* You **must modify these rules** or you will only be able to connect to your instance on port 80 (and 443 if selected)
[Google Cloud Platform Firewall Rules](http://advancedspark.com/img/gce-firewall-rules.png)

### Amazon Web Services Security Group Modifications
* Below is a screen shot of the **SECURITY GROUP CHANGES** required to allow all traffic into your instance
* In this example, my instances are using the "fluxcapacitor" security group which I created myself
* You **must modify the security group** or you will only be able to connect to your instance on port 80 (and 443 if selected)
[AWS Security Groups](http://advancedspark.com/img/aws-security-groups.png)

## Logging Into Your Instance 
* Use SSH or Putty (Windows) to log into the instance using the `.pem` file created upon Cloud Instance creation
```
ssh -i ~/.ssh/<your-pem-file> <your-username>@<your-cloud-instance-public-ip>
```

## Setup Docker
```
sudo apt-get update
sudo apt-get install curl
curl -fsSL https://get.docker.com/ | sh
curl -fsSL https://get.docker.com/gpg | sudo apt-key add -
```

## Setup Environment
```
echo "alias docker='sudo docker'" >> ~/.bash_aliases
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

* Run the following commands **inside the Docker Container**
```
cd $PIPELINE_HOME && git pull && source $CONFIG_HOME/bash/pipeline.bashrc && $SCRIPTS_HOME/setup/RUNME_ONCE_LARGE.sh
```

* Wait a few mins for initialization to complete...  this may take some time.

## Explore Your Environment
* Navigate to the main demo URL in your browser
```
http://<your-cloud-instance-public-ip>
```
* Click on the navigation links at the top and familiarize yourself with the environment