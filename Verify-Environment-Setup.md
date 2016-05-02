### Starting the Docker Container

**At this point, you should be ssh'd into your specific cloud instance**

* Run the following command to start up the Docker Container
* The assumption is that this is a fresh Cloud Instance with nothing bound to popular ports like 80, 8080, 9090, etc
Note:  This Docker Container will bind to many ports including port 80, so make sure even `apache2` is disabled before running this command
```
sudo docker run -it --name pipeline --net=host -m 48g fluxcapacitor/pipeline bash
```

### Configuring the Environment and Starting All Services

**At this point, you should be in your Docker Container**

* Run the following commands **inside the Docker Container**
```
cd $PIPELINE_HOME && git pull && source $CONFIG_HOME/bash/pipeline.bashrc && $SCRIPTS_HOME/setup/RUNME_ONCE_LARGE.sh
```


**Wait a few mins for initialization to complete...  this may take some time.**


### Verifying the Initialization
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
2555 io.confluent.kafka.schemaregistry.rest.SchemaRegistryMain <-- Kafka SchemaRegistry
3408 io.confluent.kafkarest.Main                        <-- Kafka REST API
6107 org.apache.flink.runtime.jobmanager.JobManager     <-- Flink Service
2547 org.apache.hadoop.util.RunJar                      <-- Hive Metastore Service (Uses MySQL as backing store)
3211 org.apache.nifi.NiFi                               <-- NiFi Admin
2894 org.apache.nifi.bootstrap.RunNiFi                  <-- NiFi Flow
2908 com.facebook.presto.server.PrestoServer            <-- Presto Server
1712 target/scala-2.10/finagle-assembly-1.0.jar         <-- Finagle-based Recommendation Service
...
(may be a few more or a few less...)
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

## Explore Your Environment
* Navigate your browser to the Demo Home Page
```
http://<your-cloud-instance-public-ip>
```
* Click on the navigation links at the top to familiarize yourself with the tools of the environment

## Troubleshooting
### Cannot Connect to Cloud Instance or Navigation Links
* You likely have not configured your cloud instance firewall rules (GCE) or security groups (AWS) properly
* Go back [HERE](https://github.com/fluxcapacitor/pipeline/wiki/Setup-Cloud-Environment#firewall-and-cloud-instance-security-groups) to set these up.
* Try again once you've opened up the cloud instance firewall rules or security groups to all incoming traffic on all ports