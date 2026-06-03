# 01b  Q08   Q13 Spring interview Q&As   Java Success.com

## Table of Contents

- [Q8: Can you describe the high level Spring architecture?](#q8)
- [Q9: What are the packages (i.e. jar files) required in your project to get started w](#q9)
- [Q10: Can you describe the bean life cycle?](#q10)
- [Q11: What is a BeanFactory?](#q11)
- [Q12: How do you bootstrap the initial bean?](#q12)
- [Q13: What would you do, if it’ s not practical (or impossible) to wire up your entire](#q13)

---

## Q8: Can you describe the high level Spring architecture?

**Answer:**

A Spring Bean represents a POJO (Plain Old Java Object) performing useful operation(s). All Spring Beans reside within a Spring IoC Container . The
Spring Framework hides most of the complex infrastructure and the communication that happens between the Spring Container and the Spring Beans. The Core
Container is shown below .

Spring Architecture
Spring framework architecture is modular with layers like core, data access & integration, web/remoting, and other miscellaneous support.

---

## Q9: What are the packages (i.e. jar files) required in your project to get started with a Spring application?

**Answer:**

In order to get started with Spring, your maven pom.xml file should at least have the following core Spring packages :
<dependency >
 <groupId >org.springframework </groupId >
 <artifactId >spring -core</artifactId >
 <version >${spring .version }</version >
</dependency >
<dependency >
 <groupId >org.springframework </groupId >
 <artifactId >spring -expression </artifactId >
 <version >${spring .version }</version >
</dependency >
<dependency >
<groupId >org.springframework </groupId >
<artifactId >spring -beans </artifactId >
<version >${spring .version }</version >
</dependency >
<dependency >
<groupId >org.springframework </groupId >
<artifactId >spring -aop</artifactId >

The following additional packages can be added based on the requirements:
spring-context-support package is required for integration of EhCache, JavaMail, Quartz, and Freemarker . So, if you are going to use Java Mail to send emails
or Quartz to schedule a job, then you need spring-context-support.
spring-tx package is required for transaction management support, and it depends on spring-core, spring-beans, spring-aop, and spring-context packages.
spring-jdbc and spring-orm are required for the database access. spring-jdbc depends on spring-core, spring-beans, spring-context, and spring-tx. if using
Object-to-Relation-Mapping (ORM) integration with Hibernate, JP A or iBatis then you need spring-orm, which depends on spring-core, spring-beans, spring-
context, and spring-tx.
spring-oxm is required for JAXB, JiBX,XStream, XMLBeans or any other Object-T o-Xml(OXM) mapping. You need spring-oxm, which depends on spring-
core, spring-beans, and spring-context.
Similarly ,
spring-test is required for junit testing.
spring-web is required if you want to use a web framework like Spring MVC, JSF , Struts, etc, and depends on depends on spring-core, spring-beans, and
spring-context.
spring-webmvc is required to use Spring as the MVC framework for W eb application or RESTFul web service. It depends on spring-core, spring-beans, spring-
context, and spring-web.
spring-mock containing mock classes to assist with the testing.
spring-jms for messaging and depends on spring-core and spring-oxm (i.e. for OXM).

---

## Q10: Can you describe the bean life cycle?

**Answer:**

A Spring Bean represents a POJO (Plain Old Java Object) performing useful operation(s). All Spring Beans reside within a Spring IoC Container . The
Spring Framework hides most of the complex infrastructure and the communication that happens between the Spring Container and the Spring Beans.<version >${spring .version }</version >
</dependency >
<dependency >
<groupId >org.springframework </groupId >
<artifactId >spring -context </artifactId >
<version >${spring .version }</version >
</dependency >

Spring Bean life cycle means the construction and destruction of the beans and usually this is in relation to the construction and destruction of the Spring
Context. Spring has three ways of calling your code during initialization and shut down.
1. Pr ogrammatically , usually called ‘interface callbacks’ : Spring calls your bean during the setup and tear down of the Spring Context, and your bean needs
to implement InitializingBean or DisposableBean . Spring 3.0 has the Lifecycle interface with start/stop lifecycle control methods. The typical use case for this
is to control asynchronous processing.
2. Declarative ‘method callbacks’ on a per bean basis : You use a method callback by adding a method to your bean, which you then reference in your XML
config. When Spring reads the config it figures out that there’ s a bean of type “A” with a method that it needs to call on startup and another it needs to call on
shutdown.
3. Declarative ‘method callbacks’ to all beans .
Initialization:
Step 1: The spring container finds the bean’ s definition from the XML file or annotations (like @Configuration ) and instantiates the bean.
Step 2: Using the dependency injection, spring populates all of the bean properties as specified in the bean definition.
Step 3: If the bean implements the BeanNameA ware interface, the factory calls “setBeanName()” passing the bean’ s ID.
Step 4: If the bean implements the BeanFactoryA ware interface, the factory calls “setBeanFactory()”, passing an instance of itself.
Step 5: If the bean implements the ApplicationContextFactoryA ware interface, the container calls “bean.setApplicationContext(container)”.
Step 6: If there are any BeanPostPr ocessors associated with the bean, their “postProcessBeforeInitialization()” method will be called.
Step 7a: If the bean implements InitializingBean interface, “bean.afterPropertiesSet()” method will be invoked.
Step 7b: If the bean declares custom init method, the container calls custom init method of that bean
Step 8: If there are any BeanPostPr ocessors associated with the bean, their “postProcessAfterInitialization()” method will be called.
Step 9: Bean is now ready for use.<bean id="myAppBean" class ="com.myapp.MyAppBeanImpl" init-method ="init" destroy -method ="destroy" />

shutdown:
Step 1: If the bean implements DisposableBean interface, container calls the “bean.destroy()”.
Step 2: If the bean declares custom destroy method, container calls custom destroy method of bean.
Bean Life Cycle Example
Step 1: myApp-applicationContext.xml file
Step 2: MyAppBeanImpl bean with business logic interface MyAppBean, and life cycle interfaces BeanNameA ware and BeanFactoryA ware<bean id="myAppBean" class ="com.myapp.MyAppBeanImpl" />
package com.myapp ;
public class MyAppBeanImpl implements MyAppBean , BeanNameA ware , BeanFactoryA ware {
 @Override
 public String sayHello () {
 return "Hello, I am initialized" ;
 }
 @Override
 public void setBeanFactory (BeanFactory beanFactory ) throws BeansException {
 System .out.println ("received the beanFactory " + beanFactory );
 }
 @Override
 public void setBeanName (String name ) {

Step 3: To test the bean initialization, the cogif file “myApp-applicationContext.xml” is read via XmlBeanFactory class.
Step 4: : The output of the code is:
the name of the bean is myAppBean
received the beanFactory or g.springframework.beans.factory .xml.XmlBeanFactory@f6f542:
defining beans [myAppBean]; root of factory hierarchy
Hello, I am initialized

---

## Q11: What is a BeanFactory?

**Answer:**

The BeanFactory is the actual container which instantiates, configures, and manages a number of beans. These beans typically collaborate with one
another , and thus have dependencies between themselves.

---

## Q12: How do you bootstrap the initial bean?

**Answer:**

Beans are wired up inside Spring XML file or via annotations like @Component, @Resource, etc. The initial bean needs to be bootstrapped, and there are
a number of approaches as shown below .
1. Using the “ ClassPathXmlApplicationContext ” class in Spring System .out.println ("the name of the bean is " + name );
 }
public static void main (String [] args) {
 final XmlBeanFactory beanFactory = new XmlBeanFactory (
 new ClassPathResource ("myApp-applicationContext.xml" ));
 MyAppBean myApp = (MyAppBean ) beanFactory .getBean ("myAppBean" );
 System .out.println (myApp .sayHello ());
}

2. Using the “ FileSystemResour ce” class in Spring
3. Using the “ ClassPathResour ce”mport org.springframework .beans .factory .BeanFactory ;
mport org.springframework .context .support .ClassPathXmlApplicationContext ;
public class TestSpring {
 
 public static void main (String [] args) {
 ClassPathXmlApplicationContext appContext = new ClassPathXmlApplicationContext (
 new String [] {"myApp-applicationContext.xml" });
 BeanFactory factory = (BeanFactory ) appContext ;
 MyAppBean myApp = (MyAppBean ) factory .getBean ("myBean" );
 System .out.println (myApp .sayHello ());
 } 
mport org.springframework .beans .factory .xml.XmlBeanFactory ;
mport org.springframework .core.io.FileSystemResource ;
mport org.springframework .core.io.Resource ;
public class TestSpring {
 
 public static void main (String [] args) {
 Resource res = new FileSystemResource ("bin/test/myApp-applicationContext.xml" );
 XmlBeanFactory factory = new XmlBeanFactory (res);
 MyAppBean myApp = (MyAppBean ) factory .getBean ("myBean" );
 System .out.println (myApp .sayHello ());
 } 

4. For @Configuration annotation driven configurations use AnnotationConfigApplicationContextmport org.springframework .beans .factory .xml.XmlBeanFactory ;
mport org.springframework .core.io.ClassPathResource ;
public class TestSpring {
 
 public static void main (String [] args) {
 ClassPathResource res = new ClassPathResource ("myApp-applicationContext.xml" );
 XmlBeanFactory factory = new XmlBeanFactory (res);
 MyAppBean myApp = (MyAppBean ) factory .getBean ("myBean" );
 System .out.println (myApp .sayHello ());
 } 
@Configuration
public class AppConfig {
 @Bean
 public MyAppBean myBean () {
 // instantiate, configure and return bean ...
 }
}
public class TestSpring {
 
 public static void main (String [] args) {

5. Fr om a W eb application . As opposed to the BeanFactory , which will often be created programmatically , ApplicationContexts can be created declaratively
using a ContextLoader . You can register an ApplicationContext using the ContextLoaderListener as shown below in the web.xml file. The Spring context
listener provides more flexibility in terms of how an application is wired together . It uses the application’ s Spring configuration to determine what object to
instantiate and loads the objects into the application context used by the servlet container .
By default, it looks for a file named applicationContext.xml file in WEB-INF folder . But, you can configure the
org.springframework.web.context.ContextLoaderListener class to use a context parameter called contextConfigLocation to determine the location of the Spring
configuration file. The context parameter is configured using the context-parameter element. The context-param element has two children that specify
parameters and their values. The param-name element specifies the parameter ’s name. The param-value element specifies the parameter ’s value. AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext ();
 ctx.register (AppConfig .class );
 ctx.refresh ();
 MyAppBean myApp = ctx.getBean (MyAppBean .class )
 System .out.println (myApp .sayHello ());
 } 
<web-app>
.....
<listener >
 <listener -class >org.springframework .web.context .ContextLoaderListener </listener -class >
</listener >
....
</web-app>
<web-app>
...
<context -param >

---

## Q13: What would you do, if it’ s not practical (or impossible) to wire up your entire application into the Spring framework, but you still need a Spring loaded
bean in order to perform a task?

**Answer:**

For example,
— an auto generated web service client class! But you do want to use the dependency injection feature of Spring to get some of the other beans injected in to
this class.
— A legacy code that needs to make use of a Spring bean.
The ApplicationContextA ware interface provided by Spring allows you to wire some Java classes which are unable (or you don’ t want it) to be wired to the
Spring application context.
STEP 1: The ApplicationContextA ware interface makes sense when an object requires access to a set of collaborating beans. <param -name >contextConfigLocation </param -name >
 <param -value >WEB -INF/myApp -applicationContext .xml</param -value >
</context -param >
<listener >
 <listener -class >org.springframework .web.context .ContextLoaderListener </listener -class >
</listener >
...
</web-app>
mport org.springframework .beans .BeansException ;
mport org.springframework .context .ApplicationContext ;
mport org.springframework .context .ApplicationContextA ware ;
public class MyServiceFactory implements ApplicationContextA ware {
 
 private ApplicationContext context ;
 public void testMethod2 (){
 System .out.println ("Test method2 invoked ...." );
 }
 @Override
 public void setApplicationContext (ApplicationContext ctx)
 throws BeansException {
 System .out.println ("setting application context ..." );

STEP 2: The beans.xml file. The MyServiceFactory is not wired up.
Step 3: Finally , the bootstrapping code that makes use of the MyServiceFactory class. this.context = ctx;
 }

 public MyBeanService getInstance (String accessCode ) {
 //.....some logic
 MyBeanService beanService = (MyBeanService ) context .getBean ("myBeanService" );
 return beanService ;
 }
<bean id="myBeanDao" class ="test.MyBeanDaoImpl" />
<bean id="myBeanService" class ="test.MyBeanServiceImpl" >
 <!-- setter injection of dao into service -->
 <property name ="beanDao" ref="myBeanDao" />
</bean >
<!-- No DI wiring -->
<bean id="myServiceFactory" class ="test.MyServiceFactory" />
mport org.springframework .beans .factory .BeanFactory ;

Top 20+ Spring Interview Questions & Answers:
1. 9 Spring Bean Scopes Interview Q&As
2. 17 Spring F AQ interview Questions & Answers
3. 30+ Hibernate interview questions & answersmport org.springframework .context .support .ClassPathXmlApplicationContext ;
public class TestSpring2 {
 
 public static void main (String [] args) {
 ClassPathXmlApplicationContext appContext = new ClassPathXmlApplicationContext (
 new String [] {"beans.xml" });
 BeanFactory factory = (BeanFactory ) appContext ;
 MyServiceFactory servicefactory = (MyServiceFactory )factory .getBean ("myServiceFactory" );
 MyBeanService service = servicefactory .getInstance ("111");
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
