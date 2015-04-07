# dockerkafka
docker for kafka based on CentoOS latest(7) and JDK7
The kafka version is 0.8.2.1


#Standalone mode:

- Start a zookeeper standalone (or a cluster)

- Modify the zookeeper ip in the config/server.properties

- Run a container with `docker run -d --name kafka -p 9092:9092 --hostname kafka -v /root/docker/kafka/config:/kafka_2.11/config -v /root/docker/kafka/tmp/kafka-logs:/tmp/kafka-logs duffqiu/kafka`

   - note: you need to change the path to your local path at the -v parameter

- At the host where you run the app(consumer client), add the host name "kafka" and its corresponding IP in /etc/hosts. If you are using virtualbox and map the kafka port to local port, set the host name "kafka" to 127.0.0.1

   - note: if you are using virtualbox, please use 127.0.0.1 instead of localhost because you need to set 127.0.0.1 in virtualbox's networking forwarding instead of using localhost in the forwarding rule

#Multiple-broker mode

- Clone the github: ``

- Start a zookeeper standalone (or a cluster)

- Modify the zookeeper ip in the multiple/<id>/config/server.properties

- Modify the kafka's advertised.host.name to the docker0's IP and advertised.port to the docker host's mapping port in the multiple/<id>/config/server.properties

- Start node 1: `docker run -d --name kafka1 -p 9092:9092 --hostname kafka1 -v /root/docker/kafka/multiple/1/config:/kafka_2.11/config -v /root/docker/kafka/multiple/1/tmp/kafka-logs:/tmp/kafka-logs duffqiu/kafka`

- Start node 2: `docker run -d --name kafka2 -p 9093:9092 --hostname kafka2 -v /root/docker/kafka/multiple/2/config:/kafka_2.11/config -v /root/docker/kafka/multiple/2/tmp/kafka-logs:/tmp/kafka-logs duffqiu/kafka`

- Start node 3: `docker run -d --name kafka3 -p 9094:9092 --hostname kafka3 -v /root/docker/kafka/multiple/3/config:/kafka_2.11/config -v /root/docker/kafka/multiple/3/tmp/kafka-logs:/tmp/kafka-logs duffqiu/kafka`

- Limitation: right now only can run the app(consumer/producer) in the docker's host or in the container belonding to the same docker server.(you can use duffqiu/kafka_cmd images to run the topics, consumer, producer's client which are from kafka software packages). Till now, I still can't find the way to run the app in the VM's host or in different docker server's containers.

