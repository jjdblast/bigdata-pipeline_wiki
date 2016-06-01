### Cluster Setup
* Everyone runs `stop-all-services.sh`
* Master runs `start-spark-master-only.sh`
* Workers run `start-spark-worker-only.sh`
* Master navigates to `http://<master-ip>:6060` to show all the workers
* Master `spark-submit`'s a large job
* Master navigates to `http://<master-ip>:4040` to show the details of the submitted large job

### `spark-submit` Configuration Notes
* `--num-executors` can exceed the total available executors - the job just won't use them all
* `--executor-memory` cannot exceed a single executor's memory or the job will hang
* Use `--total-executor-cores`, not `--executor-cores`
* `--total-executor-cores` can exceed the total available cores - the job just won't use them all
* Set the number of partitions (last parameter) to 5x the number of `--total-executor-cores`

### SparkPi
```
$SPARK_HOME/bin/spark-submit --class org.apache.spark.examples.SparkPi --master spark://<master-ip>:7077 --num-executors 100 --executor-memory 48g --total-executor-cores 800 $SPARK_HOME/lib/spark-examples*.jar 4000
```

### Similarity Pathways
```
$SPARK_HOME/bin/spark-submit --packages $SPARK_SUBMIT_PACKAGES --class com.advancedspark.ml.graph.SimilarityPathway --master spark://<master-ip>:7077 --num-executors 100 --executor-memory 48g --total-executor-cores 16 /root/pipeline/myapps/spark/ml/target/scala-2.10/ml_2.10-1.0.jar 80
```

### Matrix Factorization (ALS)
```
$SPARK_HOME/bin/spark-submit --packages $SPARK_SUBMIT_PACKAGES --class com.advancedspark.ml.recommendation.ALS --master spark://<master-ip>:7077 --num-executors 100 --executor-memory 48g --total-executor-cores 16 /root/pipeline/myapps/spark/ml/target/scala-2.10/ml_2.10-1.0.jar 80
```

