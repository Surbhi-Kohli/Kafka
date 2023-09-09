https://www.freecodecamp.org/news/apache-kafka-handbook/

## Q n A
### 1.Why do I need to convert json object to byte array

In Kafka, messages are transmitted as sequences of bytes, and both the key and the value of a message are expected to be byte arrays. 
This byte-oriented nature allows Kafka to efficiently handle a wide variety of data formats and to ensure compatibility across different 
programming languages and systems.

Here are the reasons why you need to convert a JSON object (or any data) to a byte array before sending it as a Kafka message:

Binary Transmission:
Kafka is designed to transmit binary data, which is more efficient and flexible for network communication.
Byte arrays provide a common way to represent data in a format that can be easily transmitted and stored.

Serialization:
Converting data to a byte array involves serialization. **Serialization is the process of converting data structures or objects into a 
format that can be easily transmitted or stored**. On the receiving end, deserialization converts the byte array back into 
the original data structure.

Cross-Language Compatibility:
Kafka is a distributed system, and consumers can be written in various programming languages. By using byte arrays, Kafka allows 
different language clients to interact with the same data. This ensures that data can be consumed and produced consistently regardless 
of the language used.

Data Transformation:
Kafka messages can contain various types of data, such as binary data, textual data, numbers, etc. Converting data to byte arrays
enables Kafka to handle a wide range of data formats and types.

Efficient Network Transfer:
Byte arrays provide an efficient way to transfer data over networks. They minimize the overhead associated with representing the data 
and reduce network bandwidth usage.

When you send a JSON object or any other data as a Kafka message, you need to serialize it into a byte array on the producer side. 
Similarly, on the consumer side, you'll need to deserialize the byte array back into the original data structure (e.g., JSON object)
to process the message effectively.

Most Kafka client libraries, including kafkajs, provide built-in serialization and deserialization mechanisms to help you handle 
this process seamlessly. These libraries allow you to work with higher-level data structures like JSON objects while handling the 
underlying byte array conversions transparently.

### 2.If a topic is supposed to have 3 partitions, because of replication , will the topic have more than 3 partitions?

A.Yes, the total number of partitions for a topic in Kafka can be greater than the initial number of partitions you set, due to replication. When replication is enabled, each partition has multiple replicas distributed across different broker servers. As a result, the actual number of partitions (including replicas) for a topic can be greater than the number of primary partitions you initially configured.

Here's how replication affects the partition count:

Initial Partitions:
When you create a topic and specify the number of partitions (e.g., 3), Kafka will create the specified number of primary partitions. These primary partitions are the ones that actually hold the data.

Replica Partitions:
For each primary partition, Kafka creates a configurable number of replicas. Replicas are copies of the data in the primary partition, maintained for replication, fault tolerance, and availability purposes. By default, Kafka creates one replica, but you can configure this with the min.insync.replicas broker configuration setting.

For example, if you set min.insync.replicas=2 and you have 3 primary partitions, Kafka will create two replicas for each primary partition, resulting in a total of six replicas.

Total Partitions:
The total number of partitions (including both primary and replica partitions) for a topic can be calculated by multiplying the number of primary partitions by the number of replicas for each primary partition.

In the example above, if you initially created a topic with 3 primary partitions and 2 replicas for each primary partition, the total number of partitions (including replicas) would be 3 (primary partitions) * 2 (replicas) = 6 partitions.

It's important to note that consumers only read from the primary partitions. Replicas are used for replication and fault tolerance but are not directly consumed from.

When creating topics, you need to consider both the desired number of primary partitions (for parallelism) and the number of replicas (for fault tolerance). The actual number of partitions and replicas may affect the storage and performance characteristics of your Kafka cluster, so it's important to choose these values based on your use case and infrastructure.


