## Running out of disk space
* It's likely that you have old, unused containers from each `docker run` command
* These don't get garbage collected automatically as Docker assumes you may want to start them again
* Use the following command to clean them out
```
docker rm `docker ps -aq`
```

## error in run: Machine "boot2docker-vm" does not exist.
* If you see this error, you've got big issues.  
* Trying to work through this on my own laptop!

