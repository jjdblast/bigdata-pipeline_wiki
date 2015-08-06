## Metrics and Monitoring
### Kibana and Logstash
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):35601'
macosx-laptop$ open $(boot2docker ip 2>/dev/null):35601
```

### Ganglia
```
macosx-laptop$ curl '$(boot2docker ip 2>/dev/null):30080/ganglia'
macosx-laptop$ open $(boot2docker ip 2>/dev/null):30080/ganglia
```