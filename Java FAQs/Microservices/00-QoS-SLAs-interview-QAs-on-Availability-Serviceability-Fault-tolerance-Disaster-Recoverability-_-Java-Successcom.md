# 00  QoS & SLAs interview Q&As on Availability, Serviceability, Fault tolerance & Disaster Recoverability   Java Success.com

## Table of Contents

- [Q1: What are the system qualities that typically form a basis for QoS requirements?](#q1)
- [Q2: How will you go about achieving QoS in your Java application?](#q2)

---

## Q1: What are the system qualities that typically form a basis for QoS requirements?

**Answer:**

Quality of service (QoS) requirements are technical specifications that specify the system quality of features such as
1) Performance : Response times and throughput with respect to concurrent user load conditions. Conduct performance testing against typical data volumes, and monitor cpu, memory , file I/O, etc. Gather stats like response times, number of
invocations, latency , throughput, etc.
2) Availability : Uptime of a system like 24 X 7, etc. E.g. clustering, load balancing for failover – active/active vs active/passive and data replication with master/master & master/slave configurations. Have proper Disaster Recovery (DR)
plans. If site “A” strikes a disaster , switch to site “B”. Perform proper outage & DR testing. A vailability is represented in 9’ s as in 1, 2, 3, 4 or 5 9’ s ==> 90%, 99.0%, 99.9%, 99.99%, & 99.999%, etc. This is also known as the SLA (i.e.
Service Level Agreement).
Availability in 9’ s
There are online SLA calculators as shown below:
SLA Calculator
A system to be highly available , we need to build in Failover with active/active OR passive/passive & Replication with Master/Master OR Master/Slave. Here is an example showing A WS based system design with both Failover &
Replication.

High A vailability with failover & replication [source: https://medium.com/becloudy/architecting-for -reliability-part-3-high-availability-architectures-8dfd0f87d25e]

Master/Master Vs Master/Slave Replication
3) Scalability : Ability to add more capacity . Increase the machine capacity with more CPUs, memory , disk storage, etc (aka vertical scaling or scale in) or add more number of nodes in a cluster (aka horizontal scaling or scale out). Cloud
technologies make horizontal scaling a breeze with auto scaling gr oups , Virtual Machines (aka VMs ), and containerisation technologies like Kubernetes & Docker .
4) Security : Integrity of a system and its users. Involves authentication, authorization, encryption, and non-repudiation.
5) Latent capacity : Ability of the system to handle unusual peak loads. For example, a panic sell scenario in a trading application. Include this scenario in your performance tests.
6) Serviceability : How easily & quickly a system can be maintained, monitored, remediated for problems that arise, and upgraded. Logging & monitoring tools like Splunk, Nagios, ELK (i.e. Elasticsearch, Logstash & Kibana) stack,
profilers like V isualVM, JPrifiler , etc to help identify the health of the overall system. Regular heart beat messages are sent to ensure system availability .
QoS also means taking care of the “ Non Functional requirements “.

---

## Q2: How will you go about achieving QoS in your Java application?

**Answer:**

Adequate monitoring, gathering performance metrics, auditing, security , raising system alerts, clustering and fail over capabilities, etc improve the quality of service provided by the application. The virtualization tools like vmwar e and
application severs that execute the application provides quality of services like reliability , high availability , and clustering. Innovative products like Terracotta, a JVM clustering solution can turn single-node, multi-threaded applications into
distributed, multi-node applications with no code changes. HP NonStop technology provides 24/7 application availability out of the box, enabling the most critical and complex environments to run continuously in a straightforward manner . It
is also vital to have a good disaster recovery plan to ensure the continuity of the business for the mission critical applications. This includes regular back up of data, uninterrupted power supply , off site back up of data, and a back up
infrastructure at a dif ferent location.
The system outage tests need to be carried out to ensure that the clustering and load balancing works as expected. This can be tested by bringing a node down in a cluster , and checking if the requests are sent to a dif ferent node. Some of the
key considerations include sticky versus non-sticky (i.e. stateless) sessions, active/active versus active/passive clusters, and application level fail overs like deadlock retry , service timeout retry , etc. In active/passive mode, all business logic
runs on one node and the “passive” node just waits for a fail-over . In Active/Active mode both nodes are running business logic and should one node fail, the other node will start the failed resources. Dif ferent configurations are possible in a
multi-node environment.
Sticky sessions mean stateful sessions where the requests from the same user will go to the same node/server . Your load balancer should be able to look at IP addresses and HTTP cookies to determine stickiness. W ith sticky sessions
— the initial requests will be distributed evenly , but you might end up with a significant number of users spending more time than others. If all of these users are initially set to a single server , that server will have much more load. This can be
mitigated by having more servers in your cluster .
— You will also lose the ability to take one or more nodes down for maintenance in the event of any system failures. If you are using a sticky session, then you will have to wait until the number of existing sticky connections to drop to an
acceptable level.
So, generally favor non-sticky sessions and stateless services where possible. But, if you must maintain session state, sticky sessions are definitely the way to go and even if you don’ t use session state, stickiness has benefits when it comes to
cache utilization.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
