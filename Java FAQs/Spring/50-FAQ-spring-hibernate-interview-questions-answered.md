# 50+ FAQ spring & hibernate interview questions answered

## Table of Contents

- [Q1: What do you understand by the terms Dependency Inversion Principle ( DIP), Depen](#q1)
- [Q2: In your experience, why would you use Spring framework?](#q2)
- [Q3: In your experience, what do you don’ t like about Spring? Are there any pitfalls](#q3)

---

## Q1: What do you understand by the terms Dependency Inversion Principle ( DIP), Dependency Injection ( DI) and Inversion of Control ( IoC) container?

**Answer:**

Dependency Inversion Principle ( DIP) is one of the 6 OO design principles abbreviated as “ SOLI D“, and is in some ways related to the Dependency
Injection (DI) pattern. The idea of DIP is that higher layers of your application should not directly depend on lower layers. Dependency Inversion Principle does
not imply Dependency Injection. This principle doesn’ t say anything about how higher layers know what lower layer to use. This could be done as shown below
by
1) Coding to interface using a factory pattern or
2) Coding to interface and through “Dependency Injection” by using an IoC container like Spring framework, Guice, or JEE 6+.
DIP (Dependency Inversion Principle)
The Dependency Inversion Principle (DIP) states that
– High level modules should not depend upon low level modules. Both should depend upon abstractions .

– Abstractions should not depend upon details. Details should depend upon abstractions.
When this principle is applied, the higher level classes will not be working directly with the lower level classes, but with an abstract layer (i.e. an abstract class
or an interface). This gives us the flexibility at the cost of increased ef fort. The “CircusService” depends on the interface “AnimalHandler” and not on the
concrete implementation “T igerhandler”. The implementation can be easily swapped to the “LionHandler” as long as it implements the interface
“AnimalHandler”.
#1. Tightly coupled “Cir cusService”
Higher layers directly depend on the implementations of the lower layers. This causes tighter coupling.
Helper implementation
Handler implementationpublic class TigerHelper {
 public void help() {
 System .out.println ("TigerHelper in action" );
 }
}
public class TigerHandler {
 private TigerHelper helper ;
 
 public TigerHandler (TigerHelper helper ) {
 this.helper = helper ;
 }

Service implementation
Output: public void handle () {
 System .out.println ("TigerHandler in action" );
 helper .help();
 } 
public class CircusService {
 
 private TigerHandler handler = null;
 
 public CircusService (TigerHandler handler ) {
 this.handler = handler ;
 }
 
 public void process () {
 handler .handle ();
 }
 public static void main (String [] args) {
 TigerHelper helper = new TigerHelper ();
 TigerHandler handler = new TigerHandler (helper );
 CircusService service = new CircusService (handler );
 service .process (); 
 }
TigerHandler in action

If you want “CircusService” to work with a TigerHandler and a LionHelper (instead of TigerHelper) as the business logic in “T igerHelper” is deprecated, you
need to
Change #1 Modify the “TigerHandler” class as it depends directly on the “TigerHelper” implementation .
Change “private TigerHelper helper;” TO: “private LionHelper helper;”
Change the constructor “public T igerHandler(T igerHelper helper) {” TO: “public T igerHandler(LionHelper helper) {”
Change #2 Modify the “CircusService” class as shown below .
Output:TigerHelper in action
public class CircusService {
 
 private TigerHandler handler = null;
 
 public CircusService (TigerHandler handler ) {
 this.handler = handler ;
 }
 
 public void process () {
 handler .handle ();
 }
 public static void main (String [] args) {
 LionHelper helper = new LionHelper (); //CHANGE
 TigerHandler handler = new TigerHandler (helper ); //CHANGE "T igerHandler .java"
 CircusService service = new CircusService (handler );
 service .process (); 
 }

As “TigerHandler” and “TigerHelper” are tightly coupled . If you apply the DIP (i.e. coding to interface) , you don’t have to make any changes to the
“TigerHandler” to use a “LionHelper”. In fact the “CircusService” is also tightly coupled to the “T igerHandler” as you need to make structural modifications to
get it to work with a “LionHandler”.
#2. Loosely coupled “Cir cusService”
by “coding to interface ” as shown below .TigerHandler in action
LionHelper in action

DIP – Dependency Inversion Principle
Define interfaces
public interface AnimalHelper {
 abstract void help();
}

Helper implementation coded to interface
Handler implementation coded to an interfacepublic interface AnimalHandler {
 abstract void handle ();
}
public class TigerHelper implements AnimalHelper {
 public void help() {
 System .out.println ("TigerHelper in action" );
 }
}
public class LionHelper implements AnimalHelper {
 public void help() {
 System .out.println ("LionHelper in action" );
 }
}

Service implementation that can swap helpers without having to modify the handlerpublic class TigerHandler implements AnimalHandler {
 private AnimalHelper helper ; //coded to an interface
 
 public TigerHandler (AnimalHelper helper ) { //coded to an interface
 this.helper = helper ;
 }
 
 public void handle () {
 System .out.println ("TigerHandler in action" );
 helper .help();
 } 
public class LionHandler implements AnimalHandler {
 private AnimalHelper helper ; //coded to an interface
 public LionHandler (AnimalHelper helper ) { //coded to an interface
 this.helper = helper ;
 }
 @Override
 public void handle () {
 System .out.println ("LionHandler in action" );
 helper .help();
 }

Now to get the “T igerHandler” to work with a “LionHelper”, you only have to change just one line :public class CircusService {
 
 private AnimalHandler handler = null; //coded to an interface
 
 public CircusService (AnimalHandler handler ) {
 this.handler = handler ;
 }
 
 public void process () {
 handler .handle ();
 }
 public static void main (String [] args) {
 AnimalHelper helper = new TigerHelper ();
 AnimalHandler handler = new TigerHandler (helper );
 CircusService service = new CircusService (handler );
 service .process (); 
 }
public class CircusService {
 
 private AnimalHandler handler = null; //coded to an interface
 
 public CircusService (AnimalHandler handler ) { //coded to an interface
 this.handler = handler ;
 }
 
 public void process () {
 handler .handle ();
 }
 public static void main (String [] args) {
 AnimalHelper helper = new LionHelper (); //ONL Y 1 CHANGE
 AnimalHandler handler = new TigerHandler (helper );
 CircusService service = new CircusService (handler );

#3. Singleton factory classes to wir e up dependencies
If you have multiple callee classes other than “CircusService” like “SancturyService”, “HuntingService”, “W ildlifeService”, etc then all the callee classes need
to be modified to get a “LionHelper” to work with a “T igerHandler”. This is tight coupling of callees with the AnimalHandler class. This can be improved with
the help of a “ singleton ” factory class as shown below .
Revised “CircusService” class using the factory class service .process (); 
 }
public final class AnimalHandlerFactory {
 private static AnimalHandler instance = null; //coded to an interface
 private AnimalHandlerFactory () {};
 public static synchronized AnimalHandler buildHandler () {
 if (instance == null) {
 AnimalHelper helper = new TigerHelper ();
 instance = new TigerHandler (helper ); // or T igerHelper
 }
 return instance ;
 }

Now , if any dependencies change like “TigerHandler” depending on a “LionHelper”, etc, change it only in the factory , and not in all the callees like
“CircusService”, “SancturyService”, “HuntingService”, “W ildlifeService”, etc.
What is DI (aka Dependency Injection)?
Dependency Injection (DI) is all about injecting a class’ s dependencies into it at runtime. This is based on the “Dependency Inversion Principle” by defining
the dependencies as interfaces, and then injecting in a concrete class implementing that interface to the constructor (or via setter methods). This allows you to
swap dif ferent implementations without having to make any structural changes.
The “Dependency Injection” pattern also promotes high cohesion via the Single Responsibility Principle ( SRP ) since your dependencies are individual objects,
which perform discrete specialized tasks like “handling animals”, “helping animal handlers”, etc. I.e. clear separation of responsibilities.
How does a “DI pattern” differ fr om a “Factory pattern”?
Dependency Injection is more of an architectural pattern for loosely coupling software components. Factory pattern is one of the ways to implement
“Dependency Injection”.
A typical commercial project will have 300+ classes, and you will need to create too many singleton factory classes . When you use IoC containers like Spring
IoC, Guice, JEE6+ CDI (Context & Dependency Injection), etc you can wire up the whole application dependencies with a lot fewer configuration files in Java
& XML.public class CircusService {
 
 private AnimalHandler handler = null; //coded to an interface
 
 public CircusService (AnimalHandler handler ) { //coded to an interface
 this.handler = handler ;
 }
 
 public void process () {
 handler .handle ();
 }
 public static void main (String [] args) {
 AnimalHandler handler = AnimalHandlerFactory .buildHandler ();
 CircusService service = new CircusService (handler );
 service .process (); 
 }

So, DI is in many ways like a configurable Factory Pattern .
#4. Spring as the IoC container to wir e up classes via DI
Spring Ioc inverts the “flow of control” by taking the task of wiring up the dependencies at runtime.
Step 1: Use Spring framework. The pom.xml file will bring in the Spring dependency jar files.
Step 2: Create an xml “ applicationContext.xml “, which wires up the dependencies. This is the replacement for the factory class “AnimalHandlerFactory”.<dependency >
 <groupId >org.springframework </groupId >
 <artifactId >spring -context </artifactId >
 <version >4.3.4.RELEASE </version >
</dependency >
<beans xmlns ="http://www .springframework.or g/schema/beans"
 xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://www .springframework.or g/schema/beans
 http://www .springframework.or g/schema/beans/spring-beans-3.0.xsd" >
 <bean id="helper" class ="LionHelper" />
 <bean id="handler" class ="TigerHandler" >
 <!-- constructor injection of helper into handler -->
 <constructor -arg ref="helper" />
 </bean >
</beans >

The helper is injected into handler via constructor injection in Spring.
Step 3: The revised “CircusService” class using the Spring IoC container for DI instead of the factory class “AnimalHandlerFactory”.
Q. Why DI frameworks ar e better than factory classes?
Flexibility & easier maintenance as you don’ t have to create too many factory classes, and also when using a DI framework, you are outsourcing the
responsibility of wiring up the classes to an external framework like Spring, which is separate from your code.mport org.springframework .context .ApplicationContext ;
mport org.springframework .context .support .ClassPathXmlApplicationContext ;
public class CircusService {
 
 private AnimalHandler handler = null;
 
 public CircusService (AnimalHandler handler ) {
 this.handler = handler ;
 }
 
 public void process () {
 handler .handle ();
 }
 public static void main (String [] args) {
 @SuppressW arnings ("resource" )
 ApplicationContext context = new ClassPathXmlApplicationContext ("applicationContext.xml" );
 AnimalHandler handler = (AnimalHandler ) context .getBean ("handler" );
 
 CircusService service = new CircusService (handler );
 service .process ();
 }

The DI Frameworks give you flexibility of how to register your abstractions against your concrete types with Java Config, XML and Annotations.
Q. What is an IoC container?
The Inversion of Contr ol Container (IoC) is a container that supports Dependency Injection. An IoC container defines what concrete classes should be used
for what dependencies throughout your application. This brings in an added flexibility through looser coupling, and makes it much easier to change what
dependencies are used.
The basic concept of the Inversion of Control pattern is that you do not create your objects but describe how they should be created. Y ou don’ t directly connect
your components and services together in the code, but describe which services are needed by which components via configuration files in XML/Java and
annotations. The IoC container is then responsible for hooking it all up. The objects are given their dependencies at creation time by some external entity that
coordinates each object in the system.
Q. What ar e the benefits of IoC containers like Spring or JEE CDI?
The real power of DI and IoC is realized in its ability to replace the compile time binding of the relationships between classes with binding those relationships
at runtime . For example, in Seam framework, you can have a real and mock implementation of an interface, and at runtime decide which one to use based on a
property , presence of another file, or some precedence values. This is incredibly useful if you think you may need to modify the way your application behaves in
different scenarios/environments.
Another real benefit of DI and IoC is that it makes your code easier to unit test. Y ou can inject mock implementations into your unit test classes. Junit with
Mockito tutorial to fully mock the DAO layer
There are other benefits like promoting looser coupling without any proliferation of factory and singleton design patterns, follows a consistent approach for
lesser experienced developers to follow , etc. These benefits can come in at the cost of the added complexity to your application and has to be carefully manged
by using them only at the right places where the real benefits are realized, and not just using them because many others are using them.
Note : The CDI (Contexts and Dependency Injection) is standard on Dependency Injection. CDI is a part of the Java EE 6 stack, meaning an application running
in a Java EE 6 compatible container can leverage CDI out-of-the-box. W eld is the reference implementation of CDI.

---

## Q2: In your experience, why would you use Spring framework?

**Answer:**

Spring framework has been very popular and been filling the gap in the Java EE stack for a number of years now . Java EE has finally caught up with DI,
AOP , batch jobs, Managed Beans, etc promoting POJO driven development with JEE 6 and 7, hence for new Java EE projects JEE 6 or 7 might be the way to
go. If you are after enhancing the current projects developed in Spring, or you are in more favor of Spring then this framewok has been popular for a number of
reasons compared to pre JEE 6.
1) A key design principle in Spring in general is the “ Open for extension, closed for modification ” principle. So, some of the methods in the core classes are
marked “final”.

2) Spring is an IoC container that supports both constructor injection (supplying ar guments to constructors) and setter -based injection (calling setters on a call)
to get the benefits of Dependency Injection (DI) like loose coupling, easier to test, etc.
3) It is modular with spring core, spring batch, spring mvc, spring orm, etc. When Spring was out, it was only a small core with IoC container and it was fast
and easy to use. Now , I cannot even count how many Spring Components are available today . Spring Boot is a new framework to address this complexity by
simplifying the bootstrapping and development of a new Spring application.
4) It is very matured and has been used in many lar ge Java/JEE applications.
5) It supports AOP to implement cross cutting concerns.
6) It supports transaction management.
7) It promotes uniformity to handle exceptions (e.g. checked vs unchecked) and integration with JMS, JDBC, JNDI, remoting, etc.
8) Closes the JDBC/JMS connection resources automatically without having to you write verbose try/catch/finally blocks with logic to close the resources.

---

## Q3: In your experience, what do you don’ t like about Spring? Are there any pitfalls?

**Answer:**

1) Spring has become very huge and bulky . So, don’ t over do it by using all its features because of the hype that Spring is good. Look at what parts of Spring
really provides some benefits for your project and use those parts. In most cases, it is much better to use proven frameworks like “Spring Boot” than create your
own equivalent solution from a maintenance and applying the best practices perspective.
2) Spring MVC is probably not the best W eb framework. There are JavaScript based alternatives like angular js, react js, etc. Y ou will be using Spring with
“Spring Boot” for building the micr oservices . Evaluate & benchmark other alternative frameworks like Dropwizard, V ertx, etc.
Top 20+ Spring Interview Questions & Answers:
6 FAQ Spring interview questions and answers | 17 Spring F AQ interview Questions & Answers
Top 30+ Hibernate Interview Questions & Answers:
30+ F AQ Hibernate interview questions & answers

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
