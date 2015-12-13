* This will take about 15 mins to build - and lots of internet traffic - as dependent binaries and libraries are retrieved from the internet.
* Run the `docker build` command from the directory where the `Dockerfile` lives (ie. ~/pipeline)

```
$ git clone https://github.com/fluxcapacitor/pipeline.git
$ cd ~/pipeline

[... Make Changes ...]

$ docker build -t fluxcapacitor/pipeline .
```
Notes
* If you run out of memory or disk space while building or running the image, you need to re-initialize boot2docker with larger `--memory` and `--disksize` per [Setup Docker Image](https://github.com/fluxcapacitor/pipeline/wiki/Setup-Docker-Image).

## Tag and Push the Docker Image to the DockerHub Registry
[TODO] Tagging
For now, just overwrite the `latest` tag with the following
```
$ docker push fluxcapacitor/pipeline
```

## Exporting and Importing Docker Images and Containers
### Export Docker Image as .tar
```
$ docker save --output="fluxcapacitor-pipeline.tar" fluxcapacitor/pipeline
```
### Export Docker Container as .tar
```
$ docker export --output="fluxcapacitor-pipeline.tar" fluxcapacitor/pipeline
```
### Import Docker Image or Container from a .tar
```
$ docker load < fluxcapacitor-pipeline.tar
```