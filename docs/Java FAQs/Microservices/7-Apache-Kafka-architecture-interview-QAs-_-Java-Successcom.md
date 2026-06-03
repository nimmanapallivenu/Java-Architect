# 7 Apache Kafka architecture interview Q&As   Java Success.com

## Table of Contents

- [Q1: What is Apache Kafka?](#q1)
- [Q2: Where is Apache Kafka used?](#q2)

---

## Q1: What is Apache Kafka?

**Answer:**

Apache Kafka is a distributed messaging broker . The purpose of the Kafka project is to provide a unified, high-throughput, and low latency system platform for real-time data processing . Kafka delivers the following three key
functions:
1) Publish/Subscribe paradigm: as Kafka publishes & subscribes to streaming data similar to other traditional messaging systems like Active MQ, Rabbit MQ, W ebsphere MQ, etc.
2) Processing: as Kafka compiles a stream processing application and responds to real-time events.
3) Storage: as Kafka securely stores streaming data in a distributed and fault-tolerant cluster .
Q. How does Kafka dif fer from the other traditional messaging systems like Active MQ or Rabbit MQ?
A. Apache Kafka is used as streaming platform , which entails messaging + distributed storage + processing of data . Kafka can be used as a traditional messaging system, but the reverse is not true.
1) Publish-Subscribe paradigm: In Kafka there is no concept of Queue and hence no send or receive for putting/getting messages from the queue. Publish-subscribe is the only paradigm available as a messaging model.
2) Message persistence: In traditional messaging systems once a message is read it is removed from the storage and is no longer available. Kafka retains the messages even after all the subscribers have read the message . The retention period
is a configurable parameter .
3) Topic partitioning Kafka has implemented the topics as partitioned logs. A partition is an ordered, immutable sequence of messages that is continually appended to. A partition is also known as a commit log .
Kafka – T opic Partitioning (Source: Dataflair)
4) Message sequencing: In traditional messaging systems there is no guarantee that the messages will be received in a sequence in which they are sent. Also, the messages can only be read First In First Out (FIFO)
In Kafka the sequence is maintained at a partition level. In other words if the topic is configured with a single partition then the messages are received in the same order that they were sent in. The consumer of the messages in Kafka issues a
fetch request to the broker leading the partition it wants to consume. This means messages can be fetched by specifying the of fset from which the message in the log is read from.

Kafka – Commit log of fsets
5) Fault-tolerance & high availability: In Kafka partitions for the same topic are distributed acr oss multiple br okers in the cluster to give fault tolerance & high availability .
Partitions are replicated across multiple servers, and this is a configurable replication parameter .

Kafka – Partition Replication
Each Partition has one server as a leader and a number of servers as followers. Each Server acts a leader for some of its partitions and as a follower for some other . This is managed via Apache Zookeeper .
Kafka chooses one broker partition’ s replica as leader using ZooKeeper . A follower that is in-sync is called an ISR (in-sync replica). If a partition leader fails, Kafka chooses a new ISR as the new leader .
6) Load balancing: as Kafka nodes publish the metadata telling the producer which servers are alive in the cluster , where the leader for the partitions is, and allows the client to send message to the appropriate server (and partition) thus
distributing the message load across the cluster .
Learn more: Getting started with Apache Kafka tutorial

---

## Q2: Where is Apache Kafka used?

**Answer:**

It is used in event driven architectures from Micro services to Big Data & low latency applications.
1. Micr o Services
The diagram depicts how to coordinate a set of decoupled and independent services on Apache Kafka message broker as a scalable, highly available, and fault-tolerant asynchronous communication backbone. Use in scenarios where
1) You have a lar ge number of Micro Services that need to communicate asynchronously .
2) You want your Micro Services to be decoupled and independently maintained.
3) You have one or more producers that produce messages for many consumers.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
