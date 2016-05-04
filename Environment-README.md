* This is a complex environment.
* You will see (non-FATAL) warnings and errors.  Most of them are OK.
* We almost always verify a step externally from the browser or CLI.
* You should be alarmed only if this external verification step fails.  Otherwise, we're OK. 
* Here are some of the (non-FATAL) errors you will see along with their (non-FATAL) descriptions

## SSH Command Line
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

### Not Enough Resources
![Not Enough CPUs](http://advancedspark.com/img/spark-ui-not-enough-cpus.png)

## Notebooks
### Strange variable assignments
* Notebooks need to be run from top to bottom.
* If you run them out of order - or jump between notebooks - your variable assignments may clobber each other
* Keep an eye on this

### File name too long
* Most Scala-based notebooks operate by generating inner classes for each cell executed within the notebook
* The names of these inner classes, over many executions of the notebook/cell, get very long and exceed the OS filename length
* When you see this, just restart the notebook kernel/interpreter
* After restarting your notebook kernel/interpreter, you'll lose all notebook variable assignments