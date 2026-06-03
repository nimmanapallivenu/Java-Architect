# 4 10 JEE overview interview Q&As   Java Success.com

## Table of Contents

- [Q1: When a company requires Java EE experience, what are they really asking for?](#q1)
- [Q2: Full stack developers are in more demand. What does the term “ full stack Java d](#q2)
- [Q3: What is Java EE? What are JEE technologies and services?](#q3)
- [Q4: Can you give some examples of improvements in the later Java EE specification in](#q4)
- [Q5: Can you describe the architecture and technologies used in one of the Java EE pr](#q5)
- [Q6: What are the advantages of a 3-tiered or n-tiered application?](#q6)
- [Q7: Explain MVC architecture relating to JEE?](#q7)
- [Q8: What is the dif ference between a W eb server and an application server?](#q8)
- [Q9: What are ear , war and jar files? What are JEE Deployment Descriptors?](#q9)
- [Q10: Can you explain the JEE class loader hierachy?](#q10)

---

## Q1: When a company requires Java EE experience, what are they really asking for?

**Answer:**

Java EE (i.e. Enterprise Edition) is a collection of specifications for developing and deploying enterprise applications. In general, JEE applications refer to
applications written in Java & packaged as executable uber jar , war or ear files and hosted within application servers like T omcat, JBoss, IBM W ebspehere, etc.
Some of the JEE core technologies & APIs are Java Servlet, JSP , JSF , JPA, JDBC, JNDI, EJB, RMI,, JMS, JTS, JT A, JavaMail, and JAF .

---

## Q2: Full stack developers are in more demand. What does the term “ full stack Java developer ” mean?

**Answer:**

Have you seen job advertisements like….
“engineering team is looking for a Full Stack Java Developer ……”
Don’ t be too overwhelmed by companies looking for the following skill sets in a full-stack developer . This site covers many of these topics to increase your
awareness and skills as a full stack Java developer:
Java, JEE, Spring, Hibernate, Maven, Relational databases, NoSQL databases, SQL, JavaScript, HTML, CSS, JSON, XML, Unix, UML ,
ERD for data modelling, AngularJS, etc.
Some are mandatory , whilst the others are optional and you need to be flexible enough to learn those skills. Full stack developer is a very loose term with
experience in 2 or more of the categories listed below .
1) Client side UI technologies such as HTML, CSS, JavaScript, and jQuery . Expand these skills to learn a MVW (i.e. Model View Whatever) framework like
AngularJS, ReactJS, etc.
2) Web service development skills using Java, JEE, Spring, Hibernate, RDBMS databases like MySQL server , SQL, and JSON.
3) Integration skills utilizing web services (i.e. RESTful & SOAP), JMS (E.g. W ebspeher MQ, ActiveMQ, etc) with topics & queues, ESB (Apache Camel,
Mule, Spring Integration), FIX Protocol, etc.
4) ETL (i.e. Extract Transform & Load) and EL T (i.e. Extract Load & Transform) skills like Spring batch, Apache Spark, Sqoop, Flume, SQL, NoSQL, etc.
5) Infrastructure skills like Unix, A WS, Cloud computing, Systems monitoring, etc.
The high-level architecture diagram in Q5 shows that user interfaces are written in JavaScript based technologies/frameworks, and web services were written in
Java, Spring, Hibernate based technologies/frameworks. Developing this application requires a full stack web developer . The back-end systems can be
integrated with the other systems via messaging, RESTful web services, and ETL as explained in 7+ Java integration styles & patterns interview questions &
answers .
Transition fr om:

Core Java Developer => Full stack web apps & web services Developer => Full stack Java developer

---

## Q3: What is Java EE? What are JEE technologies and services?

**Answer:**

Java platform Enterprise Edition or Java EE is a Java computing platform providing APIs and run time services.

JEE big picture
Web server , application server , web container & EJB container basics
When dealing with W eb based requests, the requests are tunnelled through web or HTTP servers to application servers . A client will always hit a W eb
server first. A W eb server takes requests from clients, maps that request to a file on the file system, and then sends that file back to the client. W eb servers serve
only static resources like HTML and CSS.
If you need some logic or dynamic content in your applications, you will delegate to Servlets and JSPs that are running on an application server . Applications
servers will have a W eb container to host Servlets, JSPs and Java Beans. For example, Apache T omcat or Jetty . A web container just implements the JSP and
Servlet specification of JEE.
Additionally , some application servers like W ebsphere Application Server by IBM, JBoss Application Server by Red Hat, etc have an EJB container to manage
business logic, persistence, transaction management and security in a standard way . With the advent of lightweight Java frameworks like Spring and Hibernate,
you really don’ t need the extra overhead with the EJBs. Y ou can get most of the functionality that EJBs provide through Spring, Hibernate, and JavaBeans with
less overhead and less learning curve.
When a client makes a request for a JSP or a Servlet, the request initially goes to the W eb server . The W eb server reads the special XML file the application
server provides, and realizes that the request that came in should be sent to the application server for processing. The W eb server handles the incoming request,
and matches that request to the application server set up to handle the given Servlet or JSP .
A Servlet can do pretty much anything the developer wants it to do. T ypically , a Servlet implements some control logic. For example, a Servlet might figure out
what a user typed into some text fields in a web-based form. It might then take that information and save it to a database. Whilst a Servlet can interact directly
with a database, it is not a best practice. Instead, Servlets are supposed to delegate to a JavaBean (or an EJB).
1) JEE APIs like
— javax.servlet.* for Servlet & JSP to service HTTP protocol.
— javax.faces.* for Java Server Faces ( JSF) to create user interfaces.

— javax.el (Expression Language) used for binding JSF components.
— javax.enterprise.inject.* for defining injection annotations for the Contexts and Dependency Injects ( CDI).
— javax.enterprise.context for CDI
— javax.ejb.* the Enterprise Java Beans (i.e. EJBs)
— javax.persistence.* for Java Persistence API (i.e. JPA)
— javax.transaction.* for Java T ransaction API ( JTA)
— javax.jms.* for Java Message Service (i.e. JMS ) to send and receive messages from Message Oriented Middlewares.
javax.resource.* for Java EE Connector Architecture ( JCA ) to integrate with EIS(Enterprise Integration Systems) systems.
2) runtime envir onment
for developing and running enterprise software packages like ear (Enterprise ARchive), war (W eb ARchive), or jar (Java ARchive) deployed to an JEE
application server like JBoss server having web and EJB containers. The modular components running in an application server make use of annotations and
conventions to configure or wire up all the components and modules. Optionally , XML based deployment descriptor files can be used to override annotations.

JEE Big Picture
Physically it is a 3 tier system, but logically and functionally , JEE is a multi-tiered or n-tier system.

JEE n-tier system
At the time of writing, Java EE 7 was out focusing on HTML 5 with W ebSocket and JSON. W ebSocket provides for two-way communication with a remote
host, and W eb applications can maintain bidirectional communications with server -side processes. JSON, or JavaScript Object Notation, is a lightweight data
interchange format based on JavaScript and featuring a language-independent text format, which is less verbose than XML. The goal is to have a standard way
to accomplish things.
JEE 7 T echnology Stack

---

## Q4: Can you give some examples of improvements in the later Java EE specification in comparison to the older JEE specs?

**Answer:**

The new specification is based on favoring “convention over configuration” for ease of development, and introduces annotations to replace the use of
verbose XML for configuration.
— CDI pr ovides unification of DI technologies (e.g. Guice, Spring, Seam, EJB3, and CDI itself) and introduces lifecycle to components to simplify
development of new features. Y ou can combine Seams lifecycle mappings with Spring MVC’ s and with JP A’s.

— Managed Beans was intr oduced to addr ess interaction issues among the pr esentation layers(Servlets, JSP and JSF) and persistance layers(EJB 3.0,
JTA, JCA and JP A). There are many form of beans in JEE like JSF Bean, EJB Bean, Spring Bean, Seam Bean, Guice CDI, Entity Beans, etc, but ultimately all
are POJOs defined by the “Managed Bean 1.0” spec, and the container does provide the basic services like Lifecycle Management (@PostConstruct,
@PreDestory), Injection of a r esour ce (@Resource), and Inter ception with inter ceptors (@Interceptors, @ArounInvoke). Manage Bean is an SPI that was
extended for this specific use.
— Introduction of Profiles and Pruning allow technical architects & developers to control the APIs & Specs needed for a application. Java/JEE is a huge
platform and imports a lot of APIs to projects. Profiles is an introductory concept to limit the number of APIs needed for a specific application. There are pre
defined profiles like W eb Profile. Pruning allows removal of existing APIs or Frameworks being used by an application. JEE 6 onwards has the plug-ability to
extend your Java EE with other frameworks via Extensibility points and Service Provider Interfaces (SPI).
— From Servlets 3.0 it is not required to change the descriptor file web.xml for adding the third party libraries like Struts, Spring, JSF , etc. Servlets 3.0
introdues W eb Fragments descriptor to specify the details of each library used by the container .
— Dependency Injection simplifies EJBs.
— The persistence layer was replaced with JP A.
— Batch processing is introduced for bulk processing
— HTML 5 is embraced in JEE 7 with W eb Sockets and JSON
— JEE Concurrency utilities extending the Java SE Executor framework for asynchronous processing.
— Standardized validation framework across W eb, EJB, JCA, etc
Spring framework had been filling some of the gaps in Java EE by focusing on POJO centric development, and has dominated the mainstream development in
Java, and now JEE has embraced POJO centric development with lots of enhancements and improvements in a standard way . So, be prepared for interview
questions like what is your take on Spring Vs JEE 6 or JEE 7 for future development? There are no right or wrong answers, but ask questions like
Is backward compatibility required? Does our application server support the new version of Java EE, Can I achieve everything in Java EE version 6 that can be
achieved in Spring?, etc. Spring Vs. JEE will be an ongoing hot topic for a while.

---

## Q5: Can you describe the architecture and technologies used in one of the Java EE projects you were recently involved in?

**Answer:**

This is a very popular question, and your answer will lead to other drill down questions.

Java EE high level architecture with tiers and layers

Step 1 : When the url is typed like https://myhost:8080/myapp the relevant dynamic HTML files with angularjs code and all relevant angular .js files, bootsrtap
CSS files, and images are downloaded into the client browser .
Step 2 : The angularjs based user interface will be a Rich Internet Application (aka RIA or Single Page Application) with tabs and rich GUI will be making ajax
requests to the https://myhost:8080/my-ws to get data in JSON (JavaScript Object Notation).
The “W eb Service” layer is HTTP protocol dependent, and the “Business” and “DAO” layers make us of POJOs (Plain Old Java Objects). Hence, protocol
independent. This where you need to put the business logic and data access logic.
Two web archive (i.e. war) files are deployed to the same web container . One for the user interface and the other one is to retrieve data via RESTful web
services. The ajax requests will only render a section of the single page. The presentation war file with HTML, CSS, and JavaScript can be hosted via CDN (i.e.
Content Delivery Networks), which is a lar ge distributed system of servers deployed in multiple data centers across the Internet. The goal of a CDN is to serve
content to end-users with high availability and high performance.
Note : Like me, many have a love/hate relationship with JavaScript. Now a days, JavaScript is very popular with the rich internet applications (RIA). So, it really
pays to know JavaScript. JavaScript is a client side (and server side with node.js) technology that is used to dynamically manipulate the DOM tree. There are a
number of JavaScript based frameworks like jQuery available to make your code more cross browser compatible and simpler .What is even more popular is the
JavaScript based MVW (Model V iew Whatever) frameworks like angularjs , backbone.js , ember .js, etc to build GUI.

---

## Q6: What are the advantages of a 3-tiered or n-tiered application?

**Answer:**

3-tier or multi-tier architectures force separation among presentation logic, business logic and database logic. Let us look at some of the key benefits:
Manageability : Each tier can be monitored, tuned and upgraded independently and dif ferent people can have clearly defined responsibilities.
Scalability : More hardware can be added and allows clustering (i.e. horizontal scaling).
Maintainability : Changes and upgrades can be performed without af fecting other components.
Availability : Clustering and load balancing can provide availability .
Extensibility : Additional features can be easily added.

---

## Q7: Explain MVC architecture relating to JEE?

**Answer:**

This is a very popular interview question. MVC stands for Model-V iew-Controller architecture. It divides the functionality of displaying and maintaining of
data to minimize the degree of coupling (i.e. promotes loose coupling) between components. It is often used by applications that need the ability to maintain
multiple views like HTML, WML, XML based W eb service, etc of the same data. Multiple views and controllers can interface with the same model. Even new
types of views and controllers can interface with a model without forcing a change in the model design.

Java EE MVC pattern
The JavaScript based MVC frameworks like angularjs uses MVW ( Model-V iew-Whatever . Where Whatever stands for “whatever works for you” — MVC –
Model-V iew-Controller , MVP – Model-V iew-Presenter , MVVM – Model-V iew-V iewModel, etc ).

---

## Q8: What is the dif ference between a W eb server and an application server?

**Answer:**

Web Server Application Server
Supports HTTP protocol. When the W eb server receives an
HTTP request, it responds with an HTTP response, such as
sending back an HTML page (static content) or delegates the
dynamic response generation to some other program such as
CGI scripts or Servlets or JSPs in the application server .Application Server
Exposes business logic and dynamic content to the client through various protocols such as
HTTP , TCP/IP , IIOP , JRMP etc.
Uses various scalability and fault-tolerance techniques.ses various scalability and fault-tolerance techniques. In addition provides resource pooling,
component life cycle management, transaction management, messaging, security etc.
Provides services for components like W eb container for servlet components and EJB container
for EJB components.

---

## Q9: What are ear , war and jar files? What are JEE Deployment Descriptors?

**Answer:**

The ear , war and jar are standard application deployment archive files. Since they are a standard, any application server (at least in theory) will know how to
unpack and deploy them.
An EAR file is a standard JAR file with an “.ear” extension, named from Enterprise ARchive file. A JEE application with all of its modules is delivered in EAR
file. JAR files can’ t have other JAR files. But EAR and W AR (W eb ARchive) files can have JAR files.
An EAR file contains all the JARs and W ARs belonging to an application. JAR files contain the EJB classes and W AR files contain the W eb components (JSPs,
Servlets and static content like HTML, CSS, GIF etc). The JEE application client’ s class files are also stored in a JAR file. EARs, JARs, and W ARs all contain
one or more XML-based deployment descriptor(s).

JEE ear , war , and jar
In EJB 3.1 the EJB module can be packaged within the web module in two dif ferent ways.
1) Package an ejb module into a jar , and place it under WEB-INF/lib folder .
2) Compile the ejb classes into WEB-INF/classes and put the deployment descriptor in MET A-INF folder in the web module.

A deployment descriptor is an XML based text file with an “.xml” extension that describes a component’ s deployment settings. A JEE application and each of
its modules has its own deployment descriptor .
application.xml : is a standard JEE deployment descriptor for ear files, and includes structural information: EJB jar modules, W eb war modules, etc.
ejb-jar .xml: is a standard deployment descriptor for an EJB module.
web.xml : is a standard deployment descriptor for a W eb module.

---

## Q10: Can you explain the JEE class loader hierachy?

**Answer:**

JEE application server sample class loader hierarchy is shown below . As per the diagram the JEE application specific class loaders are children of the
“System –classpath” class loader . When the parent class loader is above the “System –classpath” class loader in the hierarchy as shown in the diagram (i.e.
bootstrap class loader or extensions class loader) then child class loaders implicitly have visibility to the classes loaded by its parents. When a parent class
loader is below a “System -classpath” class loader in the hierarchy then the child class loaders will only have visibility into the classes loaded by its parents only
if they are explicitly specified in a manifest file (MANIFEST .MF) of the child class loader .
JEE sample class loader hierarchy . Actual vendor server class loaders can vary .
Example As per the diagram, if the EJB module MyAppsEJB.jar wants to refer to MyAppsCommon.jar and MyAppsUtil.jar we need to add the following entry
in the MyAppsEJB.jar ’s manifest file MANIFEST .MF.

class-path: MyAppsCommon.jar MyAppsUtil.jar
This is because the application (EAR) class loader loads the MyAppsCommon.jar and MyAppsUtil.jar . The EJB class loader loads the MyAppsEJB.jar , which is
the child class loader of the application class loader . The W AR class loader loads the MyAppsW eb.war .
Every JEE application or EAR gets its own instance of the application class loader . This class loader is also responsible for loading all the dependency jar files,
which are shared by both W eb and EJB modules. For example third party libraries like log4j, utility (e.g. MyAppsUtility .jar) and common (e.g.
MyAppsCommon.jar) jars etc. Any application specific exception like MyApplicationException thrown by an EJB module should be caught by a W eb module.
So the exception class MyApplicationException is shared by both W eb and EJB modules.
The key dif ference between the EJB and W AR class loader is that all the EJB jars in the application share the same EJB class loader whereas W AR files get their
own class loader . This is because the EJBs have inherent relationship between one another (i.e. EJB-EJB communication between EJBs in dif ferent applications
but hosted on the same JVM) but the W eb modules do not. Every W AR file should be able to have its own WEB-INF/lib third party libraries and need to be able
to load its own version of converted logon.jsp servlet. So each W eb module is isolated in its own class loader .
So if two dif ferent W eb modules want to use two dif ferent versions of the same EJB then we need to have two dif ferent ear files. Class loaders use a delegation
model where the child class loaders delegate the loading up the hierarchy to their parent before trying to load it itself only if the parent can’ t load it. But with
regards to W AR class loaders, some application servers provide a setting to turn this behavior of f (DelegationMode=false).
As a general rule classes should not be deployed higher in the hierarchy than they are supposed to exist. This is because if you move one class up the hierarchy
then you will have to move other classes up the hierarchy as well. This is because classes loaded by the parent class loader can’ t see the classes loaded by its
child class loaders (uni-directional bottom-up visibility).
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
