## tl;dr
* This is a complex environment
* You will see some scary (non-FATAL) WARNings and ERRORs
* Please don't panic - Everything is OK!
* We will verify every step of the installation through either a UI or command line as we go along
* Continue reading to see some of the specific (non-FATAL) errors that you will see

## SSH Command Line
### WARNING: Your kernel does not support swap limit capabilities, memory limited without swap.
* Docker throws this WARNING upon initial `docker run`
* Ignore this

### java.net.BindException: Address already in use
* Every Spark Application binds to an admin port for the admin UI - including Zeppelin which is a long-running Spark Application (similar to Spark Streaming)
* Spark will start at port 4040 and +1 until it finds an open port - throwing a WARN on every attempt
* When you start Zeppelin, for example, this will bind to the first available port after 4040
* We usually start Zeppelin first, so this usually binds to port 4040, but every Spark job after will throw a scary WARN.
* Ignore this

### derby.log
* Spark SQL always creates a tiny derby.log file when it first starts up - before finding the real Hive Metastore
* Sometimes this file is created in the same path where you run your Spark Application (ie. `pipeline-spark-shell` Spark Shell)
* Ignore this

### Initial job has not accepted any resources; check your cluster UI to ensure that workers are registered and have sufficient resources
* You likely have started too many long-running Spark Apps like SparkShell, PySparkShell, PySpark (Jupyter/iPython Notebook), Zeppelin Notebook, Spark Streaming, etc.
* Below is a sample screenshot of the Spark Admin UI when you are in this state
* You need to kill some of the long-running Spark Apps through the Spark Admin UI - usually one of the PySpark (Jupyter/iPython) apps
![Not Enough CPUs](http://advancedspark.com/img/spark-ui-not-enough-cpus.png)

### Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
* The optimized Hadoop libraries specific to this OS and Hardware have not been installed
* Ignore this

### WARN ObjectStore: Version information not found in metastore. hive.metastore.schema.verification is not enabled so recording the schema version 1.2.0
* This warning appears to indicate a problem with the Hive Metastore
* I assure you that the Hive Metastore is running properly
* Ignore this

### WARN ObjectStore: Failed to get database default, returning NoSuchObjectException
* Haven't bothered to track this warning down
* It doesn't affect anything
* Ignore this

## Notebooks
### Hanging iPython/Jupyter Notebooks
* Related to the resource-starvation issue
* Every iPython/Jupyter notebook you run will acquire **and hold** 1 CPU
![Not Enough CPUs](http://advancedspark.com/img/spark-ui-not-enough-cpus.png)

### Strange variable assignments
* Notebooks need to be run from top to bottom.
* If you run them out of order - or jump between notebooks - your variable assignments may clobber each other
* Keep an eye on this

### File name too long
* Most Scala-based notebooks operate by generating inner classes for each cell executed within the notebook
* The names of these inner classes, over many executions of the notebook/cell, get very long and exceed the OS filename length
* When you see this, just restart the notebook kernel/interpreter
* After restarting your notebook kernel/interpreter, you'll lose all notebook variable assignments