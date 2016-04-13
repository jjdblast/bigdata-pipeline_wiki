* **At this point, you are inside of the Docker Container.**
* Keep an eye on the prompt:  `root@docker$` means that you're inside docker, otherwise, you're on your local laptop.

### Setup the Environment
* The following command configures and starts all of the services
```
root@docker$ cd $PIPELINE_HOME && git pull && source $CONFIG_HOME/bash/pipeline.bashrc && $SCRIPTS_HOME/setup/RUNME_ONCE.sh
```

### Verify that Setup Worked Correctly
* Verify the output of the script above looks something like this
```
...Show Running Java Processes...
737 org.elasticsearch.bootstrap.Elasticsearch           <-- ElasticSearch
738 org.jruby.Main                                      <-- Logstash
1987 org.apache.zeppelin.server.ZeppelinServer          <-- Zeppelin
2243 org.apache.spark.deploy.worker.Worker              <-- Spark Worker
2123 org.apache.spark.deploy.master.Master              <-- Spark Master
3479 sun.tools.jps.Jps                                  <-- this (jps)
1529 org.apache.zookeeper.server.quorum.QuorumPeerMain  <-- ZooKeeper (for Kafka 0.8 only - will upgrade to 0.10 soon)
1973 kafka.Kafka                                        <-- Kafka
2555 io.confluent.kafka.schemaregistry.rest.Main        <-- Kafka SchemaRegistry
2556 io.confluent.kafkarest.Main                        <-- Kafka REST API
2547 org.apache.hadoop.util.RunJar                      <-- Hive Metastore Service (Uses MySQL as backing store)
...
```
* Verify that the output of `export` contains the proper `$PATH`, `$PIPELINE_HOME`, `$MYSQL_CONNECTOR_JAR` among many other environment variables
```
root@docker$ export
...
declare -x PATH=/root/titan-1.0.0-hadoop1/bin:/root/presto-server-0.137/bin:/root/airflow/bin:/root/flink-1.0.0/bin:/root/zeppelin-0.6.0/bin:/root/sbt/bin:/root/nifi-0.4.1/bin:/root/webdis:/root/redis-3.0.5/bin:/root/apache-hive-1.2.1-bin/bin:/root/hadoop-2.6.0/bin:/root/kibana-4.5.0-linux-x64/bin:/root/logstash-2.3.0/bin:/root/elasticsearch-2.3.0/bin:/root/confluent-1.0.1/bin:/root/confluent-1.0.1/bin:/root/spark-1.6.1-bin-fluxcapacitor/tachyon/bin:/root/spark-1.6.1-bin-fluxcapacitor/bin:/root/spark-1.6.1-bin-fluxcapacitor/sbin:/root/apache-cassandra-2.2.5/bin:/root/pipeline/bin/cli:/root/pipeline/bin/cluster:/root/pipeline/bin/docker:/root/pipeline/bin/initial:/root/pipeline/bin/kafka:/root/pipeline/bin/rest:/root/pipeline/bin/service:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
...
declare -x PIPELINE_HOME="/root/pipeline"
...
declare -x MYSQL_CONNECTOR_JAR="/usr/share/java/mysql-connector-java.jar"
```