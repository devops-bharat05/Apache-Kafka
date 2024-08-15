Here is a summary of the contents from the provided document, formatted into a Markdown file (`kafka_installation_guide.md`):

```markdown
# Kafka & Zookeeper Installation Guide

## Installation of Zookeeper

1. **Install Java:**
   ```bash
   yum install java -y
   ```

2. **Download and extract ZooKeeper:**
   ```bash
   wget https://archive.apache.org/dist/zookeeper/zookeeper-3.5.7/apache-zookeeper-3.5.7-bin.tar.gz
   tar -xf apache-zookeeper-3.5.7-bin.tar.gz
   ```

3. **Configure ZooKeeper:**
   - Go to the configuration directory:
     ```bash
     cd /kafka/apache-zookeeper-3.5.7-bin/conf/
     cp zoo_sample.cfg zoo.cfg
     ```
   - Edit the `zoo.cfg` file with the following content:
     ```bash
     tickTime=2000
     initLimit=10
     syncLimit=5
     dataDir=/tmp/zookeeper
     clientPort=2181
     maxClientCnxns=60
     4lw.commands.whitelist=*
     ```

4. **Install necessary tools:**
   ```bash
   yum install nc -y
   yum install net-tools -y
   ```

5. **Start ZooKeeper:**
   - Make sure you are in the `bin` directory:
     ```bash
     sh zkServer.sh start
     ```
   - Check the status of ZooKeeper:
     ```bash
     echo stat | nc localhost 2181
     ```

## Installation of Kafka

1. **Download and extract Kafka:**
   ```bash
   wget https://downloads.apache.org/kafka/3.8.0/kafka_2.13-3.8.0.tgz
   tar -xf kafka_2.13-3.8.0.tgz
   ```

2. **Make changes in `server.properties` file.**

3. **Start Kafka server in the background:**
   ```bash
   sh kafka-server-start.sh -daemon /kafka/kafka_2.13-3.8.0/config/server.properties
   ```

4. **Check Kafka status with brokers:**
   ```bash
   echo dump | nc localhost 2181 | grep brokers
   ```

5. **Logs:**
   - Kafka logs: `logs/server.log`
   - ZooKeeper logs: `/apache-zookeeper-3.5.7-bin/logs`

## Creation of Topics in Kafka

1. **Create a topic:**
   ```bash
   sh kafka-topics.sh --bootstrap-server localhost:9092 --create --topic myTopic --partitions 1 --replication-factor 1
   ```

2. **List all topics:**
   ```bash
   sh kafka-topics.sh --bootstrap-server localhost:9092 --list
   ```

3. **Describe a topic:**
   ```bash
   sh kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic myTopic
   ```

4. **Produce a message to a topic:**
   ```bash
   sh kafka-console-producer.sh --bootstrap-server localhost:9092 --topic myTopic
   ```

5. **Consume messages from a topic:**
   ```bash
   sh kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic myTopic --from-beginning
   ```

6. **Manage consumer groups:**
   - List all consumer groups:
     ```bash
     sh kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
     ```
   - Describe a specific consumer group:
     ```bash
     sh kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group console-consumer-28586
     ```

7. **Consumer Offsets:**
   - Kafka manages all the details in `__consumer_offsets`, keeping records of how many messages are consumed by each consumer.
   ```bash
   sh kafka-topics.sh --bootstrap-server localhost:9092 --list
   ```

## Notes

- Multiple producers can produce messages on topics, and multiple consumers can consume messages from multiple topics.
```

This Markdown file provides a clear and concise guide for installing and configuring Apache Kafka and ZooKeeper, creating topics, and managing consumer groups. You can save this content into a `.md` file and commit it to your Git repository.
