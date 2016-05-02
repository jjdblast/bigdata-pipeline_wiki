* The following steps should almost NEVER be done.
* Go [HERE]() to follow the Pause Exploration steps.
* These steps are for the LAST RESORT scenario when you are absolutely done with your exploration and wish to archive your work for months down the road.
* Or if you are planning to switch Cloud Providers - at which case, it's almost always easier to follow the steps from scratch and start from a fresh Docker Image per the [Setup Cloud Environment]() instructions

## Stop Services
* The following must be done within the Docker Container.
```
stop-all-services.sh
```

## Save Your Work
* `exit` out of your Docker Container (again, this is a last resort)

### Verify You are on your Host Cloud Instance <your-cloud-instance-ip> and Not your Docker Container

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

* Download the newly-created Docker image to your local home directory as follows:
```
scp -i ~/<your-pem-file.pem> <your-username>@<your-cloud-instance-public-ip>:pipeline.tar ~/pipeline.tar
```