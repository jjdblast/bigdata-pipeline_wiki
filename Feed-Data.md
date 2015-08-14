The following Akka App puts data from `datasets/dating/ratings.csv` onto the `ratings` Kafka topic to simulate real-time streaming ratings from users.
```
root@docker$ cd $PIPELINE_HOME && ./flux-akka-ratings-producer.sh
...Starting Ratings Producer...
```