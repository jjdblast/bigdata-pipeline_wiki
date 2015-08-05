## Troubleshooting Common Errors

### Running out of disk space
* It's likely that you have old, unused containers from each `docker run` command
* These don't get garbage collected automatically as Docker assumes you may want to start them again
* Use the following command to clean them out
```
docker rm `docker ps -aq`
```

NOTE: If docker fills up the root partition of your VM, then docker daemon might not start, in which case running any docker commands will tell you that the daemon is not running.  

1. Confirm out of disk space using `df -l`
2. Blow away the Docker working dirs `sudo rm -rf /var/lib/docker`
3. Pull the pipeline image again and start over.

### Machine "boot2docker-vm" does not exist.
* Run the following to repair your busted boot2docker:
```
macosx-laptop$ sudo /Library/Application\ Support/VirtualBox/LaunchDaemons/VirtualBoxStartup.sh restart
```
* More docs [here](https://github.com/boot2docker/boot2docker#boot2docker-up-doesnt-work-osx)

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
	at com.bythebay.pipeline.spark.streaming.StreamingRatings$.main(StreamingRatings.scala:39)
	at com.bythebay.pipeline.spark.streaming.StreamingRatings.main(StreamingRatings.scala)
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
	at com.bythebay.pipeline.akka.feeder.FeederActor.initData(FeederActor.scala:34)
	at com.bythebay.pipeline.akka.feeder.FeederActor.<init>(FeederActor.scala:23)
```
* You likely have not run `bythebay-config.sh` or `bythebay-setup.sh` as the required datasets have not been uncompressed.