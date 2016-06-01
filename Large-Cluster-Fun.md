### `spark-submit` Configuration Notes
* `--num-executors` can exceed the total available executors - the job just won't use them all
* `--executor-memory` cannot exceed a single executor's memory or the job will hang
* Use `--total-executor-cores`, not `--executor-cores`
* `--total-executor-cores` can exceed the total available cores - the job just won't use them all
* Set the number of partitions (last parameter) to 5x the number of `--total-executor-cores`

### SparkPi
* $SPARK_HOME/bin/spark-submit --class org.apache.spark.examples.SparkPi --master spark://tensorflow1.pipeline.io:7077 --num-executors 100 --executor-memory 48g --total-executor-cores 18 $SPARK_HOME/lib/spark-examples*.jar 10

