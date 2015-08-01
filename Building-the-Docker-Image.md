## Building a Docker Image from the Dockerfile
This will take about 15 mins to build - and lots of internet traffic - as dependent binaries and libraries are retrieved from the internet.

```
ec2-linux or macosx-laptop$ git clone https://github.com/fluxcapacitor/pipeline.git
ec2-linux or macosx-laptop$ cd pipeline
ec2-linux or macosx-laptop$ docker build -t fluxcapacitor/pipeline .
```
Notes
* If you run out of memory or disk space while building or running the image, you need to re-initialize boot2docker with larger `--memory` and `--disk-size` per the steps above.

## Tag and Push the Docker Image to the DockerHub Registry
[TODO] Tagging

For now, just overwrite the `latest` tag with the following
```
ec2-linux or macosx-laptop$ docker push fluxcapacitor/pipeline
```