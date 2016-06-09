### Cannot Connect to Cloud Instance or Navigation Links
* You likely have not configured your cloud instance firewall rules (GCE) or security groups (AWS) properly
* Go back [HERE](https://github.com/fluxcapacitor/pipeline/wiki/Setup-Cloud-Environment#firewall-and-cloud-instance-security-groups) and open up the cloud instance firewall rules or security groups to all incoming traffic on all ports
* Try again once you've properly configured the networking


### You Are Outside of the Docker Container and Can't Get Back In?!
* Make Sure Docker Container is Started
```
sudo docker start pipeline
```

* Attach to Docker Container
```
sudo docker attach pipeline
``` 

* Make Sure All Services are Running
```
jps -l
...
<a bunch of java process that you saw running before>
```

* No Services Running?  Start Services Again
```
cd $PIPELINE_HOME && start-all-services.sh
```

### Email for Help
* Emailing **help@fluxcapacitor.com** will open a ticket with our support system.  
* We will get back to you ASAP!