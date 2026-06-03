# 1 12 High level JEE architecture overviews at 100 feet with diagrams   Java Success.com

## Table of Contents

1. Model-V iew-Contr oller Architectur e
Most web and stand-alone GUI applications follow this pattern. For example, Struts, Spring MVC are server side MVC and angularjs, react.js are client side MVC. The
model represents the core business logic and state. The view renders the content of the model state by adding display logic. The controller translates the interaction with
the view into action to be performed by the model. The actions performed by a model include executing the business logic and changing the state of the model. Based on
the user interactions, the controller selects an appropriate view to render . The controller decouples the model from the view .
MVC Architecture
2. Service Oriented Ar chitectur e (SOA)
The business logic and application state are exposed as reusable services. An Enterprise Service Bus (ESB) is used as an orchestration and mediation layer to decouple
the applications from the services.

SOA (Service Oriented Architecture)
The above architecture has 5 tiers. The application tier could be using a typical MVC architecture. The service orchestration tier could be using ESB products like Oracle
Service Bus, TIBCO, etc and BPM products like Lombardi BPM, Pega BPM, etc. In the above diagram, the ESB integrates with the BPM via messaging queues. The
service tier consists of individual services that can be accessed through SOAP or RESTful web services. The SOA implementation requires change agents to drive
adoption of new approaches. The BPM, application integration, and real-time information all contribute to dynamically changing how business users do their jobs. So, it
needs full support from the business, requiring restructuring and also it can take some time to realize the benefits of SOA. Cloud computing is at the leading edge of its
hype and as a concept compliments SOA as an architectural style. Cloud computing is expected to provide a computing capability that can scale up (to massive
proportions) or scale down dynamically based on demand. This implies a very lar ge pool of computing resources either be within the enterprise intranet or on the Internet
(i.e on the cloud).
3. W eb Oriented Ar chitectur e (WOA)
Q. Differentiate between SOA (Service Oriented Architecture) versus WOA (W eb Oriented Architecture)?
A. WOA extends SOA to be a light-weight architecture using technologies such as REST and POX (Plain Old XML). POX compliments REST . JSON is a variant for
data returned by REST W eb Services. It consumes less bandwidth and is easily handled by web developers mastering the Javascript language

SOA and WOA dif fer in terms of the layers of abstraction. SOA is a system-level architectural style that tries to expose business capabilities so that they can be
consumed by many applications. WOA is an interface-level architectural style that focuses on the means by which these service capabilities are exposed to consumers.
You can start out with a WOA and then grow into SOA.
4. Micr o Services Ar chitectur e (MSA)
Q. Differentiate between SOA (Service Oriented Architecture) versus MSA(Micro Services Architecture)?
A. Microservices allow lar ge systems to be built from a number of collaborating components. The collaborating components can be web services, messaging , event-
driven APIs , Web Sockets, and non-HTTP backed RPC mechanisms. So, A web service can be a microservice, but microservice might not be a web service.
SOA (Service Oriented Ar chitectur e) MSA (Micr oServices Ar chitectur e)
SOA is coarse-grained.MSA is fine-grained. Often behavior & data are encapsulated. Adheres to the
Single Responsibility Principle(SRP).
SOA focuses more on re-usability . SOA has higher coupling, where services
depend on other services to function.MSA focuses more on low coupling & high cohesion . It also emphasizes on
autonomy , where services can be individually developed, deployed, scaled &
monitored to provide business value on its own with both behavior & data . Lower
coupling allows the service to operate independently and high cohesion increases
it’s ability to add value on its own.
Micro Services focus more on “loose coupling & high cohesion” than reuse. The definition of a service is an individual execution unit, and the definition of “ micr o” is
the size, so that the service can be autonomously developed, deployed, scaled & monitored. This means the service is full-stack and has control of all the components like
UI, middleware, persistence, and transaction. The services can be invoked both synchronously and asynchronously .

Microservices deployed to multiple hosts (e.g. on the cloud)
Micro services require a service r egistration as multiple processes working together need to find each other . Spring Cloud is built on Spring Boot, and incorporates the
registration service called “Eureka” built by Netflix.
5. User Interface (UI) Component Ar chitectur e
This architecture is driven by a user interface that is made up of a number of discrete components. Each component calls a service that encapsulates business logic and
hides lower level details. Components can be combined to form new composite components allowing richer functionality . These components can also be shared across a
number of applications. For example, JavaScript widgets, Java Server Faces (JSF) components, etc.
UI Component Architecture
6. RESTful data composition Ar chitectur e

RESTful data composition
The user interface can be built by calling a number of underlying services that are each responsible for building part of a page. The user interface translates and combine
the data in dif ferent formats like XML(translate to HTML using XSL T), JSON (Java Script Object Notation), A TOM (feed for mail messages and calendar applications),
RSS (for generating RSS feeds), etc.
7. HTML composition Ar chitectur e
HTML composition architecture
In this architecture, multiple applications output fragments of HTML that are combined to generate the final user interface. For example, Java portlets used inside a portal
application server to aggregate individual content, server side or client side mashups.

8. Plug-in Ar chitectur e
plugin architecture
In this architecture, a core application defines an interface, and the functionality will be implemented as a set of plug-ins that conform to that interface. For example, the
the Eclipse RCP framework, Maven build tool, etc use this architecture.
9. Event Driven Ar chitectur e (EDA)

EDA
The EDA pattern decouples the interactions between the event publishers and the event consumers. Many to many communications are achieved via a topic, where one
specific event can be consumed by many subscribers. The EDA also supports asynchronous operations and acknowledgments through event messaging. This architecture
requires ef fective monitoring in place to track queue depth, exceptions, and other possible problems. The traceability , isolation, and debugging of an event can be
difficult in some cases. This architecture is useful in scenarios where the business process is inherently asynchronous, multiple consumers are interested in an event(e.g.
order status has changed to partially-filled ), no immediate acknowledgment is required (e.g. an email is sent with the booking details and itinerary), and real-time
request/response is not required (e.g. a long running report can be generated asynchronously and made available later via online or via email).
10. Hybrid Ar chitectur e
Most conceptual architectures use a hybrid approach using a combination of dif ferent architectures discussed above on the benefits of each approach and its pertinence to
your situation. Here is a sample hybrid approach depicting an online trading system.

Hybrid Architecture
11. OLAP (OnLine Analytical Pr ocessing) Ar chitectur e
for business intelligence (BI), data mining, and complex reporting. IBM Cognos, JasperSoft, Oracle Enterprise BI server , etc are OLAP systems. ETL (Extract T ransform
& Load) or EL T (Extract Load & T ransform) operations are performed to move OL TP data from various source systems into data warehouse systems. ETLs can be
performed with Spring batch or commercial tools like Data Stage from IBM, Pentaho and Informatica.

OLTP vs OLAP
12. OLAP BigData Ar chitectur e
for business intelligence (BI), data mining, and complex reporting of BigData in peta bytes as opposed to tera bytes. Handles structur ed (e.g. RDBMS), unstructur ed
(e.g. images, PDFs, etc), and semi structur ed (e.g. log files, XML files) data. As the volume and complexity of data grow , the time to process grows. The traditional

ETL processes struggle to complete within the SLAs. T raditional EL T approach puts excessive loads into data warehouse (DWH). Hadoop can not only send some data
to DWH, and also provides the infrastructure to be queried directly from the HDFS via HBase, Hive, Spark, Storm, etc. for the Big Data to be queried and analyzed.
Hadoop Architecture Overview
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
Next Unit »



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
