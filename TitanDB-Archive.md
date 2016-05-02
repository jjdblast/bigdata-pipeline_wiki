### Titan
* Requires `start-titan-service.sh`
```
http://<ip>:8182/
```


### Titan
* Requires `start-titan-service.sh`
```
root@docker$ gremlin.sh $MYAPPS_HOME/titan/item-ratings.gremlin
         \,,,/
         (o o)
-----oOOo-(3)-oOOo-----
plugin activated: aurelius.titan
plugin activated: tinkerpop.server
plugin activated: tinkerpop.utilities
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/root/titan-1.0.0-hadoop1/lib/slf4j-log4j12-1.7.5.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/root/titan-1.0.0-hadoop1/lib/logback-classic-1.1.2.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
00:55:52 INFO  org.apache.tinkerpop.gremlin.hadoop.structure.HadoopGraph  - HADOOP_GREMLIN_LIBS is set to: /root/titan-1.0.0-hadoop1/lib
plugin activated: tinkerpop.hadoop
plugin activated: tinkerpop.tinkergraph
==>standardtitangraph[cassandrathrift:[127.0.0.1]]
==>/root/pipeline/datasets/graph/item_ratings.csv
gremlin> 
```
* `ctrl-c` to exit
