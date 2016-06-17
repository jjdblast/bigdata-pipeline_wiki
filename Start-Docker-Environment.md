## Download SSH `.pem` File to your Local Laptop
* Put this in the `~/.ssh/` folder
```
mkdir -p ~/.ssh

# MacOS + Linux
wget http://advancedspark.com/keys/pipeline-training-gce.pem ~/.ssh

# Windows
wget http://advancedspark.com/keys/pipeline-training-gce.ppk \Users\<username>\.ssh
```

* Update Permissions
```
# MacOS + Linux
chmod 600 ~/.ssh/pipeline-training-gce.pem

# Windows
chmod 600 \Users\<username>\.ssh\pipeline-training-gce.ppk
```

## Logging Into Your Instance
### Linux/MacOS X
* Username: **pipeline-training**
* Password: **password9** if asked for a password
* Use SSH to log in to your Cloud Instance using the `.pem` file created from the previous step
* You may have to enter the password you used when you created the key pair in an earlier step 
```
# MacOS + Linux
ssh -i ~/.ssh/pipeline-training-gce.pem pipeline-training@<your-cloud-instance-public-ip>
```

### Windows
* Username: **pipeline-training**
* Password: **password9** if asked for a password
* Download [Putty](https://the.earth.li/~sgtatham/putty/latest/x86/putty.exe) 
* Use `putty` to connect to <your-cloud-instance-ip> using `pipeline-training-gce.ppk` 

![Putty Host IP](http://advancedspark.com/img/putty-1.png)

![Putty ppk File](http://advancedspark.com/img/putty-2.png)

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
fluxcapacitor/pipeline   latest              c392786d2afc        3 mins ago          13.17 GB
```

### Start the Docker Container
* You may need to adjust the `-m 48g` memory if you're not on a cloud instance with 50+ GB of RAM
* We highly recommend 50+ GB of RAM
```
sudo docker run -it --privileged --name pipeline --net=host -m 48g fluxcapacitor/pipeline bash
...
WARNING: Your kernel does not support swap limit capabilities, memory limited without swap.
# ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
# IGNORE THIS ERROR IF YOU SEE IT
# ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```
* Hit `enter` a few times if nothing is happening

### Configure the Environment and Start All Services

**Note:  At this point, you are in the Docker Container**

* Run the following commands **inside the Docker Container**
* Note:  You will likely see many WARNINGs and ERRORs - IGNORE THESE
```
cd $PIPELINE_HOME && git pull && source $CONFIG_HOME/bash/pipeline.bashrc && $SCRIPTS_HOME/setup/RUNME_ONCE.sh
```

**Wait a few mins for initialization to complete...  this may take some time.**
**Ignore all errors!!**

### Verify the Initialization
* Verify the output of `jps -l` is *similar to* the following (may differ slightly):
```
jps -l
...
737 org.elasticsearch.bootstrap.Elasticsearch                   <-- ElasticSearch
738 org.jruby.Main                                              <-- Logstash
1987 org.apache.zeppelin.server.ZeppelinServer                  <-- Zeppelin
2243 org.apache.spark.deploy.worker.Worker                      <-- Spark Worker
2123 org.apache.spark.deploy.master.Master                      <-- Spark Master
3479 sun.tools.jps.Jps                                          <-- this (jps)
1529 org.apache.zookeeper.server.quorum.QuorumPeerMain          <-- ZooKeeper
1973 io.confluent.support.metrics.SupportedKafka                <-- Kafka
2555 io.confluent.kafka.schemaregistry.rest.SchemaRegistryMain  <-- Kafka SchemaRegistry
3408 io.confluent.kafkarest.Main                                <-- Kafka REST API
6107 org.apache.flink.runtime.jobmanager.JobManager             <-- Flink Service
2547 org.apache.hadoop.util.RunJar                              <-- Hive Metastore Service (Uses MySQL as backing store)
2908 com.facebook.presto.server.PrestoServer                    <-- Presto Server
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
## Start the Demo Spark Streaming Application
* This command will automatically `tail` the Spark Streaming Application log file
* Keep an eye on this log file during the next step
```
start-spark-streaming.sh
```

## Verify Your Environment
* Navigate your browser to the Demo Home Page
* Follow the steps detailed on the Demo Home Page
* Keep an eye on the Spark Streaming Application log file from the previous step
* (You should see ratings flowing through the Spark Streaming Application log file)
```
http://<your-cloud-instance-public-ip>
```
* Click on the navigation links at the top to familiarize yourself with the tools of the environment

## Troubleshooting
### Cannot Connect to Demo Home Page or Navigation Links?
* Your services are either not started or you have not configured your cloud instance firewall rules (GCE) or security groups (AWS) properly
* Check out [THIS](https://github.com/fluxcapacitor/pipeline/wiki/Troubleshooting-Environment) for further environment troubleshooting