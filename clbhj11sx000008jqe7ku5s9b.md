# Installing Kafka on your machine

Installing kakfa on your local machine

To Install Kafka on Mac Manually, follow these steps:

*   open [https://kafka.apache.org/downloads](https://kafka.apache.org/downloads)
    
*   from the Binary downloads
    
    *   Download the latest version.
        
        ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1670645248478/x49g7ck16.png align="center")
        
        *   Scala 2.13  - [kafka\_2.13-3.3.1.tgz](https://downloads.apache.org/kafka/3.3.1/kafka_2.13-3.3.1.tgz) ([asc](https://downloads.apache.org/kafka/3.3.1/kafka_2.13-3.3.1.tgz.asc), [sha512](https://downloads.apache.org/kafka/3.3.1/kafka_2.13-3.3.1.tgz.sha512))
            
    *   Unzip the file
        

### **Run ZooKeeper & Kafka**

From the root folder of Apache Kafka, you can run ZooKeeper by executing the following command:

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

*   Now, open a new terminal window & run the following command from the root of Apache Kafka to start the Kafka environment:
    

```bash
bin/kafka-server-start.sh config/server.properties
```

### Create topics

Now, open a new terminal window & run the following command from the root of Apache Kafka to start the Kafka environment:

```bash
bin/kafka-topics.sh --bootstrap-server localhost:9092 --create --topic foobar
```

### List topics

```bash
bin/kafka-topics.sh --bootstrap-server localhost:9092 --list
```

### Delete topics

```bash
bin/kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic  foobar
```