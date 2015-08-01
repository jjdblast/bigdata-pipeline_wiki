## Building a Docker Image
This will take about 15 mins to build - and lots of internet traffic - as dependent binaries and libraries are retrieved from the internet.

```
$ git clone https://github.com/fluxcapacitor/pipeline.git
$ cd pipeline

[... Make Changes ...]

$ docker build -t fluxcapacitor/pipeline .
```
Notes
* If you run out of memory or disk space while building or running the image, you need to re-initialize boot2docker with larger `--memory` and `--disk-size` per the steps above.
* If you see the following error
```
An error occurred trying to connect: Post https://192.168.59.103:2376/v1.19/build?cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&memory=0&memswap=0&rm=1&t=fluxcapacitor%2Fpipeline: dial tcp 192.168.59.103:2376: i/o timeout
```
* You'll need to do the following to rebuild the boot2docker VirtualBox VM:
```
macosx-laptop$ boot2docker stop
macosx-laptop$ boot2docker destroy
macosx-laptop$ boot2docker --memory=16384 --disksize=30000 init
macosx-laptop$ boot2docker up
```
* **You may lose data that is stored within boot2docker - including persistent volumes!**

## Tag and Push the Docker Image to the DockerHub Registry
[TODO] Tagging

For now, just overwrite the `latest` tag with the following
```
$ docker push fluxcapacitor/pipeline
```