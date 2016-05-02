## You Are Outside of the Docker Container and Can't Get Back In?!
### Make Sure Docker Container is Started
```
sudo docker start pipeline
```

### Attach to Docker Container
```
sudo docker attach pipeline
``` 

### Make Sure All Services are Running
```
jps -l
...
<a bunch of java process that you saw running before>
```

### No Services Running?  Start Services Again
```
cd $PIPELINE_HOME && start-core-services-large.sh
```
