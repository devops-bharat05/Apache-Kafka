# Apache Kafka Installation and Usage Guide

## Overview

Apache Kafka is a distributed event streaming platform capable of handling trillions of events a day. It is used for building real-time data pipelines and streaming applications. Kafka is horizontally scalable, fault-tolerant, and offers high throughput.

This guide covers the installation, configuration, and basic usage of Apache Kafka, along with ZooKeeper, which is used to manage Kafka clusters.

## Prerequisites

- **Java**: Kafka requires Java to be installed on your system. Ensure that Java is installed and properly configured.
  
  ```bash
  yum install java -y
  ```

- **ZooKeeper**: Kafka requires ZooKeeper to manage and coordinate the Kafka brokers.

## Installation

### Step 1: Install ZooKeeper

1. **Download ZooKeeper**:
   ```bash
   wget https://archive.apache.org/dist/zookeeper/zookeeper-3.5.7/apache-zookeeper-3.5.7-bin.tar.gz
   tar -xf apache-zookeeper-3.5.7-bin.tar.gz
   ```

2. **Configure ZooKeeper**:
   - Copy the sample configuration file and edit it:
     ```bash
     cp /kafka/apache-zookeeper-3.5.7-bin/conf/zoo_sample.cfg /kafka/apache-zookeeper-3.5.7-bin/conf/zoo.cfg
     ```
   - Edit the `zoo.cfg` file:
     ```plaintext
     tickTime=2000
     initLimit=10
     syncLimit=5
     dataDir=/tmp/zookeeper
     clientPort=2181
     maxClientCnxns=60
     4lw.commands.whitelist=*
     ```

3. **Start ZooKeeper**:
   ```bash
   cd /kafka/apache-zookeeper-3.5.7-bin/bin
   sh zkServer.sh start
   ```

4. **Check ZooKeeper Status**:
   ```bash
   echo stat | nc localhost 2181
   ```

### Step 2: Install Kafka

1. **Download Kafka**:
   ```bash
   wget https://downloads.apache.org/kafka/3.8.0/kafka_2.13-3.8.0.tgz
   tar -xf kafka_2.13-3.8.0.tgz
   ```

2. **Configure Kafka**:
   - Edit the `server.properties` file located in the `config` directory as per your environment.

3. **Start Kafka Server**:
   ```bash
   sh /kafka/kafka_2.13-3.8.0/bin/kafka-server-start.sh -daemon /kafka/kafka_2.13-3.8.0/config/server.properties
   ```

4. **Verify Kafka Broker**:
   ```bash
   echo dump | nc localhost 2181 | grep brokers
   ```

## Basic Usage

### Creating a Kafka Topic

To create a topic named `myTopic` with 1 partition and a replication factor of 1:

```bash
sh /kafka/kafka_2.13-3.8.0/bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic myTopic --partitions 1 --replication-factor 1
```

### Listing Kafka Topics

To list all topics:

```bash
sh /kafka/kafka_2.13-3.8.0/bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
```

### Describing a Kafka Topic

To describe the configuration of `myTopic`:

```bash
sh /kafka/kafka_2.13-3.8.0/bin/kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic myTopic
```

### Producing Messages

To produce messages to `myTopic`:

```bash
sh /kafka/kafka_2.13-3.8.0/bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic myTopic
```

### Consuming Messages

To consume messages from `myTopic`:

```bash
sh /kafka/kafka_2.13-3.8.0/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic myTopic --from-beginning
```

### Managing Consumer Groups

1. **List all consumer groups**:
   ```bash
   sh /kafka/kafka_2.13-3.8.0/bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
   ```

2. **Describe a consumer group**:
   ```bash
   sh /kafka/kafka_2.13-3.8.0/bin/kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group console-consumer-28586
   ```

## Logs

- **Kafka Logs**: Located at `logs/server.log`.
- **ZooKeeper Logs**: Located at `/apache-zookeeper-3.5.7-bin/logs`.

## Additional Notes

- Kafka supports multiple producers and consumers, allowing for high-throughput message processing.
- Consumer offsets are managed in the `__consumer_offsets` topic, ensuring that the state of message consumption is maintained.

## References

- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [ZooKeeper Documentation](https://zookeeper.apache.org/doc/)

---

This README provides a high-level overview of setting up Apache Kafka with ZooKeeper and performing basic Kafka operations.
```

This `README.md` file is designed to provide users with clear instructions on how to install and use Apache Kafka. It includes steps for installation, basic commands, and references to the official documentation for further details. Save this content to a file named `README.md` and commit it to your Git repository.
