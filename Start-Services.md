* **At this point, you are inside of the Docker Container.**
* Keep an eye on the prompt:  `root@docker$` means that you're inside docker, otherwise, you're on your local laptop.

### Setup the Environment
* You must `source` the following script to setup and start the pipeline services.

**--> Don't forget the `source` below! <--**

```
root@docker$ cd ~/pipeline/bin && source flux-one-time-setup.sh
```

**--> ^^ Don't forget the `source` above! ^^ <--**

### Verify that Setup Worked Correctly
* Verify the output of the script above looks something like this:
```
...Show Running Java Processes...
737 org.elasticsearch.bootstrap.Elasticsearch           <-- ElasticSearch
738 org.jruby.Main                                      <-- Logstash
1987 org.apache.zeppelin.server.ZeppelinServer          <-- Zeppelin 
2243 org.apache.spark.deploy.worker.Worker              <-- Spark Worker
1973 kafka.Kafka                                        <-- Kafka
3479 sun.tools.jps.Jps                                  <-- this (jps)
2391 org.apache.spark.deploy.history.HistoryServer      <-- Spark History Server
2408 play.core.server.NettyServer                       <-- Spark Notebook
1529 org.apache.zookeeper.server.quorum.QuorumPeerMain  <-- ZooKeeper
2555 io.confluent.kafka.schemaregistry.rest.Main        <-- Kafka SchemaRegistry (Avro)
2123 org.apache.spark.deploy.master.Master              <-- Spark Master
2556 io.confluent.kafkarest.Main                        <-- Kafka REST API
```
* Verify that the output of `export` contains `$PIPELINE_HOME` among many other new exports
```
root@docker$ export
```

* TROUBLESHOOTING ONLY:  If you don't see PIPELINE_HOME defined above, you'll need to do the following:
```
root@docker source ~/.profile
```
**Note:  This is likely an issue from an earlier step, so proceed at your own risk.**

* Verify the number of CPU cores matches what you expect (at least 4 CPU cores or things won't work right)

Note:  Knowing this number will help you troubleshoot Spark problems later as you may hit Spark Job resource starvation issues if you run too many long-running jobs at the same time (ie. Hive ThriftServer, Spark Streaming, Spark Job Server).
```
root@docker$ lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                8   <----------- Number of CPU Cores
On-line CPU(s) list:   0-7
Thread(s) per core:    2
Core(s) per socket:    4
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 62
Stepping:              4
CPU MHz:               2500.094
BogoMIPS:              5000.18
Hypervisor vendor:     Xen
Virtualization type:   full
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              25600K
NUMA node0 CPU(s):     0-7
```