* These steps are when you are ready to pause your exploration 
* You can PAUSE/STOP (not DELETE/TERMINATE) your Cloud Instance to save money
* You plan to resume later

## Pause/Stop Cloud Instance
### Your Cloud Provider allows you to Pause or Stop your Cloud Instance
### This freezes the state of the instance - including the running Docker Container

## Restarting after Pausing/Stopping your Cloud Instance
### Re-Start Cloud Instance through Cloud Provider UI

### `ssh` into your Cloud Instance similar to the initial setup using the SSH keypair `.pem`

### Start the Docker Container
```
sudo docker start pipeline
```

### Attach to the Running Docker Container
```
sudo docker attach pipeline
```
* Hit <enter> a few times to get a Docker Container prompt

### Restart the Core Services
```
cd $PIPELINE_HOME && start-core-services-large.sh
```

### Explore Services
* Follow [THESE](https://github.com/fluxcapacitor/pipeline/wiki/Explore-Services) instructions from initial setup