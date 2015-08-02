## Running out of disk space
* It's likely that you have old, unused containers from each `docker run` command
* These don't get garbage collected automatically as Docker assumes you may want to start them again
* Use the following command to clean them out
```
docker rm `docker ps -aq`
```

## error in run: Machine "boot2docker-vm" does not exist.
Run the following to repair a busted boot2docker:
```
macosx-laptop$ sudo /Library/Application\ Support/VirtualBox/LaunchDaemons/VirtualBoxStartup.sh restart
```

## More Troubleshooting from boot2docker's Github
* https://github.com/boot2docker/boot2docker#boot2docker-up-doesnt-work-osx