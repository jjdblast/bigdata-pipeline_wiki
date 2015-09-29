Obtain the IP of boot2docker (and therefore your Docker container)
```
macosx-laptop$ boot2docker ip
```
Use this ^ IP or `$(boot2docker ip 2>/dev/null)` in subsequent commands

### Apache Spark Master Admin Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):36060
```

### Apache Spark Worker Admin Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):36061
```

### Apache Spark Driver Admin Web UI
* Jobs, Stages, Tasks
* Environment Variables
* Event Timeline
* Streaming Tab
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):34040
```

### Tachyon Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):39999
```
