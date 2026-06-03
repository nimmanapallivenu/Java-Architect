# 1 40+ Java web services interview questions and answers

## Table of Contents

- [Q1: What are the dif ferent styles of W eb Services used for application integration](#q1)
- [Q2: Differentiate between SOA (Service Oriented Architecture) versus WOA (W eb Orien](#q2)
- [Q3: How would you decide what style of W eb Service to use? SOAP WS or REST?](#q3)
- [Q4: What tools do you use to test your W eb Services?](#q4)
- [Q5: Why not favor traditional style middle-ware such as RPC, CORBA, RMI and DCOM as ](#q5)
- [Q6: What is the dif ference between SOA and a W eb service?](#q6)

---

## Q1: What are the dif ferent styles of W eb Services used for application integration? and What are the dif ferences between both approaches?

**Answer:**

SOAP WS and RESTful W eb Service. W eb services are very popular and widely used to integrate similar (i.e. Java applications) and disparate systems (i.e.
legacy applications and applications written in .Net etc) as they are language neutral , which means a W eb service client written in Java can consume a web
service written in .Net and vice versa.
Java W eb Service styles comparison

SOAP vs. REST comparison
SOAP W eb service RESTful W eb service
SOAP (Simple Object Access Protocol) is a standard communication
protocol on top of transport protocols such as HTTP , SMTP , Messaging, TCP ,
UDP , etc.REST is an architectural style by which data can be transmitted over
transport protocol such as HTTP(S).
Each unique URL is a some representation of a resource (i.e Object like
Account, Customer , etc), and you can get the contents of the resources (i.e
Objects) via HTTP verb “GET” and to modify via “DELETE”,”POST”, or
“PUT”.
SOAP Layers
 REST Layers
SOAP uses its own protocol and focuses on exposing pieces of application
logic (not data) as services. SOAP exposes operations. SOAP is focused on
accessing named operations, which implement some business logic through
different interfaces.REST is about exposing a public API over the internet to handle CRUD
(Create, Read, Update, and Delete) operations on data. REST is focused on
accessing named resources through a single consistent interface.
SOAP only permits XML data formats. REST permits many dif ferent data formats like XML, JSON data, text, HTML,
atom, RSS, etc. JSON is less verbose than XML and is a better fit for data and
parses much faster .
URL: http://localhost:8080/myapp/createEmptyCase

SOAP
REST content XML, JSON, RSS, etc
SOAP based reads cannot be cached . The application that uses SOAP needs
to provide cacheing.REST based reads can be cached . Performs and scales better .
Supports both SSL security and WS-security , which adds some enterprise
security features. Supports identity through intermediaries, not just point to
point SSL.
— WS-Security maintains its encryption right up to the point where the
request is being processed.
— WS-Security allows you to secure parts (e.g. only credit card details) of the
message that needs to be secured. Given that encryption/decryption is not a
cheap operation, this can be a performance boost for lar ger messages.
— It is also possible with WS-Security to secure dif ferent parts of the message
using dif ferent keys or encryption algorithms. This allows separate parts of the
message to be read by dif ferent people without exposing other , unneeded
information.
— SSL security can only be used with HTTP . WS-Security can be used with
other protocols like UDP , SMTP , etc.Supports only point-to-point SSL security .
— The basic mechanism behind SSL is that the client encrypts all of the
requests based on a key retrieved from a third party . When the request is
received at the destination, it is decrypted and presented to the service. This
means the request is only encrypted while it is traveling between the client and
the server . Once it hits the server (or a proxy which has a valid certificate), it is
decrypted from that moment on.
— The SSL encrypts the whole message, whether all of it is sensitive or not.

Has comprehensive support for both ACID based transaction management for
short-lived transactions and compensation based transaction management for
long-running transactions. It also supports two-phase commit across
distributed resources.REST supports transactions, but it is neither ACID compliant nor can
provide two phase commit across distributed transactional resources as it is
limited by its HTTP protocol.
SOAP has success or retry logic built in and provides end-to-end reliability
even through SOAP intermediaries.REST does not have a standard messaging system, and expects clients
invoking the service to deal with communication failures by retrying.
Which one to favor? In general, a REST based web service is preferred due to its simplicity , performance, scalability , and support for multiple data formats.
SOAP is favored where service requires comprehensive support for security and transactional reliability .
SOA done right is more about RESTFul + JSON, favoring lighter weight approaches to moving messages around than the heavyweight ESBs using
WSDL+XML that gave SOA a bad name.

---

## Q2: Differentiate between SOA (Service Oriented Architecture) versus WOA (W eb Oriented Architecture)?

**Answer:**

WOA extends SOA to be a light-weight architecture using technologies such as REST and POX (Plain Old XML). POX compliments REST . JSON is a
variant for data returned by REST W eb Services. It consumes less bandwidth and is easily handled by web developers mastering the Javascript language
WOA – RESTFul Service Calls via AJAX to populate dif ferent
sections of a UI
SOA and WOA dif fer in terms of the layers of abstraction. SOA is a system-level ar chitectural style that tries to expose business capabilities so that they can
be consumed by many applications. WOA is an interface-level ar chitectural style that focuses on the means by which these service capabilities are exposed to
consumers. Y ou can start out with a WOA and then grow into SOA.

SOA (when SOAP WS in service tier) or WOA (when REST WS in service tier)
According to Nick Gall, “ WOA = SOA + REST + WWW “. In the above diagram from the Service Or chestration tier , which is responsible for loosely
coupling services,
For the SOA => you will be making SOAP style web services in the “ Service T ier“.
For the WOA => you will be making more lighter REST style web services in the “ Service T ier“.

---

## Q3: How would you decide what style of W eb Service to use? SOAP WS or REST?

**Answer:**

In general, a REST based W eb service is preferred due to its simplicity , performance, scalability , and support for multiple data formats. SOAP is favored
where service requires comprehensive support for security and transactional reliability .
The answer really depends on the functional and non-functional requirements. Asking the questions listed below will help you choose.

1) Does the service expose data or business logic? (REST is a better choice for exposing data, SOAP WS might be a better choice for logic).
2) Do consumers and the service providers require a formal contract? (SOAP has a formal contract via WSDL)
3) Do we need to support multiple data formats?
4) Do we need to make AJAX calls? (REST can use the XMLHttpRequest)
5) Is the call synchronous or asynchronous?
6) Is the call stateful or stateless? (REST is suited for statless CRUD operations)
7) What level of security is required? (SOAP WS has better support for security)
8) What level of transaction support is required? (SOAP WS has better support for transaction management)
9) Do we have limited band width? (SOAP is more verbose)
10) What’ s best for the developers who will build clients for the service? (REST is easier to implement, test, and maintain)

---

## Q4: What tools do you use to test your W eb Services?

**Answer:**

SoapUI tool for SOAP WS & RESTFul web service testing and on the browser the Firefox “ poster ” plugin or Google Chrome “ Postman ” extension for
RESTFul services.

---

## Q5: Why not favor traditional style middle-ware such as RPC, CORBA, RMI and DCOM as opposed to W eb services?

**Answer:**

The traditional middle-war es tightly couple connections to the applications. T ightly coupled applications are hard to maintain and less reusable.
Generally do not support heterogeneity . Do not work across Internet and can be more expensive and hard to use.
Web Services support loosely coupled connections . The interface of the W eb service provides a layer of abstraction between the client and the server . The
loosely coupled applications reduce the cost of maintenance and increases re-usability . Web Services present a new form of middle-ware based on XML and
Web. W eb services are language and platform independent. Y ou can develop a W eb service using any language and deploy it on to any platform, from small
device to the lar gest supercomputer . Web service uses language neutral protocols such as HTTP and communicates between disparate applications by passing
XML or JSON messages to each other via a W eb API. Do work across internet, less expensive and easier to use.

---

## Q6: What is the dif ference between SOA and a W eb service?

**Answer:**

SOA is a software design principle and an architectural pattern for implementing loosely coupled, reusable and coarse grained services. Y ou can implement
SOA using any protocols such as HTTP , HTTPS, JMS, SMTP , RMI, IIOP (i.e. EJB uses IIOP), RPC etc. Messages can be in XML or Data T ransfer Objects
(DTOs).

Web service is an implementation technology and one of the ways to implement SOA. Y ou can build SOA based applications without using W eb services – for
example by using other traditional technologies like Java RMI, EJB, JMS based messaging, etc. But what W eb services of fer is the standards based and
platform-independent service via HTTP , XML, SOAP , WSDL and UDDI, thus allowing interoperability between heterogeneous technologies such as J2EE and
.NET .
Java W eb Services interview questions & Answers
1. Java Microservices & W eb Services Interview Q&As
2. 6 Java RESTful W eb services Interview
3. “12 Rules” to write RESTFul W eb Service API
4. 11 SOAP W eb service interview | SOAP W eb Service Styles Interview Q&A
5. 5 JAXB interview Questions & Answers
6. 10 Java web services written test questions and answers
7. RESTful W eb services and HA TEOAS Q&As
8. 5 REST constraints (i.e. design rules) interview Q&A
Spring MVC RESTful W eb Services V ideo T utorial
1. Spring 4 MVC RESTful W eb Service V ideo T utorial .
2. Spring 4 MVC RESTful POST method V ideo T utorial .

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
