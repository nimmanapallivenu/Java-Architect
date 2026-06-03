# 2  Q07   Q12 Java Micro & Web services Interview Q&As   Java Success.com

## Table of Contents

- [Q7: What is a microservice architecture (aka MSA )?](#q7)
- [Q8: What are the key characteristics of microservices?](#q8)
- [Q09: How does SOA (Service Oriented Architecture) dif fer from MSA (i.e. MicroService](#q09)
- [Q10: How do you achieve low latency in microservices ?](#q10)
- [Q11: How do you version your RESTful web services?](#q11)
- [Q12: What are the dif ferent types of RESTful API changes that require newer versioni](#q12)

---

## Q7: What is a microservice architecture (aka MSA )?

**Answer:**

Martin Fowler defines Microservices as a subset of Service Oriented Architecture (SOA). In SOA, the services can be of any size. In a microservice, the service
performs a single function. Micro means small. The term micro is not well defined, but the questions you need to ask are – Are my services small enough?
1) can they be reused in many dif ferent business contexts as possible?
2) can they be individually & frequently deployed?
3) can they be individually scaled?
4) can they be individually monitored?
5) can they be individually developed by dif ferent small teams?
Martin Fowler defines it as “ Independently replaceable, and Independently upgradable. ”

---

## Q8: What are the key characteristics of microservices?

**Answer:**

Focuses more on “loose coupling & high cohesion ” than reuse . The definition of a service is an individual execution unit, and the definition of “ micr o” is the
size, so that the service can be autonomously developed , deployed , scaled & monitor ed. This means the service must be as full-stack as possible and has control of all
the components like UI, middleware, persistence, and transaction. The services can be invoked both synchronously and asynchronously . Micro services must be
organized around business functionalities like Customers, Orders, Invoices, Products, etc.
When routing to micro services via say ESB (i.e Enterprise Service Bus), the ESB should not have any business logic. The ESB should only have routing logic , and the
business logic should be in the service end-points .
Micro services give greater availability as even if some auxiliary services like customer review handling service is down, the main functionality like placing an order can
still go ahead. This also mean that all these services need to be properly monitored.
Different services can be developed using dif ferent platforms like Java/Spring, Scala, .Net, etc. Whilst this flexibility is very handy , it is a good practice to standardize on
a few platforms.

Microservices deployed to multiple hosts (e.g. on the cloud)
Microservices
1) are reusable , but NOT AL WAYS. How do we ensure the highest possible reuse? Make all service so small that they can be reused in many dif ferent business contexts
as possible. Whilst it is possible to have very small services with few or no dependencies on other services, microservices are not always reusable because 1) unlike
libraries that share only behavior , services generally share both behavior and data . 2) By increasing reuse, you are increasing dependencies and coupling as explained
below .
2) are loosely coupled and highly cohesive . All good software designs should strive for loose/low coupling & tight/high cohesion. Every time we reuse, we increase our
coupling. How? If we wrote the code all in one place, there are no dependencies. By reusing code, you’ve created a dependency . The more you reuse, the more
dependencies you have.
Coupling refers to how related are two or more classes/modules and how dependent they are on each other . Loose coupling would mean that changing something major
in one class should not af fect the other . Tight coupling would make your code dif ficult to make changes as making a change to one class/module will require a revamp to
other tightly coupled classes/modules.
High cohesion means a class is focused on what it should be doing. Low cohesion means a class does a great variety of actions.
Learn more at 6 OOP Q&As on encapsulation, coupling & cohesion .
So, applications built from microservices aim to be as decoupled and as cohesive as possible by acting more like filters in UNIX:
3) require a service r egistration as multiple processes working together need to find each other . Spring Cloud is built on Spring Boot, and incorporates the registration
service called “Eureka” built by Netflix.$ ls -l | grep “Dec” | Sort +4n | more
Receiving a request ==> applying a logic as appropriate ==> producing a response | Receiving a request ==> applying a logic as appropriate ==> producing a response |
..

Microservices allow lar ge systems to be built from a number of collaborating components. The collaborating components can be web services, messaging , event-driven
APIs , Web Sockets, and non-HTTP backed RPC mechanisms. So, a web service can be a microservice, but microservice might not be a web service .

---

## Q09: How does SOA (Service Oriented Architecture) dif fer from MSA (i.e. MicroServices Architecture)?

**Answer:**

SOA vs. MSA comparison
SOA (Service Oriented Ar chitectur e) MSA (Micr oServices Ar chitectur e)
SOA is coarse-grained.MSA is fine-grained. Often behavior & data are encapsulated. Adheres to the
Single Responsibility Principle (SRP).
SOA focuses more on re-usability . SOA has higher coupling, where services
depend on other services to function.MSA focuses more on low coupling & high cohesion . It also emphasizes on
autonomy , where services can be individually developed, deployed, scaled &
monitored to provide business value on its own with both behavior & data . Lower
coupling allows the service to operate independently and high cohesion increases
it’s ability to add value on its own.

---

## Q10: How do you achieve low latency in microservices ?

**Answer:**

A cloud based scale out model spins out more servers to improve microservices that don’ t perform well.
You can also scale up by
Favoring in-memory services.
Allocating more heap and tuning the JVM for GC as microservices tend to consume more memory .
Making use of reactive programming techniques relying on asynchronous message passing. Reactive Pr ogramming (RP) in Java Interview Q&A
Owning their own data without sharing with other services.
Avoid caching (or use sparingly) and transactions where applicable.
Using W eb Sockets for better scalability of the real-time web applications. It provides bidirectional & full duplex communication over a single socket that is native
to the browser without any additional overhead or complexity .

---

## Q11: How do you version your RESTful web services?

**Answer:**

1) Via URL & 2) Via HTTP headers.
1) A commonly used way to version your API is to add a version number in the URL .

to move to another API with significant changes
2) Hypermedia way using the “Accept” HTTP headers .

---

## Q12: What are the dif ferent types of RESTful API changes that require newer versioning ?

**Answer:**

There is no single standard approach, but here are some guidelines:
1) Simple format or cosmetic changes: Changing the output format from XML in v1 to JSON in v2. This requires JAXB annotation changes.
2) Modest schema changes. Make the required logic changes without having to create any new packages.
3) Major schema changes. May require a parallel set of packages and artifacts like controllers, services, entities, and possibly database tables.
Java W eb Services interview questions & Answers Links:
1. Java Micro Services & W eb Services Interview Q&As
2. 6 Java RESTful W eb services Interviewapi/v1/user/67
api/v2/user/67
GET /api/v2/user/67 HTTP /1.1
Accept : application /json; version =1.0

3. 11 SOAP W eb service interview | SOAP W eb Service Styles Interview Q&A
4. 5 JAXB interview Questions & Answers
5. 10 Java web services written test questions and answers
6. RESTful W eb services and HA TEOAS Q&A
7. 5 REST constraints (i.e. design rules) interview Q&A

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
