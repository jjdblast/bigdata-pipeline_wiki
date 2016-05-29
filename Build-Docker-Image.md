* This will take about 15 mins to build - and lots of internet traffic - as dependent binaries and libraries are retrieved from the internet.
* Run the `docker build` command from the directory where the `Dockerfile` lives (ie. ~/pipeline)

```
git clone https://github.com/fluxcapacitor/pipeline.git
cd ~/pipeline

[... Make Changes ...]
```
```
sudo docker login

Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.

Username:  <-- Entire DockerHub Username
Password:  <-- Entire DockerHub Password

Login Succeeded
```
* Build the image (this will take a while, so you may want to `nohup` and `&` the command
```
sudo docker build -t fluxcapacitor/pipeline .
```
Notes
* If you run out of memory or disk space while building or running the image, you need to re-initialize docker-machine with larger memory and disk size.

## Tag and Push the Docker Image to the DockerHub Registry
* Tag and push the newly-created Docker image (this will take a while, so you may want to `nohup` and `&` the command
* For now, just overwrite the `latest` tag as follows:
```
sudo docker push fluxcapacitor/pipeline
```

## Verify the Image is in DockerHub
* Login to [DockerHub](https://hub.docker.com)
* Navigate to `http://hub.docker.com/r/fluxcapacitor/pipeline` and verify the date of the Docker image
