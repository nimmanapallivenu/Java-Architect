# 111Top 50+ Java EE interview Q&A on Spring, Hibernate,

## Table of Contents

- [Q29: What are the dif ferent ways can you wire up your dependencies using Spring?](#q29)
- [Q34: Can you list some real life scenarios where you will use Spring AOP?](#q34)
- [Q35: What is the link between EntityManagerFactory/EntityManager and SessionFactory/S](#q35)
- [Q36: Explain hibernate object states? Explain hibernate objects life cycle?](#q36)
- [Q37: What are the general steps involved in creating Hibernate related class?](#q37)

---

## Q29: What are the dif ferent ways can you wire up your dependencies using Spring?

**Answer:**

3 dif ferent ways. Y ou can combine all 3 ways.
Using an XML based application context file as demonstrated here in STEP 4 .
Using annotations @Resource, @Component, @repository and @Service, and @Autowired. See steps 2 and 3 in this Spring tutorial .
Using the Java config using the annotations @Configuration and @Bean. This is demonstrated via wiring up Spring JMS with W ebsphere MQ
Listener (Receiver or Subscriber) in step2 .
Q30-

---

## Q34: Can you list some real life scenarios where you will use Spring AOP?

**Answer:**

You can use the Method AOP interceptors for dead lock retry and profiling your application. Occasionally , you get deadlocks in databases and you
database management systems resolves deadlocks by aborting a transaction. These transactions can be retried with the help of Spring AOP method
interceptors. Example 1: Spring AOP with AspectJ for profiling Java applications , Example 2: Q12 demonstrating deadlock retry with Spring and
AspectJ.
Another area is service retry service in general where any failed remote service calls can be retried at a certain interval say every 10 seconds for 3
times.
for auditing purposes. Logging and auditing in Java with Spring AOP tutorial.
Spring itself uses aspects for things like transaction management, security , and caching.
If the interview you are attending require Spring knowledge & experience, then go through the following link 17 Spring F AQ interview Questions & Answers.
Spring Boot is widely used in new Spring projects to simplify Spring configurations. So, prepare for it with

1. 11 Spring boot interview Q&As .
2. 3 Step by step Spring tutorials .

---

## Q35: What is the link between EntityManagerFactory/EntityManager and SessionFactory/Session?

**Answer:**

EntityManagerFactory and EntityManager are JPA (java Persistence API). SessionFactory and Session are hibernate -specific. Prefer
EntityManagerFactory and EntityManager . The EntityManager invokes the hibernate session under the hood. And if you need some specific features that are not
available in the EntityManager , you can obtain the session by calling

---

## Q36: Explain hibernate object states? Explain hibernate objects life cycle?

**Answer:**

Persistent objects and collections are short lived single threaded objects, which store the persistent state. These objects synchronize their state with the
database depending on your flush strategy (i.e. auto-flush where as soon as setXXX() method is called or an item is removed from a Set, List etc or define your
own synchronization points with session.flush(), transaction.commit() calls). If you remove an item from a persistent collection like a Set, it will be removed
from the database either immediately or when flush() or commit() is called depending on your flush strategy . They are Plain Old Java Objects (POJOs) and are
currently associated with a session. As soon as the associated session is closed, persistent objects become detached objects and are free to be used directly as
data transfer objects in any application layers like business layer , presentation layer , etc.Session session = (Session ) entityManager .getDelegate ();

Hibernate Objects Life Cycle

Note: In JP A 2.0, you use an EntityManager instead of a Session. So, you will use entityManager .persist(entity) to create(..), entityManager .merge(entity) to
edit(..), entityManager .remove(this.entityManager .merge(entity)) to remove(..), and entityManager .find(entityClass, primaryKey) to find(..),
Detached objects and collections are instances of persistent objects that were associated with a session but currently not associated with a session. These
objects can be freely used as Data T ransfer Objects without having any impact on your database. Detached objects can be later on attached to another session by
calling methods like session.update(), session.saveOrUpdate() etc. and become persistent objects.
Transient objects and collections are instances of persistent objects that were never associated with a session. These objects can be freely used as Data
Transfer Objects without having any impact on your database. T ransient objects become persistent objects when associated to a session by calling methods like
session.save( ), session.persist( ) etc.
Note : The states of transient and detached objects cannot be synchronized with the database as they are not managed by Hibernate.

---

## Q37: What are the general steps involved in creating Hibernate related class?

**Answer:**

The general steps involved in creating Hibernate related classes involve the following steps:
#1. Define the domain (aka entity) objects like Employee, Address, etc to represent relevant tables in the underlying database with the appropriate annotations or
using the *.hbm.xml mapping files.
#2. Define the Repository (aka DAO — Data Access Objects) interfaces and implementations classes that use the domain objects and the hibernate session to
perform data base CRUD (Create, Read, Update and Delete) operations the hibernate way .
#3. Define the service interfaces and the classes that make use of one or more repositories (aka DAOs) in a transactional context.A transaction manager will be
used to coordinate transactions (i.e. commit or rollback) between a number of repositories.
#4. Finally , use an IoC container like Spring framework to wire up the Hibernate classes like SessionFactory , Session, transaction manager , etc and the user
defined repositories, and the service classes. A number of interceptors can be wired up as well for deadlock retry , logging, auditing, etc using Spring.
#5. Favor using JP A and CrudRepository from Spring.
If the interview you are attending require Hibernate knowledge & experience, then go through the following link 30+ Hibernate interview questions & answers.
If you have time, go through the other hibernate interview questions and answers .
Q38-Q40 on Java Persistence API ( JPA) are answered with diagrams at JPA interview questions and answers

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
