### Apache Spark Master Admin Web UI (36060)
```
macosx-laptop$ open http://127.0.0.1:36060
```
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-1.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-2.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-9.png)

### Apache Spark Worker Admin Web UI (36061+)
* If multiple Workers are actively running, the port will start at `36060` and +1 per active Worker
```
macosx-laptop$ open http://127.0.0.1:36061
```
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-7.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-8.png)

### Apache Spark Application (Driver) Admin Web UI (34040+)
* Environment Variables
* Event Timeline
* DAG Visualizations
* System, JVM, and Spark Metrics
* SQL & Streaming Tab
* If multiple jobs are actively running, the port will start at `34040` and +1 per active job
* The links from the Spark Master and Worker Admin UIs are broken, so you may need to manually navigate your browser to the following URL:
```
macosx-laptop$ open http://127.0.0.1:34040
```
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-3.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-4.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-5.png)
![Spark Admin UI](https://s3.amazonaws.com/fluxcapacitor.com/img/flux-spark-6.png)
