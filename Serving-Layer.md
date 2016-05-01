## Setup the Data for Serving Layer
* The following notebook populates the `advancedspark/user-factors-als` and `advancedspark/item-factors-als` ElasticSearch indices
```
http://<ip>:8080/#/notebook/2AUYFSKXN
```

* Start the finagle-based recommendation service
```
$MYAPPS_HOME/serving/finagle/start-finagle-recommendation-service.sh
```
* Verify predictions at the following `/predict/<userId>/<itemId>` endpoint
```
http://<ip>:5080/predict/12663/7
```

## Flask (Python)
* Start the flask-based recommendation service
```
$MYAPPS_HOME/serving/flask/start-flask-recommendation-service.sh
```
* Verify predictions at the following `/predict/<userId>/<itemId>` endpoint
```
http://<ip>:5090/predict/12663/7
```