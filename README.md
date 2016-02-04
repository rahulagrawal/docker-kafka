Kafka in Docker
===

This repository provides everything you need to run Kafka in Docker.


Why?
---
The main hurdle of running Kafka in Docker is that it depends on Zookeeper.
Compared to other Kafka docker images, this one runs both Zookeeper and Kafka
in the same container. This means:

* No dependency on an external Zookeeper host, or linking to another container
* Zookeeper and Kafka are configured to work together out of the box

Run
---

```bash
docker run -d  -p 2181:2181 -p 9092:9092 --env ADVERTISED_HOST=`docker-machine ip \`docker-machine active\`` --env ADVERTISED_PORT=9092 rahulagrawal/kafka
```

To get into bash shell of the container and smoke test producers and consumers

```bash
docker exec -i -t kafka bash   
```




```bash
export KAFKA=`docker-machine ip \`docker-machine active\``:9092
kafka-console-producer.sh --broker-list $KAFKA --topic test
```

```bash
export ZOOKEEPER=`docker-machine ip \`docker-machine active\``:2181
kafka-console-consumer.sh --zookeeper $ZOOKEEPER --topic test
```


In the box
---

  The docker image with both Kafka and Zookeeper. Built from the `kafka`
  directory.


---

    docker build -t rahulagrawal/kafka kafka/


