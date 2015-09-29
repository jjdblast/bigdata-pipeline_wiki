The IP of boot2docker (and therefore your Docker container) is as follows
```
macosx-laptop$ boot2docker ip
```

### Apache Spark Master Admin Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):36060
```

### Apache Spark Worker Admin Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):36061
```

### Apache Spark Jobs Admin Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):34040
```

### Tachyon Web UI
```
macosx-laptop$ open http://$(boot2docker ip 2>/dev/null):39999
```
