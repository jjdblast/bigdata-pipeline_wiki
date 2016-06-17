## Pause/Stop Cloud Instance
* Your Cloud Provider allows you to Pause or Stop your Cloud Instance to save money
* This freezes the state of the instance - including the running Docker Container
* You can resume your work later by following the steps below

## Restarting after Pausing/Stopping your Cloud Instance
* Re-Start Cloud Instance through Cloud Provider UI
* `ssh` into your Cloud Instance similar to the initial setup using the SSH keypair `.pem`

## Start the Docker Container
```
sudo docker start pipeline
```

### Attach to the Running Docker Container
```
sudo docker attach pipeline
```
* Hit [Enter] a few times to get a Docker Container prompt

### Restart the Core Services
```
cd $PIPELINE_HOME && start-all-services.sh
```

### Explore Services
* [Explore](https://github.com/fluxcapacitor/pipeline/wiki/Explore-Services) the environment