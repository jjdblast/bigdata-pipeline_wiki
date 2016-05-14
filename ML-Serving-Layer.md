## Finagle-based (Scala) Recommendation Service
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

## Flask-based (Python) Recommendation Service
```
$MYAPPS_HOME/serving/flask/start-flask-recommendation-service.sh
```
* Verify predictions at the following `/predict/<userId>/<itemId>` endpoint
```
http://<ip>:5090/predict/12663/7
```

## Flask and TensorFlow-based (Python/C) Image Classification Service
```
$MYAPPS_HOME/tensorflow/start-tensorflow-inception-serving-service.sh
```
* Verify the image classification
```
http://<ip>:5070/classify/tabernacle.jpg

http://<ip>:5070/classify/cropped_panda.jpg
```