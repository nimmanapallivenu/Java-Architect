# Spring Cloud Microservices interview Q&As   Java Success.com

## Table of Contents

- [Q1: What is Spring Cloud?](#q1)
- [Q2: What are some of the key Spring Cloud modules?](#q2)
- [Q3: Why is it necessary to centralise configuration in cloud based Micro Services ap](#q3)
- [Q4: Why is service discovery critical in Microservices application?](#q4)
- [Q5: What is the use of client side resiliency pattern?](#q5)
- [Q6: What is Netflix Feign?](#q6)
- [Q7: What is service gate way?](#q7)
- [Q8: How do you trace micro services applications?](#q8)

---

## Q1: What is Spring Cloud?

**Answer:**

Spring Boot is widely used to develop MicroServices. As many or ganisations deploy these services on the cloud like A WS, etc you need to take care of various aspects to make it cloud native, hence Spring Cloud was created.
Spring Cloud is an implementation of various design patterns for building Cloud Native applications. Y ou can make use of various Spring Cloud modules using the common patterns like:
— Routing Pattern
— Core Pattern
— Security Pattern
— Build and deployment pattern
— Client resiliency pattern
— Logging and tracing patterns
to build distributed systems .

---

## Q2: What are some of the key Spring Cloud modules?

**Answer:**

Following modules take care of common patterns out of the box to build distributed systems. Coordination of distributed systems leads to boiler plate patterns, and using Spring Cloud developers can quickly stand up services and
applications that implement those patterns.
Spring Cloud takes a very declarative appr oach , and often you get a lot of features with just a classpath change and/or an annotation as shown below in the code snippets.
Spring Cloud Config Server: is used to externalise the configuration of applications in a central config server with the ability to update the configuration values without requiring to restart the applications. Y ou can use the Spring Cloud
Config Server with git, Consul, or ZooKeeper as a config repository .
Like all Spring Boot applications, it runs on port 8080 by default, but you can switch it to the more conventional port 8888 in various ways.
Service Registry and Discovery: to solve the problem of dynamically scaling up & down of many services without hard coded hostnames & port numbers for the inter service communications. Spring Cloud provides Netflix Eureka-
based Service Registry and Discovery support with just minimal configuration. Y ou can also use Consul , Eureka or ZooKeeper for Service Registry and Discovery .
Circuit Br eaker: as in Micro Services architecture one service may depend on another service, and if one service goes down, then failures may cascade to other services as well. Spring Cloud provides a Netflix Hystrix Circuit Breaker to
handle these kinds of issues.@SpringBootApplication
@EnableConfigServer
public class ConfigServer {
public static void main (String [] args) {
 SpringApplication .run(ConfigServer .class , args);
}
}
@SpringBootApplication
@EnableDiscoveryClient
public class Application {
 public static void main (String [] args) {
 SpringApplication .run(Application .class , args);
 }
}

The pom.xml will have
You can refer to the Spring Guides for an example Spring Guides Circuit Breaker .
Spring Cloud Data Str eams: allows to integrate microservices with message brokers RabbitMQ, Apache Kafka, Kafka Streams, Amazon Kinesis, Google PubSub, Azure Event Hubs, etc. This enables you to work with huge volumes of
data streams using Kafka or Spark. Spring Cloud Data Streams provides higher -level abstractions to use message brokers more easily .
Spring Cloud Security: is an authentication and authorization framework that can control who can access your services and what they can do with your services. In spring cloud security microservices communicate using tokens (E.g.
SAML, JWT – JSON W eb Token). Receiving service first check token to validate caller .
Many MicroServices need to be accessible to authenticated users only , hence Spring Cloud Security provides authentication services using OAuth2.
The pom.xml will have
Configuration class in your Client application,
where any requests that require authentication will be redirected to the Authorization Server . For this to work you also have to define the server properties:...
 <dependency >
 <groupId >org.springframework .cloud </groupId >
 <artifactId >spring -cloud -starter -netflix -hystrix </artifactId >
 </dependency >
...
 <dependency >
 <groupId >org.springframework .cloud </groupId >
 <artifactId >spring -cloud -starter -oauth2 </artifactId >
 <version >2.2.2.RELEASE </version >
 </dependency >
.....
@Configuration
@EnableOAuth2Sso
public class SiteSecurityConfigurer
extends WebSecurityConfigurerAdapter {
 @Override
 protected void configure (HttpSecurity http) throws Exception {
 // ... 
 }

Distributed T racing: as one of the pain points with any distributed architectures like Micro Services is the ability to debug issues. One simple end-user action might trigger a chain of Micro Service calls, hence there should be a mechanism
to trace the related call chains. Y ou can use Spring Cloud Sleuth with Zipkin to trace cross-service invocations.
Spring Cloud Contract: is required as there will be separate teams working on dif ferent microservices. A mechanism is required to agree upon API endpoint contracts so that each team can develop their APIs independently . Spring Cloud
Contract helps to create such contracts and validate them by both the service provider and consumer .
you can add the Spring Cloud Contract V erifier dependency and plugin to your build file, as the following example shows:
You can refer to Spring-Cloud for all the Spring-cloud-xxxxx modules as in spring-cloud-common, spring-cloud-config, spring-cloud-contract, spring-cloud-security , spring-cloud-aws, spring-cloud-zookeeper , spring-cloud-gateway ,
spring-cloud-consul, spring-cloud-sleuth, spring-cloud-stream, etc.
There are other modules for Load Balancing, Global locks, Leadership election and cluster state, etc. Y ou can refer to Spring Cloud Doco .

---

## Q3: Why is it necessary to centralise configuration in cloud based Micro Services applications?

**Answer:**

In cloud based Microservices application, there will be many microservices, and it will be really dif ficult to maintain separate repository for each microservice. So, it is good practice to maintain a centralised repository to hold application
configurations.ecurity :
oauth2 :
 client :
 accessT okenUri : http://localhost:7090/authserver/oauth/token
 userAuthorizationUri : http://localhost:7090/authserver/oauth/authorize
 clientId : authserver
 clientSecret : password123
 resource :
 userInfoUri : http://localhost:8080/order
<dependency >
 <groupId >org.springframework .cloud </groupId >
 <artifactId >spring -cloud -starter -contract -verifier </artifactId >
 <scope >test</scope >
</dependency >
<plugin >
 <groupId >org.springframework .cloud </groupId >
 <artifactId >spring -cloud -contract -maven -plugin </artifactId >
 <version >${spring -cloud -contract .version }</version >
 <extensions >true</extensions >
</plugin >

---

## Q4: Why is service discovery critical in Microservices application?

**Answer:**

Service discovery is the process of finding the physical address of the service where that service is deployed. When a service consumer wants to consume any service, it needs service location. T o get service location it calls the service
discovery and gets all service instances of that service
— A service consumer will not know the physical address of the service instance, hence using a service discovery , an application developer can not only discover the service, but also can horizontally scale up and down service instance
running in an environment.
— It also increases application resiliency & availability by removing unavailable service instances from its list. Service lookups are shared among all nodes of service discovery cluster . So, even if a node becomes unavailable then other nodes
can take over .
— The service lookup invocations are also load balanced by ensuring that when service invocation happens then invocation is spread across all instances.

---

## Q5: What is the use of client side resiliency pattern?

**Answer:**

Even though microservice architecture isolates failures through defined boundaries, there is a high chance of network, hardware, database, or application issues, which will lead to the temporary unavailability of a component.
This pattern protects the client from crashing when a remote resource is failing due to poor performance or error .
Retry & Fallback pattern
Retry provides the ability to invoke failed operations, which is very helpful when errors are transient in nature. It will retry the failed operation for configured times and then proceed to the fallback mechanism like returning the data from the
cache or the default value.
Circuit Breaker pattern
In general you tend to use time-outs to limit the duration of operations which will prevent hanging of an operation. But most of us are not able to predict the perfect timeout which will be suitable for all the operations in this dynamic
environment, so it is called an anti-pattern.
Circuit breakers came to deal with the above problems and are very helpful in a distributed systems where repetitive failur es can bring down the whole system down . If any service throws an error of a particular type continuously over a
short period, then the circuit breaker will open the connections so that no service can communicate with that until it becomes stable. Not all errors will trigger the circuit breaker .
Bulkhead pattern
Bulkheads are used to prevent faults in one part of a system taking the entire system down by limiting the number of concurrent calls to a component. It is mainly used to segregate resources. For example, if service has two components, the
components 1 start hanging, it will result in all threads hanging. T o avoid this, bulkheads will separate the number of threads and only threads allocated to component 1 will hang and others will be there to process component 2’ s requests.
Client side load balancing
Server side load balancing is involved in monolithic applications where you have limited number of application instances behind the load load balancer . Server side load balancing requires a manual ef fort and you need to add/remove instances
manually to the load balancer for it to work. By doing this manually you are loosing today’ s on demand scalability to auto-discover and configure when any new instances are spun of f. Another problem is to have a fail-over policy to provide
the client a seamless experience.
To overcome the above mentioned problems of traditional load balancing, client side load balancing came into picture. They reside in the application as inbuilt component and bundled along with the application, so you don’ t have to
deploy them in separate servers.
If one microservice wants to communicate with another microservice, it generally looks up the service registry using discovery client like Consul or Eureka server to return all the instances of that tar get microservice to the caller service. Then
it is the responsibility of the caller service to choose which instance to send request.
Netflix Ribbon from Spring Cloud family provides such facility to set up client side load balancing along with the service registry component.
<dependency >
 <groupId >com.netflix .ribbon </groupId >
 <artifactId >ribbon </artifactId >

---

## Q6: What is Netflix Feign?

**Answer:**

Feign is a declarative web service client. It makes writing web service clients easier . To use Feign create an interface and annotate it.
It has pluggable annotation support including Feign annotations and JAX-RS annotations. Feign also supports pluggable encoders and decoders. Spring Cloud adds support for Spring MVC annotations and for using the same
HttpMessageConverters used by default in Spring W eb. It also supports Netflix ribbon for client side load balancing.

---

## Q7: What is service gate way?

**Answer:**

In distributed microservices application, cross cutting concern like logging, security , etc are done through separate independent service and call to other service are routed through this independent service.This service is called service
gateway (E.g. Netflix/zuul ). It acts as an intermediary between the service client and a service being invoked.
 <version >2.2.2 </version >
</dependency >
@SpringBootApplication
@EnableFeignClients
public class Application {
 public static void main (String [] args) {
 SpringApplication .run(Application .class , args);
 }
@FeignClient ("stores" )
public interface StoreClient {
 @RequestMapping (method = RequestMethod .GET , value = "/stores" )
 List<Store > getStores ();
 @RequestMapping (method = RequestMethod .POST , value = "/stores/{storeId}" , consumes = "application/json" )
 Store update (@PathV ariable ("storeId" ) Long storeId , Store store );

Micro Services – Security
Routing is an integral part of a microservice architecture. For example, / may be mapped to your web application, /api/users is mapped to the user service and /api/shop is mapped to the shop service. Zuul is a JVM-based router and server -
side load balancer from Netflix.
application.yml

---

## Q8: How do you trace micro services applications?

**Answer:**

Spring cloud Sleuth implements a distributed tracing solution for Spring Cloud. It integrates unique tracking identifiers into the HTTP calls and message channels, which helps to track a transaction as it flows across the dif ferent
services in your application.
Spring Cloud T utorials
Spring Cloud T utorialszuul:
routes :
 users :
 path: /myusers /**
 url: https ://example.com/users_service

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
