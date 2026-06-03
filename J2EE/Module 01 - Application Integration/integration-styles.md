# Application Integration Styles

> **Module**: Application Integration  
> **Topic**: Application Integration Styles

---

## 📋 Table of Contents



- [Q1: What are the dif ferent application integration styles?](#q1)
- [Q2: How does a Java EE application integrate with other systems?](#q2)

---

## Q1: What are the dif ferent application integration styles?

**Answer:**

There are a number of dif ferent integration styles like

Integration Styles
1. Shar ed database

2. batch file transfer. E.g ETL (i.e. Extract Transform & Load), and ELT in BigData & data lakes where large quantities of data is first loaded and then
transformed.
3. Invoking remote procedures ( RPC ). RPC is an inter-process communication that allows one program to directly call procedures in another program either
on the same machine or another machine on the network. Spring supports inter -process communication (aka remoting) – Web Service support via JAX-WS and
JAX-RS, which are successors of JAX-RPC, RMI, Spring’ s HTTP Invoker, Hessian, and Burlap.
4. This is also an inter -process communication (aka remoting ) whereby exchanging messages asynchronously over a message oriented middle-ware ( MOM ).
Spring supports both JMS and AMQP. [Asynchronous processing in Java real life examples .]

---

## Q2: How does a Java EE application integrate with other systems?

**Answer:**

Using various protocols like HTTP(S), SOAP, RMI, FTP, TCP, proprietary, etc.
JEE Big Picture
1) SOAP (e.g. Apache CXF) and RESTful (e.g. Apache CXF, RESTEasy, Jersey, etc) Web Services. SOAP uses JAX-WS, and RESTFul uses JAX-RS .
RESTful W eb Service is more prevalent.

RESTFul Service Calls
2) Messaging with JMS. JMS is an interface with which you can switch from one JMS compliant message broker (e.g. W eb Methods) with another one (e.g.
MQSeries or W ebspehreMQ) with little or no changes to your source code. It is like JDBC, which allows you to switch underlying databases.
JMS

3) JavaMail for sending emails and Simplewir e Java SMS library to send SMSs.
4) Overnight batch job runs to load data feeds with Spring batch or the new JEE batch jobs. These are ETL (Extract T ransform and Load) tasks. Y ou use
Hadoop to ingest big data via ELT (Extract Load & T ransform);
5) Using open source integration frameworks like Spring Integration, Mule ESB, Apache Camel, etc. This helps you integrate systems in a standardised
way adhering to the enterprise integration patterns (EIP). Apache Camel is a light weight integration framework that allows you to use HTTP, FTP, JMS, EJB,
JPA, RMI, JMS, JMX, LDAP, and Netty to name a few .

Apache Camel Routes
6) Using an ESB (Enterprise Service Bus) to integrate your applications. For example, Oracle Service Bus, TIBCO ESB, webMethods, Mule ESB, etc. Under
the hood, the ESB also uses an integration framework and provide more services and management functionalities like monitoring, high availability, clustering,
graphical user inteface for routing and configuring, etc. Usually, an ESB is a complex and powerful product with a higher learning curve. Suited for very large

integration projects. Projects requiring BPM (Business Process Managemnt) integration and other integrated services like monitoring, clustering, etc. Mule does
provide proprietary connector support for systems like SAP, Tibco Rendevous, PayPal, Sibel CRM, IBM’ s CICS, etc.
ESB and BPM
7) TCP based socket level integration. MINA is a popular framework for TCP based non blocking socket level communication. MINA is based on EDA
(Event Driven Architecture ). In EDA, Both the “Event” producers and listeners are loosely coupled via an “EventHub” and “Event”. An “EventHub” is used
to register and unregister listeners.

EDA (Event Driven ARchitecture)
Akka is a higher level framework for building event-driven, scalable, fault-tolerant applications. Akka uses reactive programming with its Actor model .
Reactive Programming or Reactor pattern (RP) in Java Interview Q&As | Simple Akka tutorial in Java step by step
RxJava is a reactive pr ogramming library for composing asynchronous and event-based programs by using observable sequences. It is a library with rich set
of Functional Programming operations that let you transform, combine, split and compose data sources.
What is reactive programming?
Reactive programming is all about asynchr onous data flows with principles such as 1) being responsive to react to user requests even under load 2)
Resilient & scalable 3) message driven, which is the foundation for writing scalable, resilient, and responsive systems.
Reactive Programs are of 2 types:
1) Event-driven concurrency: E.g. RxJava. This is based on events, which are monitored by zero or more observers. The big difference between event-driven
style and imperative style is that the caller does not block and hold onto a thr ead while waiting for a r esponse .

2) Message-driven concurrency: E.g. Akka. Actor based where the messages are sent to an Actor via a mailbox. Actors can pass messages back and forth, or
even pass messages to them selves. Apache Spark is a fast and general execution engine for large-scale data developed on the “ Actor Model “. Scala Async
and Actor System Interview Q&As .
Spray is an open-source asynchr onous & actor based toolkit for building REST/HTTP-based integration layers on top of Scala and Akka. It’ s a great way to
integrate your Scala applications.
Netty has NIO at its core, and works at a lower level than Akka. More at a networking level by supporting TCP, UDP, HTTP, FTP, SSL, etc. Akka abstracts out
the networking level for you to focus on the problem domain.
8) Invoking remote pr ocedur es via RMI, Burlap, and Hessian. Burlap/Hessian remote objects are just ordinary Java objects that implement some interfaces.
They don’ t require special proxy, home, or remote classes. One of the inherent benefits of this object-and-interface model is that it promotes the good object-
oriented design practice of design by interface.
9) FIX pr otocol to exchange financial information. FIX stands for Financial Information eXchange, which is an open protocol intended to streamline
electronic communications in the financial securities industry. Most of the exchanges use this standard for communication like sending Order, Executions,
MarketData, etc. QuickFIX/J and CameronFIX are popular FIX frameworks for Java.

FIX to send trades to stock exchange
10) Integration with data-warehouse systems for multi-dimensional reporting. OLTP (OnLine T ransaction Processing) data is summarised and sent to OLAP
(OnLine Analytical Processing) systems for business intelligence, data mining, and complex reporting. IBM Cognos, JasperSoft, Oracle Enterprise BI server, etc
are OLAP systems. Scalable Straight Through Processing System (OL TP) vs OLAP in Java

OLTP vs OLAP
11) Server side and client side mashups. Mer ging of services and content from multiple web sites in an integrated and coherent way is called a mashup.

A server -side mash-up integrates content in the server and pass it to the client. Hence this style of mash-up is also called a proxy-style mash up because the
server acts as a proxy .
Server side mashup
A client side mash-up integrate services and content on the client by mashing up directly with the other web applications’ data or functionality. You also
need to be aware of the cross domain restrictions imposed by JavaScript for Ajax calls and you can overcome these restrictions with JSONP or CORS (Cross
Origin Resource Sharing). The CORS is more industrial strength solution.

Client side mashup
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03