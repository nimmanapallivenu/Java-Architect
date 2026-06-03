# 05  9 Spring Bean scopes interview Q&A   Java Success.com

## Table of Contents

- [Q1: Does Spring dependency injection happen during compile time or runtime?](#q1)
- [Q2: What is the dif ference between prototype scope and singleton scope? Which one i](#q2)
- [Q3: When will you use singleton scope? When will you use prototype scope?](#q3)
- [Q4: Would both singleton and prototype bean’ s life cycle be managed by the Spring I](#q4)
- [Q5: What happens if you inject a prototype scoped bean into a singleton scoped bean?](#q5)
- [Q6: What if you want the singleton scoped bean to be able to acquire a brand new ins](#q6)
- [Q7: What are the scopes defined in HTTP context?](#q7)
- [Q8: Does Spring allow you to define your own bean scopes?](#q8)
- [Q9: When will you use a spring lookup method?](#q9)

---

## Q1: Does Spring dependency injection happen during compile time or runtime?

**Answer:**

Runtime during creating an object.

---

## Q2: What is the dif ference between prototype scope and singleton scope? Which one is the default?

**Answer:**

Singleton means single bean instance per IoC container , and prototype means any number of object instances per IoC container . The default scope is
“singleton”.

---

## Q3: When will you use singleton scope? When will you use prototype scope?

**Answer:**

Singleton scope is used for stateless object use. For example, injectiong a DAO (i.e. Data Access Object) into a service object. DAOs don’ t need to
maintain conversation state. For example,
Prototype is useful when your objects maintain state in a multi-threaded environment. Each thread needs to use its own object and cannot share the single
object. For example, you might have a RESTFul web service client making multi-threaded calls to W eb services. The REST easy client APIs like RESTEasy
uses the Apache Connection manager which is not thread safe and each thread should use its own client. Hence, you need to use the prototype scope.

---

## Q4: Would both singleton and prototype bean’ s life cycle be managed by the Spring IoC container?

**Answer:**

Yes and no. The singleton bean’ s complete life cycle will be managed by Spring IoC container , but with regards to prototype scope, IoC container only
partially manages the life cycle – instantiates, configures, decorates or assembles a prototype object, hands it to the client and then has no further knowledge of
that prototype instance. As per the spring documentation
“This means that while initialization lifecycle callback methods will be called on all objects regardless of scope, in the case of prototypes, any configured
destruction lifecycle callbacks will not be called. It is the responsibility of the client code to clean up prototype scoped objects and release any expensive<?xml version ="1.0" encoding ="UTF-8" ?>
<beans xmlns ="http://www .springframework.or g/schema/beans"
 xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://www .springframework.or g/schema/beans http://www .springframework.or g/schema/beans/spring-beans-3.0.xsd" >
 <bean id="myDaoDef" class ="com.mycompany .understanding.spring.MyDaoImpl" scope ="singleton" />
 
 <bean id="myServiceDef" class ="com.mycompany .understanding.spring.MyServiceImpl" scope ="singleton" >
 <constructor -arg name ="myDao" ref="myDaoDef" />
 </bean >
</beans >

resources that the prototype bean(s) are holding onto.”

---

## Q5: What happens if you inject a prototype scoped bean into a singleton scoped bean?

**Answer:**

A new prototype scoped bean will be injected into a singleton scoped bean once at runtime, and the same prototype bean will be used by the singleton bean.

---

## Q6: What if you want the singleton scoped bean to be able to acquire a brand new instance of the prototype-scoped bean again and again at runtime?

**Answer:**

In this use-case, there is no use in just dependency injecting a prototype-scoped bean into your singleton bean, because as stated above, this only happens
once when the Spring container is instantiating the singleton bean whilst resolving and injecting its dependencies. Y ou can just inject a singleton (e.g. a factory)
bean and then use Java class to instantiate (e.g with a newInstance(…) or create(..) method) a new bean again and again at runtime without relying on Spring or
alternatively have a look at Spring’ s “method injection”. As per Spring documentation for “ Lookup method injection ”
“Lookup method injection refers to the ability of the container to override methods on container managed beans, to return the result of looking up another
named bean in the container . The lookup will typically be of a prototype bean as in the scenario described above. The Spring Framework implements this
method injection by dynamically generating a subclass overriding the method, using bytecode generation via the CGLIB library”. Spring lookup-method
example to inject prototype scoped bean into a singleton scoped bean

---

## Q7: What are the scopes defined in HTTP context?

**Answer:**

Following scopes are only valid in the context of a web-aware Spring ApplicationContext.
Request Scope is for a single bean definition to the lifecycle of a single HTTP request. In other words each and every HTTP request will have its own instance
of a bean created of f the back of a single bean definition.
Session Scope is for a single bean definition to the lifecycle of a HTTP Session.
Global Session Scope is for a single bean definition to the lifecycle of a global HTTP Session. T ypically only valid when used in a portlet context.

---

## Q8: Does Spring allow you to define your own bean scopes?

**Answer:**

Yes, from Spring 2.0 onwards you can define custom scopes. For example,
— You can define a ThreadOrRequest and ThreadOrSession scopes to be able to switch between the environment you run in like JUnit for testing and Servlet
container for running as a W eb application.
— You can write a custom scope to inject stateful objects into singleton services or factories.
— You can write a custom bean scope that would create new instances per each JMS message consumed.
— Oracle Coherence has implemented a datagrid scope for Spring beans. Y ou will find many others like this.

---

## Q9: When will you use a spring lookup method?

**Answer:**

to inject a new prototype scoped been into a singleton scoped bean. Spring lookup-method example to inject prototype scoped bean into a singleton scoped
bean .

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
