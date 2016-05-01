* This will take about 15 mins to build - and lots of internet traffic - as dependent binaries and libraries are retrieved from the internet.
* Run the `docker build` command from the directory where the `Dockerfile` lives (ie. ~/pipeline)

```
local-laptop$ git clone https://github.com/fluxcapacitor/pipeline.git
local-laptop$ cd ~/pipeline

[... Make Changes ...]

local-laptop$ docker build -t fluxcapacitor/pipeline .
```
Notes
* If you run out of memory or disk space while building or running the image, you need to re-initialize docker-machine with larger memory and disk size.

## Tag and Push the Docker Image to the DockerHub Registry
For now, just overwrite the `latest` tag as follows:
```
local-laptop$ docker push fluxcapacitor/pipeline
```
