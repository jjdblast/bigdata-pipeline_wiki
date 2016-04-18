## Flask (Python)
* Setup the data in the `advancedspark/item-factors-als` ElasticSearch index
```
# TODO:  User Factor Vectors

# TODO:  Item Factor Vectors
```

* Start the flask-based recommendation service
```
root@docker$ $MYAPPS_HOME/serving/flask/start-flask-recommendation-service.sh
```

* Verify predictions at the following `predict/<userId>/<itemId>` endpoint
```
http://127.0.0.1:35090/predict/12663/7
```

## Finagle (Scala/Java)
* Setup the data in the `advancedspark/item-factors-als` ElasticSearch index
```
# TODO:  User Factor Vectors

# TODO:  Item Factor Vectors
```

* Start the flask-based recommendation service
```
root@docker$ $MYAPPS_HOME/serving/finagle/start-finagle-recommendation-service.sh
```

* Verify predictions at the following `predict/<userId>/<itemId>` endpoint
```
http://127.0.0.1:35080/predict/12663/7
```