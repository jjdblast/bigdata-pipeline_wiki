### Apache Spark Master Admin Web UI (6060)
* Admin UI of the single Master node in the Spark Cluster
```
http://<ip>:6060
```
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-1.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-2.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-9.png)

### Apache Spark Worker Admin Web UI (6061+)
* If multiple Workers are actively running, the port will start at `6060` and +1 per active Worker
```
http://<ip>:6061
```
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-7.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-8.png)

### Apache Spark Application (Driver) Admin Web UI (4040+)
* Environment Variables
* Event Timeline
* DAG Visualizations
* System, JVM, and Spark Metrics
* SQL & Streaming Tab
* If multiple jobs are actively running, the port will start at `4040` and +1 per active job
* The links from the Spark Master and Worker Admin UIs are broken, so you may need to manually navigate your browser to the following URL:
```
http://<ip>:4040
```
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-3.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-4.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-5.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-6.png)
