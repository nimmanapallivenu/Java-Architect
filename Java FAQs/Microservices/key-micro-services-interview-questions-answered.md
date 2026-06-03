# 7 Key Micro services Interview Questions Answered   Java Success.com

## Table of Contents

- [Q1: How will you go about choosing REST vs Messaging for Micro Services?](#q1)
- [Q2: How do you handle Micro Services security?](#q2)
- [Q3: How do you handle composition of Micro Services Architecture?](#q3)
- [Q4: What is the key consideration for Micro Services to be deployed independently?](#q4)

---

## Q1: How will you go about choosing REST vs Messaging for Micro Services?

**Answer:**

Micro Services can be invoked both synchronously and asynchronously . Micro services must be or ganized around business functionalities like Customers, Orders, Invoices, Products, etc.
Synchr onous Request/Response & public facing APIs
REST is a great fit for request/response interactions as HTTP(S) network protocol itself Request/Response base. REST are interoperable with every programming language and the message payload can be easily documented using tools such
as Swagger , which is an OpenAPI Specification.
Asynchr onous, Looser coupling & scalable
RESTful/synchronous Micro Services have a number of shortcomings like tight coupling, blocking calls, less scalable, and less fault tolerant. This is where the asynchronous messaging comes to the rescue.
1. Tight Coupling: Each Micro Service should have its own databases and Data MUST not be shared via a database. This rule removes a common cause that leads to tight coupling between services. For example, if two services share the
same database, the second service will break if the first service has changed the database schema. Then teams will have to talk to each other before changing databases, leading to delays, taking us backward.
Q. What if both services need to share the same data?
A. You can replicate the data via messaging, and one service will be the source of truth where update takes place and the other service just reads the data as shown below . The data can be synchronised asynchronously via a messaging queue.
Micro Service should share db
Q. What if both services need to update the data?
A. Either mer ge both the services or use transactions. Distributed transactions can be expensive, an you can use a service compensation. For example, if the second update fails, compensate the first update that has succeeded.
For example, if you are shipping a product, first deduct the money , and then ship the product. If the shipping of the product failed, then refund the money .
2. Blocking calls: When invoking a REST based Micro Service, your service will be blocked waiting for a response for certain long running services. This can adversely impact application performance as this blocked thread could be
processing other requests. Asynchronous messaging based client APIs can send a request to messaging queue or topic and process another request instead of waiting for a response.
3. Scalability: If you need to scale your Micro Services due to increased demand, the ability scale out is one of the key advantages of Micro Services architecture. Event driven architecture and messaging make it much easier for the Micro
Services to scale since they’re decoupled and do not block.

4. Resiliency (i.e Fault tolerant): In REST based Micro Services, the error handling, service retry & service timeout logic need to be incorporated in the code. This tightly couples the services.
Messaging platforms of fer guaranteed delivery , which is very helpful in the event of failures where the messages will not be lost. In the case of service failures the messaging systems allow other healthy services to continue processing as they
are not blocked on the failed service. Once the failed service is restarted, it will start processing the data that had been accumulated in the messaging queues during the downtime. This makes the failed system eventually consistent . This
makes your code simpler & cleaner without any complex error handling & retry logic.
Micro Services – Event Driven (Kafka)

---

## Q2: How do you handle Micro Services security?

**Answer:**

Micro Services and web applications must be stateless and decoupled so that they can be easily deployed and scaled. So, Token based security (i.e. JWT – JSON W eb Token) is used. The resource servers hosting the Micro Services
don’t have to maintain any state with the token based security . Authorization server (i.e. STS – Secured T oken Service) validates the credentials and issue a token to the client whereas the client sends the token back in the request header and
the STS server uses it to verify the authenticity and access of the request. CORS (i.e. Cross-origin resource sharing) can be enabled for the cross domain service calls.

Micro Services – Security
Q. Why token based security?
A. Unlike the cookie based security , which is stateful, the token based security is stateless. The token based security is scalable & decoupled from the service providers. The JWT (i.e. JSON W eb Token) contains all the information to check its
validity , user ’s identity and access details. OAUth 2.0 and OpenID use them to exchange information between the parties. JWT is simpler than SAML 2.0 and supported by all devices and it is more powerful than SWT(Simple W eb Token).
JWT can be encrypted for tamper proof.
Q. What is OAuth 2?
A. OAuth2 is an authorization protocol designed to support a variety of dif ferent client types, which access REST APIs. This includes both applications running on web servers within the enterprise calling out to the cloud as well as
applications running on mobile devices.

---

## Q3: How do you handle composition of Micro Services Architecture?

**Answer:**

In SOA composition was handled via centralized ESB (i.e. Enterprise Service Bus).

ESB and BPM
Micro Services discourage the use of ESB, and promotes the composition of services to take place from 1) UI composition pattern – the client browser 2) Aggr egator pattern – An API Composer that combines the results in memory 3) API
Gateway pattern – A centralised API gateway server .

Micro Service – API Gateway

---

## Q4: What is the key consideration for Micro Services to be deployed independently?

**Answer:**

If Micro Services are to released independently , you must handle the below scenario where
Scenario 1:
“Micro Service 2 was upgraded from version V1 to V2, the Micro Service 1, which used to send a request to V1 will now have to send to V2.”
Scenario 2:
“Micro Service 2 was down graded from version V2 to V1, the Micro Service 3, which used to send a request to V2 will now have to send to V1.”

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
