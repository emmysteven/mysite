+++
title = 'How to Use Apache Kafka in Spring Boot'
slug = 'spring-boot-kafka'
date = 2025-04-15T11:09:51+01:00
draft = false
description = 'Dive into Apache Kafka integration with Spring Boot effortlessly! Kickstart your journey to seamless messaging and data streaming in just a few steps.'
[cover]
image = 'img/00-spring-boot-kafka.png'
alt = 'Spring Boot with Apache Kafka'
caption = 'Spring Boot with Apache Kafka'
+++

### What is Apache Kafka?
Kafka is an open-source software tool used for sending and receiving messages between different computer programs. It was created by some super smart engineers at LinkedIn and then made available to everyone else through the Apache Software Foundation. They wanted to make sure it could handle really big amounts of data quickly and reliably, so it works great for apps that need to process lots of information right away.

### Why Use Apache Kafka?
Kafka's design allows it to be versatile and effective in different situations, making it a valuable tool across the board.

- **Real-time Data Processing:** Kafka is great for handling lots of data all at once, especially when that data comes in quickly and needs to be processed right away. It's like a superhero for real-time data processing, able to handle big tasks with lightning speed and accuracy.

- **Scalability:** Its distributed nature enables seamless scalability, allowing you to handle large volumes of data without sacrificing performance.

- **Fault Tolerance:** Kafka's redundancy system guarantees that information remains accessible even when server breakdowns occur, protecting against loss of data.

- **Event Sourcing:** Kafka serves as a crucial element in event sourcing architectures, providing a systematic approach to capturing and storing changes within an application's state through a sequence of events.

- **Log Aggregation:** Kafka acts as a crucial component in documenting and saving the evolution of an application's condition across time, presenting these adjustments in a systematic sequence of occurrences.

### Key Concepts of Kafka
**Topics:** In Kafka, data is organized into topics, these topics act like labels or channels that help manage the flow of information by grouping related messages to be published.

**Producers:** Producers send data to Kafka topics, essentially serving as the source of data streams.

**Consumers:** Consumers subscribe to topics and process the messages published by producers. They are the recipients of the data streams.

**Brokers:** Kafka clusters consist of brokers that store data and manage the distribution of messages across topics.

**Partitions:** Each topic can be divided into partitions, which allow for parallel processing and the distribution of data across the cluster.


### USE CASES OF KAFKA
**Message Queuing:** Kafka is a distributed large-scale publish-subscribe message processing application.

**Website Activity Tracking:** To be able to rebuild a user activity tracking pipeline as a set of real-time publish-subscribe feeds, it is the original Use Case for Kafka.

**Metrics:** For operational monitoring data, to produce centralized feeds of operational data, it includes aggregating statistics from distributed applications.

**Log Aggregation:** In order to collect logs from multiple services and make them available in a standard format to multiple consumers, we can use Kafka across an organization.

**Stream Processing:** Many users of Kafka process data in processing pipelines consisting of multiple stages, where raw input data is consumed from Kafka topics and then aggregated, enriched, or otherwise transformed into new topics for further consumption or follow-up processing.

**Commit Log:** While it comes to a distributed system, Kafka can serve as a kind of external commit-log for it. Generally, it replicates data between nodes. Also, acts as a re-syncing mechanism for failed nodes to restore their data. The feature of log compaction in Kafka helps to support this usage.

///// Diagram here


### Topics
According to the Kafka documentation, A topic is a category or feed name to which records are published. They have the following characteristics:
- A Topic is identified by its name.
- Messages are sent to a topic and read from the same topic
- Topics can accommodate multiple subscribers simultaneously, allowing for flexibility in how data is consumed. This means that a single topic can have either no subscribers, one subscriber, or numerous subscribers interested in receiving updates.
- An Application that sends data to one or more Kafka topics is referred to as a `Producer` and an application that reads data from the topic is referred to as a `Consumer`.
- Data in topics are stored for a specific time known as TTL(Time to Live). This TTL can be adjusted.
- A topic is then divided into partitions, where each contains a subset of a topic’s messages.

### Partitions
- Topics are usually slitted into partitions. While creating a topic, you can specify the number of partitions it should have.
- Every partition is arranged in a specific sequence i.e. they are ordered
- Within each partition, order is ensured, but there is no guarantee of order across partitions.
- Each record contained within a partition is assigned an incremental id called the offset.
- offsets refer to specific points of reference when it comes to partitions. They do not hold any inherent value or meaning on their own but rather serve as a point of comparison within the context of a particular partition.
- Once data has been stored on a partition, it becomes immutable, meaning it cannot be altered or modified in any way.
- Multiple partitions exist primarily to increase throughput; concurrent access to the topic can occur.

### Brokers
A Broker is nothing but a server, it has the following characteristics:
- A Kafka cluster is made of multiple brokers
- Every Broker is known by an ID.(broker.id)
- Once a connection is established to a broker, you get connected to the whole Kafka cluster.
- Brokers usually hold the topic and the partition(s).

The below image has 2 Brokers, Topic 1 has 2 partitions and Topic 2 has one partition.

//////// Diagram

### Replication Factor
- Kafka is a distributed system, what this means is that if one of its brokers fails or becomes unavailable, the data remains accessible and intact due to the concept of replication.
- It is advisable to have a topic that has a replication factor greater than 1. It is recommended to have 3 partitions. This is used only for Kafka-to-Kafka workflows.
- A replication factor of 3 implies that there will be three duplicates of data stored across three separate servers.

### Leader for a Partition.
Each partition will have a Leader and multiple ISR(in-sync replicas). An ISR is the number of redundant copies apart from the one that exists on the leader. Only a leader can receive and serve data for that partition.
If a leader goes down, then one of ISR becomes the leader. When the leader comes back, the temporary leader goes back to ISR.


### Producers
Producer is an application that sends data to topics. they know which broker and partition to write to. If you send a message without specifying the key, the messages will get to brokers in the round robin. This ensures equal load on every broker i.e. load balancing. However, if you do send a message with a key, the message for that key will be stored in the same partition.

When data is sent to brokers, a producer can be configured to receive confirmation of messages.

ack=0
No acknowledgment, this has possible data loss.

ack=1
Only a leader will send acknowledgment, it would have limited data loss.

ack=all
All the ISR and leader will send acknowledgment, No data loss but a slower speed of processing.



