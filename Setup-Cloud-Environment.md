## Cloud Instance
* The following are the minimum requirements for your Cloud Instance:
**8 Cores**, **50GB RAM**, **100GB SSD**
* Typically, we use either the Amazon Web Services `r3.2xlarge` EC2  or Google Cloud Platform `n1-highmem-8` GCE Cloud Instance  types
* These Cloud Instance types cost around $8-10 per day and get less expensive each month
* Later, we will show you how to save money by pausing/stopping your instance - allowing you to resume your work at a later date.

(Note:  We have deprecated the local laptop installation instructions given that most laptops are not equipped to handle the large memory and disk/dataset footprint of this environment.)

## Firewall and Cloud Instance Security Groups
* Make sure all ports are open on your Cloud Instance
* We will tighten the security later, but for now it's easier to just open them all

### Google Cloud Platform Firewall Rule Modifications
* Below is a screen shot of the **FIREWALL RULES CHANGES** required to allow all traffic into your instance
* In this example, my instances are using the "default" network which is the Google default
* You **must modify these rules** or you will only be able to connect to your instance on port 80 (and 443 if selected)

![Google Cloud Platform Firewall Rules](http://advancedspark.com/img/gce-firewall-rules.png)

### Amazon Web Services Security Group Modifications
* Below is a screen shot of the **SECURITY GROUP CHANGES** required to allow all traffic into your instance
* In this example, the instance is using a security group named `fluxcapacitor`
* You **must modify the security group** or you will only be able to connect to your instance on port 80 (and 443 if selected)

![AWS Security Groups](http://advancedspark.com/img/aws-security-groups.png)

## Setup SSH Key Pairs
### Google Cloud Platform
* https://cloud.google.com/compute/docs/instances/connecting-to-instance#generatesshkeypair

### Amazon Web Services
* Create SSH Key Pair
![Create Key Pair](http://advancedspark.com/img/aws-create-keypair.png)

* Result of Associating Key Pair to a Cloud Instance at Creation Time
![Cloud Instance Keypair Association](http://advancedspark.com/img/aws-keypair-instance.png) 

## Download and Configure SSH Key (.pem file)
* Download the SSH Key and place into '~/.ssh/<keypair-name>.pem`
* Modify the permissions of the SSH Key
```
chmod 600 ~/.ssh/<key-name>.pem
```
 
## Logging Into Your Instance 
### Linux/MacOS X
* Use SSH to log in to your Cloud Instance using the `.pem` file created from the previous step
* You may have to enter the password you used when you created the keypair in an earlier step 
```
ssh -i ~/.ssh/<your-pem-file> <your-username>@<your-cloud-instance-public-ip>
```
### Windows
* Use [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

![Putty Host IP](http://advancedspark.com/img/putty-1.png)

![Putty pem File](http://advancedspark.com/img/putty-2.png)

## Setup Docker
```
sudo apt-get update
sudo apt-get install curl
curl -fsSL https://get.docker.com/ | sh
curl -fsSL https://get.docker.com/gpg | sudo apt-key add -
```
* Note:  Please ignore the following message that appears after the above:
```
... <IGNORE THIS>
If you would like to use Docker as a non-root user, you should now consider 
adding your user to the "docker" group with something like:

  sudo usermod -aG docker <username> 
... </IGNORE THIS>
```

* Pull down the latest Docker image
(This will take a few mins, please be patient)
```
sudo docker pull fluxcapacitor/pipeline
```