* **At this point, you are inside of the Docker Container.**

### Verify that Setup Worked Correctly
* Verify the output of `jps -l` is *similar to* the following (may differ slightly):
```
jps -l
...
737 org.elasticsearch.bootstrap.Elasticsearch           <-- ElasticSearch
738 org.jruby.Main                                      <-- Logstash
1987 org.apache.zeppelin.server.ZeppelinServer          <-- Zeppelin
2243 org.apache.spark.deploy.worker.Worker              <-- Spark Worker
2123 org.apache.spark.deploy.master.Master              <-- Spark Master
3479 sun.tools.jps.Jps                                  <-- this (jps)
1529 org.apache.zookeeper.server.quorum.QuorumPeerMain  <-- ZooKeeper
1973 io.confluent.support.metrics.SupportedKafka        <-- Kafka
2555 io.confluent.kafka.schemaregistry.rest.SchemaRegistryMain        <-- Kafka SchemaRegistry
3408 io.confluent.kafkarest.Main                        <-- Kafka REST API
6107 org.apache.flink.runtime.jobmanager.JobManager     <-- Flink Service
2547 org.apache.hadoop.util.RunJar                      <-- Hive Metastore Service (Uses MySQL as backing store)
3211 org.apache.nifi.NiFi                               <-- NiFi Admin
2894 org.apache.nifi.bootstrap.RunNiFi                  <-- NiFi Flow
2908 com.facebook.presto.server.PrestoServer            <-- Presto Server
1712 target/scala-2.10/finagle-assembly-1.0.jar        <-- Finagle-based Recommendation Service

...
```
* Verify that the output of `export` contains the follows exports (among many others)
```
export
...
...
declare -x PIPELINE_HOME="/root/pipeline"
...
declare -x MYSQL_CONNECTOR_JAR="/usr/share/java/mysql-connector-java.jar"
```