### Stop the Pipeline Services
* The following must be done within the Docker Container.
```
stop-all-services.sh
```

### Saving Your Work
* You can save your live Docker container as follows:
```
root@docker$ cd ~ && docker export --output="pipeline.tar" pipeline
```
* This creates an 11 GB Docker image based on a snapshot of your live, running container

* Download the newly-created Docker image to your local home directory as follows:
```
scp -i ~/<your-pem-file.pem> <your-username>@<your-cloud-instance-public-ip>:pipeline.tar ~/pipeline.tar
```