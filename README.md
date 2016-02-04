### Kafka in Docker
===

This repository provides everything you need to run Kafka in Docker.


### Why?
---
The main hurdle of running Kafka in Docker is that it depends on Zookeeper.
Compared to other Kafka docker images, this one runs both Zookeeper and Kafka
in the same container. This means:

* No dependency on an external Zookeeper host, or linking to another container
* Zookeeper and Kafka are configured to work together out of the box

Run
----

```bash
docker run -d --name kafka -p 2181:2181 -p 9092:9092 --env ADVERTISED_HOST=`docker-machine ip \`docker-machine active\`` --env ADVERTISED_PORT=9092 rahulagrawal/kafka
```

To get into bash shell of the container and smoke test producers and consumers
```bash
docker exec -i -t kafka bash   
```

After we have logged into the bash shell.
```bash   
# cd to Kafka installation directory   
cd /opt/kafka_2.11-0.9.0.0/

# Create a topic
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test

# List the topic that was created
bin/kafka-topics.sh --list --zookeeper localhost:2181

# Send some messages
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test 
This is a message
This is another message

# Consume the messages and display
bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning
This is a message
This is another message

#To Exit Kafka Container:
exit
```

```bash   
To Stop Kafka in a Docker container:
# Stop and delete the Kafka Docker container
docker stop kafka
docker rm -v kafka
```

Alternate way to avoid localhost
```bash
export KAFKA=`docker-machine ip \`docker-machine active\``:9092
bin/kafka-console-producer.sh --broker-list $KAFKA --topic test
```

```bash
export ZOOKEEPER=`docker-machine ip \`docker-machine active\``:2181
bin/kafka-console-consumer.sh --zookeeper $ZOOKEEPER --topic test
```


In the box
----

  The docker image with both Kafka and Zookeeper. Built from the `kafka`
  directory.


----

    docker build -t rahulagrawal/kafka kafka/


