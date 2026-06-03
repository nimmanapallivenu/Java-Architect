# 15 Spring Boot interview Q&As   Java Success.com

## Table of Contents

- [Q1: What is the key benefit of using Spring boot?](#q1)
- [Q2: How does spring boot enable you to “build a production ready application from sc](#q2)
- [Q3: How do set up a Spring Boot application with Maven?](#q3)
- [Q4: How do you specify dependencies in Spring boot?](#q4)
- [Q5: How will you get Spring boot to use Jetty server instead of T omcat, which is th](#q5)
- [Q6: What documentation would you be using to get started with your enterprise Spring](#q6)
- [Q7: What is Spring Boot CLI?](#q7)
- [Q8: What are the dif ferent ways to generate a Spring boot project?](#q8)
- [Q9: What is the dif ference between Spring IO & Spring Boot?](#q9)
- [Q10: What are the key components of Spring Boot framework?](#q10)
- [Q11: How does Spring Boot work under the hood to simplify the build dependency & conf](#q11)
- [Q12: What are the benefits of using Spring Boot in your next micro-service applicatio](#q12)
- [Q13: How do you disable a specific auto-configuration in Spring Boot?](#q13)
- [Q14: How do you register your own custom auto-configuration?](#q14)
- [Q15: Can you conditionally include a configuration bean?](#q15)

---

## Q1: What is the key benefit of using Spring boot?

**Answer:**

The key benefit is that you can “build a production ready application from scratch in a matter of minutes”.
Over the years since its inception, Spring has grown to be very complex in terms of the amount of configuration an application requires. This is where Spring
Boot comes in handy to simplify the configuration with opinionated defaults & inclusion of the libraries for you to get started quickly .

---

## Q2: How does spring boot enable you to “build a production ready application from scratch in a matter of minutes”?

**Answer:**

It takes the approach of “ convention over configuration “.
1) The Spring jars dependency management and versioning are simplified as demonstrated in the spring boot example – Simple Spring Boot T utorial in 8 steps
Spring Boot’ s main benefit is its ability to configure resources based on what it finds in your classpath. If your Maven POM includes JP A dependencies and a
PostgreSQL driver , then Spring Boot will setup a persistence unit based on PostgreSQL. If you’ve added a web dependency , then you get Spring MVC
configured with sensible defaults.
2) Spring boot is based on an HTTP server . Spring Boot has an embedded version of T omcat by default, but gives you a way to opt for Jetty server if you wish.
This is demonstrated via Simple Spring Boot Restful W eb Service T utorial

---

## Q3: How do set up a Spring Boot application with Maven?

**Answer:**

Inheriting the spring-boot-starter -parent project in pom.xml.
<parent >
 <groupId >org.springframework .boot</groupId >
 <artifactId >spring -boot-starter -parent </artifactId >
 <version >2.1.1.RELEASE </version >
</parent >

Q. What if your enterprise has its own parent artefact to inherit from?
A. You can still get the benefits of dependency management as in

---

## Q4: How do you specify dependencies in Spring boot?

**Answer:**

Via spring-boot-starter -xxxxx .<dependencyManagement >
 <dependencies >
 <dependency >
 <groupId >org.springframework .boot</groupId >
 <artifactId >spring -boot-dependencies </artifactId >
 <version >2.1.1.RELEASE </version >
 <type>pom</type>
 <scope >import </scope >
 </dependency >
 </dependencies >
</dependencyManagement >
<dependencies >
<dependency >
 <groupId >org.springframework .boot</groupId >
 <artifactId >spring -boot-starter -web</artifactId >
</dependency >
<dependency >
 <groupId >org.springframework .boot</groupId >
 <artifactId >spring -boot-starter -data-jpa</artifactId >
</dependency >
<dependency >

Since “h2” database dependency is used, Spring boot will configure JP A persistence unit for H2 rather than the HSQLDB, which is the default. This approach is
known as the “Opinionated Defaults Configuration ”

---

## Q5: How will you get Spring boot to use Jetty server instead of T omcat, which is the default?

**Answer:**

By adding the jetty server dependency “ spring-boot-starter -jetty ” in the pom.xml file.

---

## Q6: What documentation would you be using to get started with your enterprise Spring boot application?

**Answer:**

“https://spring.io/docs” has lots of getting started guides http://spring.io/guides & tutorials.

---

## Q7: What is Spring Boot CLI?

**Answer:**

CLI stands for Command Line Interface, which is a Spring Boot software to run and test Spring Boot applications from command prompt. When you run
Spring Boot applications using CLI, then it internally uses Spring Boot Starter and Spring Boot AutoConfigurate components to resolve all dependencies
and execute the application. It internally contains Groovy and Grape (JAR Dependency Manager) to add Spring Boot Defaults and resolve all dependencies
automatically .

---

## Q8: What are the dif ferent ways to generate a Spring boot project?

**Answer:**

1) Using Maven as demonstrated in Simple Spring Boot T utorial in 8 steps
2) Via online Spring Initializr – “http://start.spring.io/ “. This is demonstrated at Kubernetes (i.e. Minikube) – deploy a spring-boot app
3) Via Spring Boot CLI .
4) Via Spring Boot IDE .

---

## Q9: What is the dif ference between Spring IO & Spring Boot?

**Answer:**

Spring IO Platform is all about “list of dependencies and their versions that work well together”. It is implemented as a Maven POM file via Maven Bill-of- <groupId >com.h2database </groupId >
 <artifactId >h2</artifactId >
 <version >1.3.174 </version >
</dependency >
<dependency >
 <groupId >org.springframework .boot</groupId >
 <artifactId >spring -boot-starter -test</artifactId >
 <scope >test</scope >
</dependency >
</dependencies >

Materials dependency that you can import into your projects to set the versions for dependencies. Spring IO tutorial in 6 steps
Spring Boot is built on top of the “Spring IO” platform. Spring Boot makes it easy to create stand-alone, production-grade Spring applications that you can just
run as covered in “ Simple Spring Boot T utorial in 8 steps ” and Simple Spring Boot Restful W eb Service T utorial .

---

## Q10: What are the key components of Spring Boot framework?

**Answer:**

Spring Boot has 4 key components.
1) Spring Boot Starter : is responsible for combining a group of common or related dependencies. E.g. spring-boot-starter -actuator , spring-boot-starter -web,
spring-boot-starter -data-rest, spring-boot-starter -hateoas, spring-boot-starter -jdbc, spring-boot-starter -tomcat, etc.
2) Spring Boot AutoConfigurator : is responsible for simplifying the wiring up of Spring components. One of the common criticisms of Spring IO
framework is that it requires lot of XML or Java based configurations. The Spring Boot AutoConfigurator component will take the burden of wiring up the
Spring components. It also reduces the number of annotations. For example, @SpringBootAnnotation = @Configuration + @ComponentScan +
@EnableAutoConfiguration.
3) Spring Boot CLI : is responsible for running & testing a Spring Boot application from a command prompt. It internally uses the components “Spring Boot
Starters” and “Spring Boot AutoConfigurator”. Y ou can also run Spring W eb Applications from a command prompt.
4) Spring Boot Actuator : is responsible for providing production-ready features to a Spring Boot application without having to actually implement these
things yourself. it exposes dif ferent types of information about the running application – health, metrics, info, env etc. This is not a replacement for a
production-grade monitoring solution, but is a good starting point from a development & testing perspective.

---

## Q11: How does Spring Boot work under the hood to simplify the build dependency & configuration?

**Answer:**

Spring Boot internally uses Groovy to tap into its features such as JAR dependency resolver engine (i.e. GRAPE) and default import statements.

---

## Q12: What are the benefits of using Spring Boot in your next micro-service application?

**Answer:**

You can quickly build a stand-alone production ready application. It reduces lots of development time and increases the overall productivity due to
1) Lesser dependency management ef fort.
2) Lesser boiler plate code to wire up Spring components.
3) Easier to integrate within Spring ecosystems like spring-jdbc, spring-web, spring-orm, spring-data, spring-security , etc.
4) It follows “Opinionated Configuration Defaults” approach.
5) It provides embedded HTTP servers like T omcat and Jetty to test your applications.

6) It provides lots of plugins to develop & test Spring applications with Maven & Gradle. For example, “spring-boot-maven-plugin” to create uber jars that can
be deployed to a web server .
7) Spring Boot CLI tool helps to develop & test from a command line.

---

## Q13: How do you disable a specific auto-configuration in Spring Boot?

**Answer:**

For example, if you want to disable all the database related auto configuration:
1. Annotation:
2. Using “application.pr operties”:

---

## Q14: How do you register your own custom auto-configuration?

**Answer:**

Register your custom configuration class in “MET A-INF/ spring.factories “.@SpringBootApplication
@EnableAutoConfiguration (exclude = {DataSourceAutoConfiguration .class , DataSourceT ransactionManagerAutoConfiguration .class ,
HibernateJpaAutoConfiguration .class })
public class Application {
 public static void main (String [] args) {
 SpringApplication .run(RunMyApplication .class , args);
 }
spring .autoconfigure .exclude =org.springframework .boot.autoconfigure .jdbc.DataSourceAutoConfiguration ,
org.springframework .boot.autoconfigure .orm.jpa.HibernateJpaAutoConfiguration

Step 1: Create a custom configuration for a MySQL data source:
Step 2: Register the class as an auto-configuration candidate in “resources/MET A-INF/ spring.factories ”

---

## Q15: Can you conditionally include a configuration bean?

**Answer:**

Yes. Annotations @ConditionalOnClass & @ConditionalOnMissingClass allow you to specify that a configuration bean will be included if a
specified class is pr esent or if a specified class is absent respectively .@Configuration
public class MySQLAutoConfig {
 //...
}
org.springframework .boot.autoconfigure .EnableAutoConfiguration =
com.myapp .autoconfiguration .MySQLAutoconfiguration
@Configuration
@ConditionalOnClass (DataSource .class )
public class MySQLAutoConfig {
 //...
}

Similarly , you can also use @ConditionalOnBean and @ConditionalOnMissingBean annotations to include a bean only if a specified bean is pr esent
or not .
Sometimes, we might want to load a bean only if a certain other bean is available in the application context. The “DependantModule” is only loaded if there is a
bean of class “OtherModule” in the application context. Y ou could also define the bean name instead of the bean class.
If you want to conditionally configure the JpaT ransactionManager if it is missing:
There are other @ConditionalXXXXXX annotations, and you can create your own custom with the meta annotation @Conditional .@Configuration
@ConditionalOnBean (OtherModule .class )
class DependantModule {
...
}
@Bean
@ConditionalOnMissingBean (type = "JpaT ransactionManager" )
JpaT ransactionManager transactionManager (EntityManagerFactory entityManagerFactory ) {
 JpaT ransactionManager transactionManager = new JpaT ransactionManager ();
 transactionManager .setEntityManagerFactory (entityManagerFactory );
 return transactionManager ;
}

and use it as:
Spring boot other links
1. Spring Boot W eb & Actuator Beginner T utorial Step by Step .
2. Spring Cloud Microservices interview questions & answers
3. Tutorials – Spring Cloud@Target({ ElementT ype.TYPE , ElementT ype.METHOD })
@Retention (RetentionPolicy .RUNTIME )
@Documented
@Conditional (OnOSCondition .class )
public @interface ConditionalOnOpSys {}
@Bean
@ConditionalOnOpSys
OSBean osBean (){
return new OSBean ();
}

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
