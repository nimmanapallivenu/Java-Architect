# 26 11 Spring boot & IO interview Q&As   Java Success.com

## Table of Contents

- [Q1: What is the key benefit of using Spring boot?](#q1)
- [Q2: How does spring boot enable you to “build a production ready application from sc](#q2)
- [Q3: How do you specify dependencies in Spring boot?](#q3)
- [Q4: How will you get Spring boot to use Jetty server instead of T omcat, which is th](#q4)
- [Q5: What documentation would you be using to get started with your enterprise Spring](#q5)
- [Q6: What is Spring Boot CLI?](#q6)
- [Q7: What are the dif ferent ways to generate a Spring boot project?](#q7)

---

## Q1: What is the key benefit of using Spring boot?

**Answer:**

The key benefit is that you can “build a production ready application from scratch in a matter of minutes”.

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

## Q3: How do you specify dependencies in Spring boot?

**Answer:**

Via spring-boot-starter -xxxxx .
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
 <groupId >com.h2database </groupId >
 <artifactId >h2</artifactId >
 <version >1.3.174 </version >
</dependency >
<dependency >
 <groupId >org.springframework .boot</groupId >

Since “h2” database dependency is used, Spring boot will configure JP A persistence unit for H2 rather than the HSQLDB, which is the default. This approach is
known as the “Opinionated Defaults Configuration ”

---

## Q4: How will you get Spring boot to use Jetty server instead of T omcat, which is the default?

**Answer:**

By adding the jetty server dependency “ spring-boot-starter -jetty ” in the pom.xml file.

---

## Q5: What documentation would you be using to get started with your enterprise Spring boot application?

**Answer:**

“https://spring.io/docs” has lots of getting started guides [http://spring.io/guides] & tutorials.

---

## Q6: What is Spring Boot CLI?

**Answer:**

CLI stands for Command Line Interface, which is a Spring Boot software to run and test Spring Boot applications from command prompt. When you run
Spring Boot applications using CLI, then it internally uses Spring Boot Starter and S pring Boot AutoConfigurate components to resolve all dependencies
and execute the application. It internally contains Groovy and Grape (JAR Dependency Manager) to add Spring Boot Defaults and resolve all dependencies
automatically .

---

## Q7: What are the dif ferent ways to generate a Spring boot project?

**Answer:**

1) Using Maven as demonstrated in Simple Spring Boot T utorial in 8 steps
2) Via online Spring Initializr – “http://start.spring.io/ “. More info at “http://spring.io/”.
3) Via Spring Boot CLI .
4) Via Spring Boot IDE .
Q8 What is the dif ference between Spring IO & Spring Boot?
A8. Spring IO Platform is all about “list of dependencies and their versions that work well together”. It is implemented as a Maven POM file via Maven Bill-of-
Materials dependency that you can import into your projects to set the versions for dependencies. Spring IO tutorial in 6 steps
Spring Boot is built on top of the “Spring IO” platform. Spring Boot makes it easy to create stand-alone, production-grade Spring applications that you can just
run as covered in “ Simple Spring Boot T utorial in 8 steps ” and Simple Spring Boot Restful W eb Service T utorial . <artifactId >spring -boot-starter -test</artifactId >
 <scope >test</scope >
</dependency >
</dependencies >

Q9 What are the key components of Spring Boot framework?
A9. Spring Boot has 4 key components.
1) Spring Boot Starter : is responsible for combining a group of common or related dependencies. E.g. spring-boot-starter -actuator , spring-boot-starter -web,
spring-boot-starter -data-rest, spring-boot-starter -hateoas, spring-boot-starter -jdbc, spring-boot-starter -tomcat, etc.
2) Spring Boot AutoConfigurator : is responsible for simplifying the wiring up of Spring components. One of the common criticisms of Spring IO framework
is that it requires lot of XML or Java based configurations. The Spring Boot AutoConfigurator component will take the burden of wiring up the Spring
components. It also reduces the number of annotations. For example, @SpringBootAnnotation = @Configuration + @ComponentScan +
@EnableAutoConfiguration.
3) Spring Boot CLI : is responsible for running & testing a Spring Boot application from a command prompt. It internally uses the components “Spring Boot
Starters” and “Spring Boot AutoConfigurator”. Y ou can also run Spring W eb Applications from a command prompt.
4) Spring Boot Actuator : is responsible for providing production-ready features to a Spring Boot application without having to actually implement these things
yourself. it exposes dif ferent types of information about the running application – health, metrics, info, env etc. This is not a replacement for a production-grade
monitoring solution, but is a good starting point from a development & testing perspective.
Q10 How does Spring Boot work under the hood to simplify the build dependency & configuration?
A10. Spring Boot internally uses Groovy to tap into its features such as JAR dependency resolver engine (i.e. GRAPE) and default import statements.
Q11 What are the benefits of using Spring Boot in your next micro-service application?
A11. You can quickly build a stand-alone production ready application. It reduces lots of development time and increases the overall productivity due to
1) Lesser dependency management ef fort.
2) Lesser boiler plate code to wire up Spring components.
3) Easier to integrate within Spring ecosystems like spring-jdbc, spring-web, spring-orm, spring-data, spring-security , etc.
4) It follows “Opinionated Configuration Defaults” approach.
5) It provides embedded HTTP servers like T omcat and Jetty to test your applications.
6) It provides lots of plugins to develop & test Spring applications with Maven & Gradle. For example, “spring-boot-maven-plugin” to create uber jars that can
be deployed to a web server .
7) Spring Boot CLI tool helps to develop & test from a command line.
Have you completed this unit? Then mark this unit as completed.

 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
