## Linux (ie. AWS EC2 Ubuntu 14.04)

### Install Docker

**boot2docker is Not Needed**

Install Docker as follows
```
ec2-linux$ wget -qO- https://get.docker.com/ | sh
```

Add your user (ie. ubuntu) to the docker group:
```
ec2-linux$ sudo usermod -aG docker ubuntu
```
Notes:
* You need to logout and login for the ubuntu user to get added to the docker group
* If you see the following error when running subsequent `docker` commands, you likely did not logout and login for the ubuntu user to be added to the docker group.  Please do this, otherwise you'll need to prepend `sudo` to all of your docker commands.
```
dial unix /var/run/docker.sock: permission denied. Are you trying to connect to a TLS-enabled daemon without TLS?
```

### Running a Docker Container
[EC2-Linux] You'll need to do the following when you first login to Docker using the `docker run' command above:
```
root@[docker]$ sudo su -
```

### Testing your Docker Container from Outside Docker (local laptop) 

### [EC2-Linux] Use the Public IP of your Linux machine (ie. AWS EC2 Elastic or Dynamic IP) 

If you're on an AWS EC2 instance, you can use the following to discover the public IP:
```
ec2-linux$ curl http://169.254.169.254/latest/meta-data/
<public-ipv4>
```