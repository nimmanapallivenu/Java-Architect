# Why low latency & Big Data are high income skills    Java Success.com

## Table of Contents

- [Q1: Why acquire skills & experience in low latency & Big Data?](#q1)
- [Q2: What key words & relevant experiences should you have in your resumes and bring ](#q2)
- [Q3: What are some of the buzz words around low latency & Big Data?](#q3)
- [Q4: Why is streaming data in real-time equally important as batch processing?](#q4)
- [Q5: Why is it necessary to know functional programming (i.e. FP) paradigm?](#q5)

---

## Q1: Why acquire skills & experience in low latency & Big Data?

**Answer:**

10 years ago Spring & Hibernate were sought after skills, and now a days low latency and Big Data skills are more sought-after as modern applications
need to deal with
#1 Response times in milliseconds as opposed to seconds.
#2 Data sizes in Terabytes & Petabytes as opposed to gigabytes. Y ou need to handle not only structured data, but also semi-structur ed data like log files,
event logs, etc and unstructur ed data like PDFs, W ord documents, etc.
#3 Server nodes in 100s with fault-tolerance as opposed to in tens. So be prepared to use terms like the application was deployed to “24 core 100 node cluster
in AWS”, etc.
#4 Architectures are 1) highly distributed , 2) event-driven and 3) asynchronous with more complexities requiring good debugging skills . In event-
driven architectures there is no single code logic that outlines overall behaviour . Debugging challenges include:
a) Out of order processing issues as events are streamed potentially in out of order from various event sources requiring additional logic.
b) Replay considerations as the master data in the HDFS (i.e. Hadoop Distributed File System) raw zone needs to be rerun in batch modes with renewed access
patterns, business logic & rules. Append only systems (E.g. Big Data on HDFS) don’ t update data models, but keep appending to the existing data & evaluate
the current state by 1) deleting the current data models and 2) replaying the event logs (i.e. the master data). These jobs are run as batch jobs with eventual
consistency . This is known as event-sourcing .
c) The NoSQL (i.e. Not Only SQL) databases make development quicker as they are schema-less (i.e. implicit schema), scale rapidly by adding more nodes &
handle lar ge volumes of aggregated data (i.e. you store Order , Line Items, and possibly Customer details together as a JSON clob or key-value pairs), which

minimizes the number of joins. The NoSQL databases don’ t handle ACID (e.g. transaction boundaries, isolation levels, constraints like foreign key , etc)
properties, and shift that responsibility to the application layer . This can add more challenges.
d) The data structuring, partitioning & distribution challenges – Skewed data, data shuf fling, cartesian join prevention, etc. Y ou will see potential challenges and
solutions on the post 15 Apache Spark best practices & performance tuning interview F AQs.
e) Race conditions, performance issues, and out-of-memory issues when dealing with lar ger volumes of immutable & append-only data logs that need to be
replayed into memory or indexed data models for responsiveness & real-time querying.
#5 Deployments & partial service upgrades are more frequent as in once a day or week as opposed to once a month. Micro-services are conducive to
frequent & independent upgrades. Some services can be deployed more frequently to more cluster nodes than other services. Deploying to cloud servers like
AWS allows you to spin more nodes very quickly to adapt to increasing demand.

---

## Q2: What key words & relevant experiences should you have in your resumes and bring up in your job interviews?

**Answer:**

It makes more sense to pay attention to key words like:
real-time processing (i.e. under 200 ms), Near Real T ime (i.e. NR T, 200ms to 2 seconds), low latency , event-driven (i.e. react to events or messages),
scalable (i.e. react to load by adding more nodes to the cluster), Big Data (i.e. data volume in T erabytes or Petabytes), asynchronous processing, streaming
where you process data incrementally as they arrive, Microservices for frequent & independent upgrades, reactive programming where you react to events
or messages, responsive (i.e. react to users fast), resilient (i.e. react to failures).
Distributed data storage (E.g. HDFS, A WS S3, etc), computing (E.g. Apache Spark), and messaging (E.g. Apache Kafka) systems with share nothing
architectur e will be running on 100+ nodes with each node having its own CPU, memory and I/O) and as demand increases more nodes can be added. This is
also known as horizontal scaling .
In distributed systems (E.g. NoSQL databases like HBase, Cassandra, etc) the same piece of data is usually replicated to multiple machines to give fault
tolerance & high availability . When you update that piece of information on one of the machines, it may take some time (usually milliseconds) to reach every
machine that holds the replicated data. This creates the possibility that you might get information that hasn’ t yet updated on the replica. In many scenarios, the
this anomaly is acceptable. In scenarios where this type of behavior is not acceptable, the NoSQL databases may give the the choice, per transaction, whether
data to be eventually consistent or strongly consistent .
In distributed systems the data needs to be partitioned into multiple chunks and placed on dif ferent nodes, so that both the read and the write load gets
distributed. These chunks are called shards or partitions or vnodes, etc. The partitioning needs to be fair , so that each partition gets a similar load of data. If the
partitioning is skewed , a few partitions will handle most of the requests. The partitions which are highly loaded become the bottlenecks for the system, and this
is known as hotspotting .

Key Range partitioning is form of partitioning, where you divide the entire keys into continuous ranges and assign each range to a partition. The downside of
using key range partitioning is that if the range boundaries are not decided properly , it may lead to hotspots. Key Hash Partitioning is a form of partitioning to
apply a hash function on the key which results in a hash value, and then mod it with the number of partitions. The same key will always return the same hash
code. The real problem with hash partitioning starts when you change the number of partitions. Consistent Hashing to the rescue where the output range of a
hash function is treated as a fixed circular space or “ring”. Each node in the system is assigned a random value within this space which represents its “position”
on the ring.
During job interviews bring out these keywords, skills, and experience when responding to open-ended questions like what are some of your recent
accomplishments that you are proud of, etc.

---

## Q3: What are some of the buzz words around low latency & Big Data?

**Answer:**

Reactive Programming (RP), Lambda Architecture, Kappa Architecture, Event-driven architecture, CAP theorem, Event sourcing, Micro-services, Akka
library for scalable applications, Apache Spark libraries based on Akka to process Big Data in both batch and streaming modes, NoSQL databases (E.g.
Cassandra, HBase, etc), columnar data formats (E.g. Parquet, ORC, etc), compression algorithms(E.g. LZO, Snappy , LZ4, etc), BigData friendly data formats
(E.g. A vro, Parquet, Sequence files, etc), Streaming APIs (E.g. Apache Storm, Apache Spark, StAX, etc), Distributed storage systems (E.g. HDFS, A WS S3,
etc), and real-time data pipelines (E.g. Apache Kafka, Amazon Kinesis, etc).

---

## Q4: Why is streaming data in real-time equally important as batch processing?

**Answer:**

The reason for this is that processing big volumes of data has to be fast so that a firm can react to changing business conditions in real time .

---

## Q5: Why is it necessary to know functional programming (i.e. FP) paradigm?

**Answer:**

FP compliments OOP , and can be more robust due to fewer moving parts and fewer or no mutable variables & hidden states if done properly . FP is widely
used in Big data, especially in writing Apache Spark jobs in Scala, Python or Java.
1) FP is all about functions, and focuses on computational problems by evaluating functions to transform a collection of data by focusing on the composition
and application of functions. For example, you apply “map”, “flatMap”, “reduceByKey”, “groupByKey”, etc to a collection of data.
2) FP favors immutability , which is in line with the modern event sourcing models that replay append only event logs to recompute current state.
3) FP shines in building concurr ent applications. OOP languages cannot readily take advantage of multiple processors whereas pure functional programming
can run two or more functions at once as functions are not altering outside state.
4) Debugging & testing can be easier . Debugging is easier as pure functions depend only on their input parameters to produce their output. T esting is easier as
you don’ t have to worry about dealing with hidden state and side effects .
You can learn more about this at Transforming your thinking from OOP to FP , and where does FP shine? .

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
