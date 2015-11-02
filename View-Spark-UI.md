## Obtain <docker-hostname-or-ip> using one of the following:
```
macosx-laptop$ boot2docker ip
```
or
```
macosx-laptop$ `$(boot2docker ip 2>/dev/null)`
```

### Apache Spark Master Admin Web UI
```
macosx-laptop$ open http://<docker-hostname-or-ip>:36060
```

### Apache Spark Worker Admin Web UI
```
macosx-laptop$ open http://<docker-hostname-or-ip>:36061
```

### Apache Spark Driver Admin Web UI
* Jobs, Stages, Tasks
* Environment Variables
* Event Timeline
* Streaming Tab
```
macosx-laptop$ open http://<docker-hostname-or-ip>:34040
```

### Tachyon Web UI
```
macosx-laptop$ open http://<docker-hostname-or-ip>:39999
```
