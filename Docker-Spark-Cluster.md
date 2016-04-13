## Stop Spark Local Worker and Master
```
$SPARK_HOME/sbin/stop-slave.sh
stopping org.apache.spark.deploy.worker.Worker

$SPARK_HOME/sbin/stop-master.sh
stopping org.apache.spark.deploy.master.Master
```

## Attach New Spark Local Worker to Remote Master
```
$SCRIPTS_HOME/cluster/start-core-services-only-worker.sh <spark-master-ip>
```

## Check Logs of New Spark Local Worker
```
tail -f /root/pipeline/logs/spark/spark--org.apache.spark.deploy.worker.Worker-1-docker.out
```

## Check Remote Spark Master Admin UI
```
http://<spark-master-ip>:36060
```

## Let's Have Fun With All These Workers!