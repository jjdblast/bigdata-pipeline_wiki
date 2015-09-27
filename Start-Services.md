* **At this point, you are inside of the Docker Container.**
* Keep an eye on the prompt:  `root@docker$` means that you're inside docker, otherwise, you're on your local laptop.

### Setup the Environment
* You must `source` the following script to setup and start the pipeline services.
--> Don't forget the `source` below! <--
```
root@docker$ cd ~/pipeline && source ~/pipeline/flux-one-time-setup.sh
```
### --> ^^ Don't forget the `source` above! ^^ <--

### Verify that Setup Worked Correctly
* Verify that the output of `export` contains `$PIPELINE_HOME` among many other new exports
```
root@docker$ export
```
* If not, you'll need to do the following again:
```
source ~/.profile
```

* Verify the output of `jps -l` looks something like this
```
root@docker$ jps -l
2374 kafka.Kafka <-- Kafka Server
3764 io.confluent.kafka.schemaregistry.rest.Main <-- Kafka Schema Registry
2373 org.apache.zookeeper.server.quorum.QuorumPeerMain <-- ZooKeeper
95 -- process information unavailable <-- Either ElasticSearch or Cassandra*
3765 io.confluent.kafkarest.Main <-- Kafka Rest Proxy
3762 play.core.server.NettyServer <-- Spark-Notebook
919 -- process information unavailable <-- Either ElasticSearch or Cassandra*
2435 org.apache.zeppelin.server.ZeppelinServer <-- Zeppelin WebApp
2743 org.apache.spark.deploy.master.Master <-- Spark Master
4074 sun.tools.jps.Jps <-- This jps Process
3599 tachyon.master.TachyonMaster <-- Tachyon Master
3718 tachyon.worker.TachyonWorker <-- Tachyon Worker
2908 org.apache.spark.deploy.worker.Worker <-- Spark Worker
```
Note that the "process information unavailable" message appears to be an OpenJDK [bug](https://bugs.openjdk.java.net/browse/JDK-8075773).

* Verify the number of CPU cores matches what you expect (at least 4 CPU cores or things won't work right)
* Note:  Knowing this number will help you troubleshoot Spark problems later as you may hit Spark Job resource starvation issues if you run too many long-running jobs at the same time (ie. Hive ThriftServer, Spark Streaming, Spark Job Server).
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