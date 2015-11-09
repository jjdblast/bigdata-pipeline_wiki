## Common Errors

### Running out of disk space
* It's likely that you have old, unused containers from each `docker run` command
* These don't get garbage collected automatically as Docker assumes you may want to start them again
* Use the following command to clean them out
```
docker rm `docker ps -aq`
```

### Your kernel does not support swap limit capabilities, memory limited without swap
* Just ignore this error

### Verify boot2docker Disk and Memory Settings are Correct
```
macosx-laptop$ boot2docker ssh

boot2docker$ df -h
boot2docker$ cat /proc/meminfo
```

NOTE: If docker fills up the root partition of your VM, then docker daemon might not start, in which case running any docker commands will tell you that the daemon is not running.  

1. Confirm out of disk space using `df -l`
2. Blow away the Docker working dirs `sudo rm -rf /var/lib/docker`
3. Pull the pipeline image again and start over.

### Are you trying to connect to a TLS-enabled daemon without TLS?  
* Make sure you've run the following
```
eval $(boot2docker shellinit)
```
You should also add the following exports the following in your MacOS X `~/.bash_profile`
```
export DOCKER_TLS_VERIFY=1
export DOCKER_HOST=tcp://192.168.59.103:2376
export DOCKER_CERT_PATH=/Users/[YOUR_USERNAME]/.boot2docker/certs/boot2docker-vm
```

### java.nio.channels.ClosedChannelException
```
java.nio.channels.ClosedChannelException
	at org.apache.spark.streaming.kafka.KafkaUtils$$anonfun$createDirectStream$2.apply(KafkaUtils.scala:416)
	at org.apache.spark.streaming.kafka.KafkaUtils$$anonfun$createDirectStream$2.apply(KafkaUtils.scala:416)
	at scala.util.Either.fold(Either.scala:97)
	at org.apache.spark.streaming.kafka.KafkaUtils$.createDirectStream(KafkaUtils.scala:415)
	at com.fluxcapacitor.pipeline.spark.streaming.StreamingRatings$.main(StreamingRatings.scala:39)
	at com.fluxcapacitor.pipeline.spark.streaming.StreamingRatings.main(StreamingRatings.scala)
```
* You likely have not started your services using `flux-start.sh`.
* Or there was an issue starting your Spark Master and Worker services. 

### Caused by: java.io.FileNotFoundException: datasets/dating/ratings.csv (No such file or directory)
```
Caused by: java.io.FileNotFoundException: datasets/dating/ratings.csv (No such file or directory)
	at java.io.FileInputStream.open(Native Method)
	at java.io.FileInputStream.<init>(FileInputStream.java:146)
	at scala.io.Source$.fromFile(Source.scala:90)
	at scala.io.Source$.fromFile(Source.scala:75)
	at scala.io.Source$.fromFile(Source.scala:53)
	at com.fluxcapacitor.pipeline.akka.feeder.FeederActor.initData(FeederActor.scala:34)
	at com.fluxcapacitor.pipeline.akka.feeder.FeederActor.<init>(FeederActor.scala:23)
```
* You likely have not run `flux-config-before-starting-services.sh` as the required datasets have not been uncompressed.

### Failed to initialize machine "boot2docker-vm": exit status 1
* Run the following to repair your busted boot2docker:
```
macosx-laptop$ sudo /Library/Application\ Support/VirtualBox/LaunchDaemons/VirtualBoxStartup.sh restart
```
More docs [here](https://github.com/boot2docker/boot2docker#boot2docker-up-doesnt-work-osx)

* Re-run the following including the `-v` flag
```
macosx-laptop$ boot2docker stop
macosx-laptop$ boot2docker destroy
macosx-laptop$ boot2docker -v --memory=8192 --disksize=20000 init
macosx-laptop$ boot2docker up
```

* You likely need to remove an existing directory and re-initialize boot2docker:
```
macosx-laptop$ rm -rf /Users/<user-name>/.boot2docker/certs/boot2docker-vm/
macosx-laptop$ boot2docker stop
macosx-laptop$ boot2docker destroy
macosx-laptop$ boot2docker -v --memory=8192 --disksize=20000 init
macosx-laptop$ boot2docker up
```

### TaskSchedulerImpl: Initial job has not accepted any resources; check your cluster UI to ensure that workers are registered and have sufficient resources
* You likely have not configured your VM environment to have enough cores to run the Spark jobs
* Check spark-defaults.conf has the following:
```
spark.executor.cores=2
spark.cores.max=2
```
* Check Number of CPU Cores for your Docker Container
```
lscpus
```
* Check the Spark Admin UI to see if a Job is pending/waiting for resources
```
macosx-or-windows-linux-laptop$ open http://<ip-address-of-docker-container>:36060
```

### Help Me!
Feel free to email help@fluxcapacitor.com for help.  We love to help!