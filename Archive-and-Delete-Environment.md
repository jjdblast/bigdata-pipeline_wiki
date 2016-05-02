* The following steps should almost NEVER be done.
* If you simply want to pause your exploration and resume later, please follow [THESE](http://github.com/fluxcapacitor/pipeline/wiki/Pause-and-Stop-Environment).
* These steps are the LAST RESORT scenario when you are absolutely done with your exploration and wish to archive your environment for later.
* This is a very length process, so please be sure this is worth the effort.
* Unless you've made a lot of changes that you'd like to keep, it's almost always easier to follow [THESE](https://github.com/fluxcapacitor/pipeline/wiki/Setup-Cloud-Environment) steps from scratch and start with a fresh Docker Image when you're ready to explore again
* If you still wish to continue, here you go!

## Stop Services
* The following must be done within the Docker Container.
```
stop-all-services.sh
```

## Exit Docker Container
* `exit` out of your Docker Container (again, this is a last resort) to stop your Docker Container

## Save Options
* There are two options:  both create a 10 GB+ .tar file that you can download AS A LAST RESORT

### Option 1:  Save Live Docker Container
* Save your live Docker Container as a 10 GB+ .tar file as follows:
```
cd ~ && sudo docker export --output="pipeline.tar" pipeline
```

### Option 2:  Save Underlying Docker Image (without any of your changes)
* Save your underlying Docker Image as 10 GB+ .tar file follows:
```
cd ~ && sudo docker save --output="pipeline.tar" fluxcapacitor/pipeline
```

* Download the newly-created `pipeline.tar` file from your Cloud Instance to your local home directory using your `.pem` file from the SSH Keypair that you created during Cloud Instance setup:
```
scp -i ~/<your-pem-file.pem> <your-username>@<your-cloud-instance-public-ip>:pipeline.tar ~/pipeline.tar
```