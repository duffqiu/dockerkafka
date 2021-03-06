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

- Clone the github: `https://github.com/duffqiu/dockerkafka.git`

- Start a zookeeper standalone (or a cluster)

- Modify the zookeeper ip in the multiple/{id}/config/server.properties

- Modify the kafka's advertised.host.name to the docker0's IP and advertised.port to the docker host's mapping port in the multiple/{id}/config/server.properties

- Start node 1: `docker run -d --name kafka1 -p 9092:9092 --hostname kafka1 -v /root/docker/kafka/multiple/1/config:/kafka_2.11/config -v /root/docker/kafka/multiple/1/tmp/kafka-logs:/tmp/kafka-logs duffqiu/kafka`

- Start node 2: `docker run -d --name kafka2 -p 9093:9092 --hostname kafka2 -v /root/docker/kafka/multiple/2/config:/kafka_2.11/config -v /root/docker/kafka/multiple/2/tmp/kafka-logs:/tmp/kafka-logs duffqiu/kafka`

- Start node 3: `docker run -d --name kafka3 -p 9094:9092 --hostname kafka3 -v /root/docker/kafka/multiple/3/config:/kafka_2.11/config -v /root/docker/kafka/multiple/3/tmp/kafka-logs:/tmp/kafka-logs duffqiu/kafka`

   - note: you need to change the path to your local path at the -v parameter

   - note: if you want to run the app outside the docker's host or the container is not under the same the docker, you need to use the bridge network mode in the virtualbox.

   - note: you need to make sure you have enough memory for each kafka container (1G at lease)
