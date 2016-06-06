* The following steps should almost NEVER be done.
* If you simply want to pause your exploration and resume later, please follow [THESE](http://github.com/fluxcapacitor/pipeline/wiki/Pause-and-Stop-Environment) steps.
* These steps are the LAST RESORT scenario when you are absolutely done with your exploration and wish to archive your environment for later.
* This is a very length process, so please be sure this is worth the effort.
* Unless you've made a lot of changes that you'd like to keep, it's almost always easier to follow [THESE](https://github.com/fluxcapacitor/pipeline/wiki/Setup-Cloud-Environment) steps from scratch and start with a fresh Docker Image when you're ready to explore again
* If you still wish to continue, here you go!

## Exit Docker Container
* `exit` out of your Docker Container (again, this is a last resort) 
* This will stop your Docker Container as well as all services running within the Container

## Save Options
* The following creates a 10 GB+ .tar file that you will need to download from your Cloud Instance to your local laptop
* Export your Docker Container
```
cd ~ && sudo docker export --output="pipeline.tar" pipeline
```

* Download the newly-created `pipeline.tar` file from your Cloud Instance to your local home directory using your `.pem` file from the SSH Keypair that you created during Cloud Instance setup:
```
scp -i ~/<your-pem-file.pem> <your-username>@<your-cloud-instance-public-ip>:pipeline.tar ~/pipeline.tar
```

## Reload the Docker Container at a Later Date
* When you are ready to reload the Docker Container, you can use the command below
* Note:  It is almost always easier to start with a fresh install unless you've made a lot of changes that you chose to preserve
```
sudo docker load < ~/pipeline.tar
```