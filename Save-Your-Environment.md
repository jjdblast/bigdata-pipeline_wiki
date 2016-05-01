## Stop Services
* The following must be done within the Docker Container.
```
stop-all-services.sh
```

## Save Your Work
* Hit `ctrl-p ctrl-q` to soft-exit from your Docker Container
* **Make sure you are on your host Cloud Instance (<your-cloud-instance-ip>) - and not within the Docker Container!**

* There are two options - and both create a 10 GB+ .tar file

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

* Download the newly-created Docker image to your local home directory as follows:
```
scp -i ~/<your-pem-file.pem> <your-username>@<your-cloud-instance-public-ip>:pipeline.tar ~/pipeline.tar
```