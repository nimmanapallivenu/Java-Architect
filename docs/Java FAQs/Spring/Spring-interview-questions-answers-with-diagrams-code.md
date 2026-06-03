# Spring interview questions & answers with diagrams & code

## Table of Contents

- [Q1: What do you understand by the terms Dependency Inversion Principle ( DIP ), Depe](#q1)
- [Q3: Dependency Injection is a design pattern that allows us to write loosely coupled](#q3)
- [Q4: What are the dif ferent types of dependency injections?](#q4)
- [Q5: Which ones are the most commonly used DIs?](#q5)
- [Q6: When will you favor DI type “Constructor Injection” over “Setter Injection”?](#q6)
- [Q7: When will you favor DI type “Setter Injection” over “Constructor Injection”?](#q7)

---

## Q1: What do you understand by the terms Dependency Inversion Principle ( DIP ), Dependency Injection ( DI) and Inversion of Control ( IoC) container?

**Answer:**

The dif ferences are very subtle and can be hard to understand. Hence, explained via code samples.
DIP, DI & IoC
1) Dependency Inversion Principle ( DIP): is one of the 5 OO design principles abbreviated as “SOLI D”, and “D” stands for DIP meaning that we
should always only rely on interfaces and not on their implementations . The idea of DIP is that higher layers of your application should not directly depend on

lower layers. DIP is the principle that guides us towards DI pattern. Y ou will see in the example below that the higher layer module “MyServiceImpl” depends
on the lower layer module interface “ Processor ” and NOT on the implementations “XmlProcessor” & “JsonProcessor”. This is commented on the code shown
in Q3 for “ MyServiceImpl ” as “ // code to interface ” for the understanding.
DIP – Dependency Inversion Principle
2) Dependency Injection ( DI): is a design pattern where instead of having your objects create a dependency or asking a factory object to make one for
you, you pass the needed dependencies into the constructor (i.e. Constructor Injection ) or via setter methods (i.e. Setter Injection ) from outside the class .
This is achieved by defining the dependencies as interfaces, and then injecting in a concrete class implementing that interface via a constructor (i.e. constructor
injection) or a setter method (i.e. setter injection) by wiring up via an IoC container like Spring. Y ou can wire up the dependencies from outside using an XML
config file as shown below:
<?xml version ="1.0" encoding ="UTF-8" ?>

or using annotations (i.e. preferred) as demonstrated with code in

---

## Q3: Dependency Injection is a design pattern that allows us to write loosely coupled code
for better maintainability .
Q. Wondering, what loose coupling is? how to write loosely coupled code?
A. Top 6 tips to go about writing loosely coupled Java applications .
3) Inversion of Control ( IoC): is a software design principle where the framework controls the program flow . Spring framework, Guice, etc are
IoC containers that implement the IoC principle. An IoC container like Spring is responsibly for loosely wiring up the dependencies. When Spring application
runs, it looks at the either XML config file or the annotations to wire up the dependencies. For example, in the example shown below Spring creates instances of
XmlPr ocessor & JsonPr ocessor and inject them via a constructor into “ MyServiceImpl “.
Q2. What are you “Inverting” in IoC?
A2. Flow of contr ol is “inverted ” by dependency injection because you are ef fectively delegating dependencies to some external system (e.g. IoC container
like Spring or Service Locator).
Still not too clear? Try Spring DIP , DI & IoC in detail with diagrams & a step by step video tutorial : Spring DIP , DI, and IoC .
Q3. What are the dif ferent implementation patterns of IoC principle?

**Answer:**

The two implementation patterns of the IoC design principles are
1. Dependency Injection ( DI) pattern: A class is given it’ s dependencies from outside like Spring IoC or JEE 7+ container . It neither knows, nor cares
where the dependencies are coming from.
2. Service Locator ( SL) pattern: has the same goal as DI where a service locator class is responsible for creating its dependencies. E.g. JNDI or other Map
based registries to map dependencies. In a map based registry you can lookup services based on keys. Data sources, JMS connection factories and
destinations (e.g. Queue or T opic) can be looked up via JNDI.<beans xmlns ="http://www .springframework.or g/schema/beans" xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://www .springframework.or g/schema/beans http://www .springframework.or g/schema/beans/spring-beans-3.2.xsd" >
 
 <bean id="xmlProcessor" class ="com.mytutorial.XmlProcessor" />
 <bean id="jsonProcessor" class ="com.mytutorial.JsonProcessor" />
 
 <!--constructor injection -->
 <bean id="myServiceImpl" class ="com.mytutorial.MyServiceImpl" >
 <constructor -arg ref="xmlProcessor" />
 <constructor -arg ref="jsonProcessor" />
 </bean >
 
</beans >

Here is a DI example with Spring IoC container . We inject XML & JSON processors into “MyServiceImpl”.
Interface Pr ocessor
Spring Configuration AppConfig
Processor implementations “JsonPr ocessor” and “XmlPr ocessor”package com.mytutorial ;
public interface Processor {
 <T> T process ();
}
package com.mytutorial ;
mport org.springframework .context .annotation .Bean ;
mport org.springframework .context .annotation .ComponentScan ;
mport org.springframework .context .annotation .Configuration ;
@Configuration
@ComponentScan ("com.mytutorial" )
public class AppConfig {
 @Bean
 MyServiceImpl myServiceImpl () {
 return new MyServiceImpl ();
 }

Service class MyServiceImplpackage com.mytutorial ;
mport org.springframework .stereotype .Component ;
@Component ("JsonProcessor" )
class JsonProcessor implements Processor {
 public <T> T process () {
 //...T ODO:
 System .out.println ("jsonProcessor .....");
 return null;
 }
package com.mytutorial ;
mport org.springframework .stereotype .Component ;
@Component ("XmlProcessor" )
class XmlProcessor implements Processor {
 public <T> T process () {
 //... T ODO:
 System .out.println ("xmlProcessor .....");
 return null;
 }

Standalone App to executepackage com.mytutorial ;
mport org.springframework .beans .factory .annotation .Autowired ;
mport org.springframework .beans .factory .annotation .Qualifier ;
class MyServiceImpl {
 @Autowired
 @Qualifier ("XmlProcessor" )
 Processor xmlProcessor ; // code to interface - DON"T DO "XmlProcessor xmlProcessor" that violates DIP
 @Autowired
 @Qualifier ("JsonProcessor" )
 Processor jsonProcessor ; //code to interface - DON"T DO "JsonProcessor jsonProcessor" that violates DIP
 public void processXml () {
 xmlProcessor .process ();
 // ....
 }
 public void processJson () {
 jsonProcessor .process ();
 // ....
 }
package com.mytutorial ;
mport org.springframework .context .annotation .AnnotationConfigApplicationContext ;

Output:
Service Locator (SL) type IoC example:
Not a common IoC pattern. V ery rarely used. Same code as above can be modified to use a Service Locator .
Service Locator “Pr ocessorServiceLocatorFactory”public class App
{
 public static void main ( String [] args )
 {
 AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext ();
 ctx.register (AppConfig .class );
 ctx.refresh ();
 MyServiceImpl bean = ctx.getBean (MyServiceImpl .class );
 bean .processXml ();
 bean .processJson ();
 ctx.close ();
 }
xmlProcessor .....
sonProcessor ....
package com.mytutorial ;
mport org.springframework .stereotype .Service ;

Modified “MyServiceImpl” to use the Service Locator@Service
public class ProcessorServiceLocatorFactory {
 
 public Processor getProcessor (String processorName ) {
 //lookup dynamically via JNDI or other Map based registries to map dependencies
 if("XmlProcessor" .equalsIgnoreCase (processorName )){
 return new XmlProcessor ();
 } else {
 return new JsonProcessor ();
 }
 }
package com.mytutorial ;
mport org.springframework .beans .factory .annotation .Autowired ;
class MyServiceImpl {
 @Autowired
 ProcessorServiceLocatorFactory locatorService ;
 public void processXml () {
 Processor processor = locatorService .getProcessor ("XmlProcessor" );
 processor .process ();
 // ....
 }
 public void processJson () {
 Processor processor = locatorService .getProcessor ("JsonProcessor" );
 processor .process ();
 // ....
 }

The core of the Spring Framework is its Inversion of Control (Ioc) container . The Spring IoC container manages Java objects from their instantiation to
destruction via its BeanFactory . Java components that are instantiated by the IoC container are called beans, and the IoC container manages a bean’ s scope (e.g.
prototype vs singleton ), lifecycle events (e.g. initialization, method callbacks & shutdown), and any AOP (Aspect Oriented Programming) features if
configured.
The key focus of both types of IoC is to loosely couple dependencies among components like MyApp, MyServiceImpl, and Procesor as per the above examples.

---

## Q4: What are the dif ferent types of dependency injections?

**Answer:**

There are 4 types of dependency injection. Spring supports 3 types. 1, 2 & 4 shown below .
1) Constructor Injection (e.g. Spring): Dependencies are provided as constructor parameters .
package com.mytutorial ;
mport org.springframework .beans .factory .annotation .Autowired ;
mport org.springframework .beans .factory .annotation .Qualifier ;
class MyServiceImpl {
 private final Processor xmlProcessor ;
 private final Processor jsonProcessor ;
 @Autowired
 public MyServiceImpl (@Qualifier ("XmlProcessor" ) Processor xmlProcessor ,
 @Qualifier ("JsonProcessor" ) Processor jsonProcessor ) {
 super ();
 this.xmlProcessor = xmlProcessor ;
 this.jsonProcessor = jsonProcessor ;
 }
 public void processXml () {
 xmlProcessor .process ();
 // ....

2) Setter Injection (e.g. Spring): Dependencies are assigned through setter methods . }
 public void processJson () {
 jsonProcessor .process ();
 // ....
 }
package com.mytutorial ;
mport org.springframework .beans .factory .annotation .Autowired ;
mport org.springframework .beans .factory .annotation .Qualifier ;
class MyServiceImpl {
 private Processor xmlProcessor ;
 private Processor jsonProcessor ;
 public void processXml () {
 xmlProcessor .process ();
 // ....
 }
 public void processJson () {
 jsonProcessor .process ();
 // ....
 }
 @Autowired
 @Qualifier ("XmlProcessor" )
 public void setXmlProcessor (Processor xmlProcessor ) {
 this.xmlProcessor = xmlProcessor ;
 }

3) Interface Injection (e.g. A valon): Injection is done through an interface.
4) Field injection : Using annotations on fields and parameters.
Spring supports 1) Constructor Injection, 2) Setter Injection & 4) Field injection with annotations.

---

## Q5: Which ones are the most commonly used DIs?

**Answer:**

1) Constructor Injection, 2) Setter Injection & 4) Field injection with annotations.

---

## Q6: When will you favor DI type “Constructor Injection” over “Setter Injection”?

**Answer:**

Using constructor injection allows you to hide immutable fields from users of your class. Immutable classes don’ t declare setter methods. This also @Autowired
 @Qualifier ("JsonProcessor" )
 public void setJsonProcessor (Processor jsonProcessor ) {
 this.jsonProcessor = jsonProcessor ;
 }
class MyServiceImpl {
 @Autowired
 @Qualifier ("XmlProcessor" )
 Processor xmlProcessor ;
 public void processXml () {
 xmlProcessor .process ();
 // ....
 }

enforces that you have the valid objects at the construction time. It also prompts you to rethink about your design when you have too many constructor
parameters.

---

## Q7: When will you favor DI type “Setter Injection” over “Constructor Injection”?

**Answer:**

In some scenarios, the constructors may get a lot of parameters, which force you to create a lot of overloaded constructors for every way the object might
be created. In these scenarios setter injection can be favored over constructor injection, but having too many constructor parameters may be an indication of a
bad design.
Q8 to Q13: 13 Spring basics Q8 – Q13 interview questions & answers .
More FAQ Spring Interview Questions & Answers:
1. 9 Spring Bean Scopes Interview Q&As
2. 17 Spring F AQ interview Questions & Answers
3. 30+ F AQ Hibernate interview questions & answers

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
