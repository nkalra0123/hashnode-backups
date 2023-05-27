---
title: "Understanding Kafka Producer: How Partition Selection Works"
seoDescription: "understanding Kafka partitions how sticky partition strategy is used"
datePublished: Sat May 27 2023 17:23:57 GMT+0000 (Coordinated Universal Time)
cuid: cli69h2fy000d09l83ci2emnp
slug: understanding-kafka-producer-how-partition-selection-works
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/xbEVM6oJ1Fs/upload/cef16a2851b22fe378df5d43517981d8.jpeg
tags: kafka

---

Apache Kafka is a highly scalable and distributed streaming platform known for its fault-tolerant and high-throughput capabilities. One key component in the Kafka ecosystem is the producer client, responsible for publishing data to Kafka topics. In this blog post, we will dive into the internals of the Kafka producer and explore how it selects the partition to which a message should be sent.

### Understanding Kafka Partitions

To comprehend the partition selection process, it's crucial to first understand Kafka partitions. A Kafka topic is divided into one or more partitions, each representing an ordered and immutable sequence of records. Partitions allow for parallelism and scalability by enabling multiple consumers to read and process messages concurrently.

When a Kafka producer client sends a record to a topic, it needs to decide which partition to send it to. The partition selection algorithm is responsible choosing the partition.

### Partitioning Strategies

Kafka provides flexible partitioning strategies that allow producers to control how messages are distributed across partitions. The choice of partitioning strategy has implications for data distribution, load balancing, and order preservation.

1. Key-based Partitioning: In key-based partitioning, the producer assigns a key to each message. The key is used to determine the target partition. Kafka guarantees that **all messages with the same key will be written to the same partition**, **ensuring order preservation for those messages**. This strategy is suitable when maintaining message order based on a specific key is crucial.
    
2. Round-robin Partitioning: In the absence of a key or if the producer explicitly chooses round-robin partitioning, Kafka will assign the message to a partition in a round-robin fashion. This strategy evenly distributes messages across all partitions, promoting load balancing. However, message ordering cannot be guaranteed as different partitions may receive messages in different orders.
    

In versions of Apache Kafka prior to 2.4, the partitioning strategy for messages without keys involved cycling through the partitions of the topic and sending a record to each one. However, this approach had drawbacks in terms of batching efficiency and potential latency issues.

To address this issue, Apache Kafka version 2.4 introduced a new partitioning strategy called "**sticky partitioning**." This strategy aims to assign records to partitions in a more efficient manner, reducing latency.

With sticky partitioning, records with null keys are assigned to specific partitions, rather than cycling through all partitions. This approach leverages the concept of "stickiness," where records without keys are consistently routed to the same partitions based on certain criteria.

This is dependent on [linger.ms](http://linger.ms) and batch.size properties.

Even when [linger.ms](http://linger.ms) is 0, the producer will group records into batches when they are produced to the same partition around the same time.

For more details check this link : [https://www.confluent.io/blog/apache-kafka-producer-improvements-sticky-partitioner/](https://www.confluent.io/blog/apache-kafka-producer-improvements-sticky-partitioner/)

Custom Partitioning: Kafka allows the implementation of custom partitioners by extending the `org.apache.kafka.clients.producer.Partitioner` interface. Custom partitioners enable the producer to define their own logic for selecting the target partition based on message attributes, algorithms, or business requirements.

**Partition Selection Process:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685206897141/c6fcb658-d663-4198-949b-071e5a20364c.png align="center")

For more details check this video : [A Kafka Client’s Request: There and Back Again by Danica Fine - YouTube](https://www.youtube.com/watch?v=DkYNfb5-L9o&ab_channel=Devoxx)

Understanding how the Kafka producer selects the partition for messages is essential for designing efficient and reliable data pipelines. By leveraging key-based partitioning, round-robin partitioning, or custom partitioners, producers can control data distribution, load balancing, and message ordering according to their specific requirements.

Whether you need strict order preservation based on keys or load balancing across partitions, Kafka's partition selection process offers the flexibility to tailor your message publishing strategy to suit your use case. By optimizing partitioning, you can fully leverage Kafka's scalability and high-throughput capabilities.

Partition selection is just one step that are involved that happens when you call kafkaTemplate.send()

```java
public void sendMessage(String topic, String message) {
        kafkaTemplate.send(topic, message);
        System.out.println("Sent message: " + message + " to topic: " + topic);
    }
```

Other steps that happen, when the producer sends a message and that message is persisted in Kafka brokers and the producer receives the acknowledgement.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685207356977/276698f3-d345-4105-a971-95430997dc1f.png align="center")

Message is converted to a ProducerRecord object before it is sent.

```java
public class ProducerRecord<K, V> {
    private final String topic;
    private final Integer partition;
    private final Headers headers;
    private final K key;
    private final V value;
    private final Long timestamp;
```

DefaultPartitioner strategy

```java
public int partition(String topic, Object key, byte[] keyBytes, Object value, byte[] valueBytes, Cluster cluster,
                         int numPartitions) {
        if (keyBytes == null) {
            return stickyPartitionCache.partition(topic, cluster);
        }
        // hash the keyBytes to choose a partition
        return Utils.toPositive(Utils.murmur2(keyBytes)) % numPartitions;
    }
```

If you liked this blog, [**you can follow me on twitter**](https://twitter.com/nkalra0123), and learn something new with me.