* **At this point, you are inside of the Docker Container.**
* Keep an eye on the prompt:  `root@docker$` means that you're inside docker, otherwise, you're on your local laptop.

### Setup the Environment
* The following command configures and starts all of the services
```
root@docker$ ~/pipeline/bin/RUNME_ONCE.sh
```

### Verify that Setup Worked Correctly
* Verify the output of the script above looks something like this
```
...Show Running Java Processes...
737 org.elasticsearch.bootstrap.Elasticsearch           <-- ElasticSearch
738 org.jruby.Main                                      <-- Logstash
1987 org.apache.zeppelin.server.ZeppelinServer          <-- Zeppelin 
2243 org.apache.spark.deploy.worker.Worker              <-- Spark Worker
1973 kafka.Kafka                                        <-- Kafka
3479 sun.tools.jps.Jps                                  <-- this (jps)
1529 org.apache.zookeeper.server.quorum.QuorumPeerMain  <-- ZooKeeper (for Kafka 0.8 - will upgrade to 0.10 soon)
2555 io.confluent.kafka.schemaregistry.rest.Main        <-- Kafka SchemaRegistry
2123 org.apache.spark.deploy.master.Master              <-- Spark Master
2556 io.confluent.kafkarest.Main                        <-- Kafka REST API
...
```
* Verify that the output of `export` contains `$PIPELINE_HOME` among many other new exports
```
root@docker$ export
```
