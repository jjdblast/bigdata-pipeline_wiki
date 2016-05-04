* This is a complex environment.
* You will see (non-FATAL) warnings and errors.  Most of them are OK.
* We almost always verify a step externally from the browser or CLI.
* You should be alarmed only if this external verification step fails.  Otherwise, we're OK. 
* Here are some of the (non-FATAL) errors you will see along with their (non-FATAL) descriptions

### java.net.BindException: Address already in use

* Every Spark Application binds to an admin port for the admin UI - including Zeppelin which is a long-running Spark Application (similar to Spark Streaming)
* Spark will start at port 4040 and +1 until it finds an open port - throwing a WARN on every attempt
* When you start Zeppelin, for example, this will bind to the first available port after 4040
* We usually start Zeppelin first, so this usually binds to port 4040, but every Spark job after will throw a scary WARN.
* Ignore this

