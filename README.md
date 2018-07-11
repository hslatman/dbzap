# Debezium POC

A simple proof of concept with Debezium and API Platform

## Starting

The topology can be started using docker-compose.
I've based the docker-compose.yaml file on the example supplied by [Debezium](https://github.com/debezium/debezium-examples/blob/master/tutorial/docker-compose-postgres.yaml).
Below are the instructions for running the minimal application.

```bash
# Set version of Debezium, or set it in a .env file (copy it from .env.dist)
$ export DEBEZIUM_VERSION=0.8

# Starting the topology
$ docker-compose up


# Start Postgres connector. Change localhost to IP of Docker machine when using that
$ curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-postgres.json

# Consume messages from a Debezium topic using built-in Kafka consumer
$ docker-compose exec kafka /kafka/bin/kafka-console-consumer.sh \
    --bootstrap-server kafka:9092 \
    --from-beginning \
    --property print.key=true \
    --topic dbserver1.public.greeting

# Modify records in the database via Postgres client
$ docker-compose exec postgres env PGOPTIONS="--search_path=inventory" bash -c 'psql -U $POSTGRES_USER postgres'

# Shut down the cluster
$ docker-compose down
```
