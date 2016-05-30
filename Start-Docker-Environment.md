## Logging Into Your Instance
### Linux/MacOS X
* Use SSH to log in to your Cloud Instance using the `.pem` file created from the previous step
* You may have to enter the password you used when you created the key pair in an earlier step 
```
ssh -i ~/.ssh/<your-pem-file> <your-username>@<your-cloud-instance-public-ip>
```
### Windows
* Use [Putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

![Putty Host IP](http://advancedspark.com/img/putty-1.png)

![Putty pem File](http://advancedspark.com/img/putty-2.png)

### Start Docker Container

**At this point, you should be ssh'd into your specific cloud instance**

* The assumption is that this is a fresh Cloud Instance with nothing bound to popular ports like 80, 8080, 9090, etc
* This Docker Container will bind to many ports including port 80, so make sure even `apache2` is disabled before running this command

### Verify Docker Images
* Run the following to verify that you have the latest `fluxcapacitor/pipeline` Docker image
```
sudo docker images
...
REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
fluxcapacitor/pipeline   latest              c392786d2afc        3 mins ago          11.17 GB
```

### Start the Docker Container
* Adjust the `-m 48g` memory as needed - the larger, the better!
```
sudo docker run -it --privileged --name pipeline --net=host -m 48g fluxcapacitor/pipeline bash
...
#############################################################################################
# IGNORE THIS ERROR IF YOU SEE IT
WARNING: Your kernel does not support swap limit capabilities, memory limited without swap.
#############################################################################################
```

### Configure the Environment and Start All Services

**At this point, you should be in your Docker Container**

* Run the following commands **inside the Docker Container**
* Note:  You will see many WARNINGs and ERRORs
* Please ignore these
```
cd $PIPELINE_HOME && git pull && source $CONFIG_HOME/bash/pipeline.bashrc && $SCRIPTS_HOME/setup/RUNME_ONCE_LARGE.sh
```

**Wait a few mins for initialization to complete...  this may take some time.**

### Verify the Initialization
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
declare -x PIPELINE_HOME="/root/pipeline"
...
declare -x MYSQL_CONNECTOR_JAR="/usr/share/java/mysql-connector-java.jar"
```

## Verify Your Environment
* Navigate your browser to the Demo Home Page
```
http://<your-cloud-instance-public-ip>
```
* Click on the navigation links at the top to familiarize yourself with the tools of the environment

## Troubleshooting
### Cannot Connect to Demo Home Page or Navigation Links?
* Your services are either not started or you have not configured your cloud instance firewall rules (GCE) or security groups (AWS) properly
* Check out [THIS](https://github.com/fluxcapacitor/pipeline/wiki/Troubleshooting-Environment) for further environment troubleshooting