# 7  11 SOAP Web service interview Q&As   Java Success.com

## Table of Contents

- [Q1: What are the dif ferent approaches to developing a SOAP based W eb service?](#q1)
- [Q2: What are the pros and cons of each approach, and which approach would you prefer](#q2)
- [Q3: A web service protocol stack from bottom to top consists of
a) HTTP , SOAP , des](#q3)
- [Q4: What are the key roles played in a W eb service
a) Service provider , Service re](#q4)
- [Q5: Which of the following are in the WSDL document structure? </m:GetPrice >
</soap](#q5)
- [Q6: What are the dif ferent types of operations available in a WSDL?
a) One-way
b) R](#q6)
- [Q7: What is the dif ference between Request-Response and Solicit-Response?](#q7)
- [Q8: Which of the following are true?
a) SOAP is a protocol and REST is a concept wit](#q8)
- [Q9: Though both RESTful web series and SOAP web service can operate cross platform, ](#q9)
- [Q10: Which of the following statements are correct?
a) JAX-WS is an API for SOAP base](#q10)
- [Q11: What is JAX-WS and why does it replace JAX-RPC?](#q11)

---

## Q1: What are the dif ferent approaches to developing a SOAP based W eb service?

**Answer:**

There are 2 approaches.
1) The contract-first appr oach , where you define the contract first with XSD and WSDL and the generate the Java classes from the contract.
2) The contract-last appr oach where you define the Java classes first and then generate the contract, which is the WSDL file from the Java classes.
Note: The WSDL describes all operations that the service provides, locations of the endpoints (i.e.e where the services can be invoked), and simple and complex
elements that can be passed in requests and responses.

---

## Q2: What are the pros and cons of each approach, and which approach would you prefer?

**Answer:**

Contract-first W eb service
PROS:
a) Clients are decoupled from the server , hence the implementation logic can be revised on the server without af fecting the clients.
b) Developers can work simultaneously on client and server side based on the contract both agreed on.
c) You have full control over how the request and response messages are constructed — for example, should “status” go as an element or as an attribute? The contract
clearly defines it. Y ou can change OXM (i.e. Object to XML Mapping) libraries without having to worry if the “status” would be generated as “attribute” instead of an
element. Potentially , even W eb service frameworks and tool kits can be changed as well from say Apache Axis to Apache CXF , etc
CONS:
a) More upfront work is involved in setting up the XSDs and WSDLs. There are tools like XML Spy , Oxygen XML, etc to make things easier . The object models need
to be written as well.
b) Developers need to learn XSDs and WSDLs in addition to just knowing Java.
Contract-last W eb service
PROS:
a) Developers don’ t have to learn anything related to XSDs, WSDLs, and SOAP . The services are created quickly by exposing the existing service logic with
frameworks/tool sets. For example, via IDE based wizards, etc.
b) The learning curve and development time can be smaller compared to the Contract-first W eb service.

CONS:
a) The development time can be shorter to initially develop it, but what about the on going maintenance and extension time if the contract changes or new elements
need to be added? In this approach, since the clients and servers are more tightly coupled, the future changes may break the client contract and af fect all clients or
require the services to be properly versioned and managed.
b) In this approach, The XML payloads cannot be controlled. This means changing your OXM libraries could cause something that used to be an element to become
an attribute with the change of the OXM.
So, which appr oach will you choose? The best practice is to use “contract-first” as the contract-last can be more fragile. Y ou will have to decide what is most
appropriate based on your requirements, tool sets you use, etc.

---

## Q3: A web service protocol stack from bottom to top consists of
a) HTTP , SOAP , description language, UDDI
b) SMTP , XML messaging, WSDL, Service discovery
c) HTTP , XML messaging, WSDL, UDDI
d) HTTP , XML-RPC, WSDL, UDDI
e) HTTP , WSDL, SOAP , UDDI

**Answer:**

The answer is a,b,c, and d. The e is not right because of the order .
This is an evolving standard, but the basic W eb service protocol stack is (aka web service components) comprised of
Service transport is the lowest layer in the stack, and is responsible for transporting messages between applications. Currently , this layer includes hypertext transfer
protocol (HTTP), Simple Mail T ransfer Protocol (SMTP), file transfer protocol (FTP), and newer protocols, such as Blocks Extensible Exchange Protocol (BEEP).
XML messaging layer is responsible for encoding messages in a common XML format so that messages can be understood at either end. Currently , this layer
includes XML-RPC and SOAP .
Service description layer responsible for describing the public interface to a specific web service. Currently , service description is handled via the W eb Service
Description Language (WSDL or W ADL[for RESTful]).
Service discovery layer is responsible for centralizing services into a common registry , and providing easy publish/find functionality . Currently , service discovery is
handled via Universal Description, Discovery , and Integration (UDDI).
SOAP stands for Simple Object Access Protocol. It is an XML based lightweight protocol, which allows software components and application components to
communicate, mostly using HTTP (can use SMTP etc). SOAP sits on top of the HTTP protocol. SOAP is nothing but XML message based document with predefined
format. SOAP is designed to communicate via the Internet in a platform and language neutral manner and allows you to get around firewalls as well.

SOAP Message
SOAP Request:
POST /Price HTTP /1.1
Host : www .mysite .com
Content -Type: application /soap+xml; charset =utf-8
Content -Length : 300
<?xml version ="1.0" ?>
<soap:Envelope
xmlns :soap="http://www .w3.or g/2001/12/soap-envelope"
oap:encodingStyle ="http://www .w3.or g/2001/12/soap-encoding" >
<soap:Body >
 <m:GetPrice xmlns :m="http://www .mysite.com/prices" >
 <m:Item>PlasmaTV </m:Item>

SOAP Response:

---

## Q4: What are the key roles played in a W eb service
a) Service provider , Service requester , Service registry
b) producer , consumer , Service registry
c) producer , consumer
d) publish, bind, find

**Answer:**

. The answer is a,b,c, and d.

---

## Q5: Which of the following are in the WSDL document structure? </m:GetPrice >
</soap:Body >
</soap:Envelope >
HTTP /1.1 200 OK
Content -Type: application /soap; charset =utf-8
Content -Length : 200
<?xml version ="1.0" ?>
<soap:Envelope
xmlns :soap="http://www .w3.or g/2001/12/soap-envelope"
oap:encodingStyle ="http://www .w3.or g/2001/12/soap-encoding" >
<soap:Body >
 <m:GetPriceResponse 
 xmlns :m="http://www .mysite.com/prices" >
 <m:Price >3500.00 </m:Price >
 </m:GetPriceResponse >
 </soap:Body >
</soap:Envelope >

a) types, message, port type, binding
b) input, output, binding, exception
c) types, input, output, exception
d) input, output, operations, binding

**Answer:**

The answer is a.

WSDL
types : The data types used by the web service.
message : The input and output messages used by the web service.
port type : The operations performed by the web service. This is analogous to a class in Java programming. This also defines the input/output via messages. This is the
most important part of the wsdl.
binding : The communication protocol used by the web service.

---

## Q6: What are the dif ferent types of operations available in a WSDL?
a) One-way
b) Request-Response
c) Solicit-Response
d) Notification or fire and for get

**Answer:**

The answer is a, b, c, and d. Supports all 4 types of operations.
One-way : The operation (or endpoint) receives a message, but will not return a response.
Request-Response : The most common one. The operation (or endpoint) receives a request message and responds with a response message.
Solicit-Response : The operation (or endpoint) sends a request message and receives a correlated response message.
Notification or fir e and forget : The operation (or endpoint) sends a request message, but will not wait for a response.

---

## Q7: What is the dif ference between Request-Response and Solicit-Response?

**Answer:**

. Solicit-Response is a push operation like Notification, but waits for a response. Request-Response is a pull operation .
The only way to tell the dif ference between a request-response operation and a solicit-response operation is the ordering of the input and output elements. In request-
response, the input child element comes first. In solicit-response, the output child element comes first.

---

## Q8: Which of the following are true?
a) SOAP is a protocol and REST is a concept without any defined spec at all
b) SOAP is a XML-based message protocol, while REST is an architectural style
c) You can send SOAP envelopes in a REST application.
d) The REST verbs are “get”, “put”, “post” and “delete” and the nouns are identified by URLs.
e) SOAP allows many dif ferent verbs to be applied to many dif ferent nouns.

**Answer:**

The answer is a,b,c,d, and e. All are true.

a, b, and c are true because they state the fact that SOAP is an XML based message protocol and REST is a concept or architectural style.
d is true because RESTful url define the noun via the urls like
http://localhost:8080/accounting-services/1.0/forecasting/account/123/transaction/567
http://localhost:8080/accounting-services/1.0/forecasting/account/123/transactions/search?txn-date=20120201
http://localhost:8080/accounting-services/1.0/forecasting/account/123/transaction
e is true because you use dif ferent functions in SOAP port-type definition

---

## Q9: Though both RESTful web series and SOAP web service can operate cross platform, they are architecturally dif ferent to each other . Which of the following
statements are correct?
a) REST is more simple and easy to use than SOAP , hence currently more popular .
b) REST uses HTTP protocol for producing or consuming web services while SOAP uses XML.
c) REST is lightweight as compared to SOAP and preferred choice in mobile devices and PDA ’s.
d) REST supports dif ferent format like text, JSON and XML while SOAP only support XML.
e) REST web services call can be cached to improve performance.
f) SOAP provides more comprehensive security and transaction management.

**Answer:**

All are correct.

---

## Q10: Which of the following statements are correct?
a) JAX-WS is an API for SOAP based web service.
b) JAX-RS is an API for RESTFul web service.
c) SOAP invokes services by calling RPC method, REST just simply calls services via URL path.
d) Apache CXF framework only supports JAX-WS
e) Jersey and RESTEasy are reference implementations of JAX-RS.

**Answer:**

a, b, c, and e are correct. d is incorrect because Apache CXF supports both JAX-WS and JAX-RS.

---

## Q11: What is JAX-WS and why does it replace JAX-RPC?

**Answer:**

JAX-WS stands for Java Apifor Xml W eb Services, which contains a set of APIs for creating web services in XML format (SOAP). JAX-WS provides many
annotation to simplify the development and deployment for both web service consumers (i.e. clients) and web service providers (i.e. endpoints).
SOAP has been there for a while now , and its history goes like — firstly there was SOAP . But SOAP only described what the messages looked like. Then there was
WSDL. But WSDL didn’ t tell you how to write web services in Java™. Then along came JAX-RPC 1.0. Since the industry was using message-oriented web services,
“RPC” was removed from the name and replaced with “WS”, so JAX-RPC became JAX-WS. JAX-WS defines a standard Java-to-WSDL mapping, which determines
which Java method gets invoked and how that SOAP message is mapped to the method’ s parameters.

JAX-WS
JAX-WS uses a number of annotations like @W ebService and @W ebMethod.
mport javax .jws.WebMethod ;
mport javax .jws.WebService ;
@WebService
public interface Greeting {
 @WebMethod String sayHello (String name );
}
mport javax .jws.WebService ;
@WebService (endpointInterface = "Greeting" )

The WSDL can be found at http://localhost:8080/WS/Greeting?wsdl, and this is contract last approach.
To create the consumer , you can use wsimport on the client project folder
Which creates a few classes.public class GreetingImpl implements Greeting {
 @Override
 public String sayHello (String name ) {
 return "Hello: " + name ;
 }
mport javax .xml.ws.Endpoint ;
public class WSProvider {
 public static void main (String [] args) {
 Endpoint .publish ("http://localhost:8080/WS/Greeting" ,new GreetingImpl ());
 }
}
wsimport –s . http://localhost:8080/WS/Greeting?wsdl

Q. What is the dif ference between JAX-WS and JAX-RS?
A. JAX-WS represents SOAP and JAX-RS represents REST
SOAP Web Services interview Questions & Answers Links:
6 Java RESTful W eb services Interview | 5 JAXB interview Questions & Answers | Java W eb Services interview Questions & Answerspublic class WSConsumer {
 public static void main (String [] args){
 GreetingImplService service = new GreetingImplService ();
 Greeting greeting = service .getGreetingImplPort ();
 System .out.println (greeting .sayHello ("John" ));
 }
}

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
