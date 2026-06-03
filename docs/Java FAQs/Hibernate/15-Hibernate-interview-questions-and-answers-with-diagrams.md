# 15+ Hibernate interview questions and answers with diagrams

## Table of Contents

- [Q1: What is an Object-to- Relational Mapping (i.e. ORM) tool?](#q1)
- [Q2: Where will you use ORM tools like Hibernate as opposed to using direct Spring JD](#q2)
- [Q3: What are the features & strengths of ORM tools?](#q3)
- [Q4: Can you explain the Hibernate architecture?](#q4)
- [Q5: What is a SessionFactory object? Is it a thread-safe object?](#q5)
- [Q6: What is a Session object? Can you share a session object between dif ferent thre](#q6)
- [Q7: What is the link between EntityManagerFactory/EntityManager and SessionFactory/S](#q7)
- [Q8: Why favor JPA APIs like EntityManagerFactory and EntityManager over Hibernate AP](#q8)

---

## Q1: What is an Object-to- Relational Mapping (i.e. ORM) tool?

**Answer:**

There are several ways to persist data to a database from your Java applications.
1) Dir ect JDBC SQL statements for the CRUD ( Create, Read, Update, and Delete) operations. Direct JDBC is more SQL centric. Y ou need to write all the
SQL statements. This is covered in detail at JDBC Overview Interview Questions and Answers .
2) Persistence frameworks like Spring JDBC T emplate, iBatis, etc to make things easier to use. Spring JDBC T emplate is very popular , and provides an
abstraction over JDBC.
3) ORM frameworks like Hibernate, T opLink from Oracle, etc which are more object centric. Y ou create your domain objects in Java and use Hibernate
annotations to map your objects to underlying database tables and columns, and ORM tool will do a lot of work like generating the CRUD SQL statements,
caching, optimistic locking, etc. These out of the box benefits come at a price of increased complexity . Using ORM tools like Hibernate as a black-box withoutry (
 Connection connection = ds.getConnection ();
 Statement statement = connection .createStatement ();
 ResultSet rs = statement .executeQuery ("SELECT COUNT(*) FROM Customers" )
{
 rs.next();
 final int c = rs.getInt (1);
final int count = new JdbcT emplate (ds).queryForInt ("SELECT COUNT(*) FROM Customers" );

properly understanding its object life cycle, proxying concepts, caching strategies, lazy loading vs eager fetching, etc can not only cause performance issues, but
also lots of frustrations during development.
Spring-data with Hibernate: Spring Data unifies the access to dif ferent kinds of persistence stores, both relational database systems and NoSQL data.

---

## Q2: Where will you use ORM tools like Hibernate as opposed to using direct Spring JDBC T emplate?

**Answer:**

1) If you have complete control of your database schema and if you don’ t have an extremely high throughput requirement then Hibernate can work quite well
when used properly by the developers who know what they are doing. Never use it as a black box framework, which can lead to performance issues and
frustrations during development.
2) ORM is a technique of mapping data representation from an object model to a SQL based relational model, and is well suited for “read, modify and write
centric applications” and not suited for bulk write centric applications like batch processes with lar ge data sets like 5000 rows or more where data is seldom
read. Although this was generally true of many earlier ORM mapping frameworks, most today’ s ORM tools including Hibernate allow for ef ficient ways of
performing lar ge batch style write operations.

---

## Q3: What are the features & strengths of ORM tools?

**Answer:**

ORM tools/frameworks allow your application to be:
– Less verbose (e.g. transparent persistence , Object Oriented query language , transitive persistence, etc)
– More portable (i.e. vendor independence due to multi dialect support ).
– More maintainable (i.e. transparent persistence, inheritance mapping strategies, automatic dirty checking etc).
Takes care of much of the plumbing like
1) Connection establishment & Multi-dialect support.
2) SQL code generation under the covers.
3) Cache management.public interface CustomerRepository extends CrudRepository <Customer , Integer > {
 Long count ();
} 

4) Optimistic locking.
5) Support for auditable fields like CreatedBy , UpdatedBy , CreatedDateT ime, and UpdatedDateT ime.
6) Soft deletes with @SQLDelete annotation.
7 Exception handling.
and you can often focus on the functional aspects. Y ou can also leverage the framework’ s strategies and capabilities to get ef ficiencies via features such as eager
fetching, lazy loading (i.e. using proxy objects), caching strategies, detached objects to be passed to the other layers, dirty checking for optimistic concurrency
control, etc.

---

## Q4: Can you explain the Hibernate architecture?

**Answer:**

The Hibernate architecture is layered to keep you isolated from having to know the underlying APIs. Configuration, SessionFactory , Session, T ransaction,
Query , and Criteria are some of the Hibernate objects.
Hibernate Layered Architecture

---

## Q5: What is a SessionFactory object? Is it a thread-safe object?

**Answer:**

SessionFactory is Hibernate’ s concept of a single datastore and is threadsafe so that many threads can access it concurrently and request for sessions and
immutable cache of compiled mappings for a single database. A SessionFactory is usually only built once at startup. SessionFactory should be wrapped in some
kind of singleton so that it can be easily accessed & reused in an application code.

---

## Q6: What is a Session object? Can you share a session object between dif ferent threads?

**Answer:**

Session is a light weight and a non-threadsafe object. So, you cannot share it between threads. Session represents a single unit-of-work with the database.
Sessions are opened by a SessionFactory and then are closed when all work is complete. Session is the primary interface for the persistence service. A session
obtains a database connection lazily (i.e. only when required). T o avoid creating too many sessions, a ThreadLocal object can be used to have a session per
thread. In JP A 2.0, you use an EntityManager instead of a Session.

---

## Q7: What is the link between EntityManagerFactory/EntityManager and SessionFactory/Session?

**Answer:**

EntityManagerFactory and EntityManager are JPA (java Persistence API) specific. SessionFactory and Session are hibernate -specific.

---

## Q8: Why favor JPA APIs like EntityManagerFactory and EntityManager over Hibernate APIs like SessionFactory & Session?

**Answer:**

1) JPA API is a standard and evolving whereas Hibernate API is proprietary . So, use “javax.persistence.*” annotations as opposed to
“org.hibernate.annotations.*”. Standard APIs in theory has better portability than proprietary APIs. This does not mean that you can easily port your application
to use other ORM tools like T opLink or EclipseLink.
2) JPA API has better semantics than Hibernate as it has rectified some of the the early mistakes of Hibernate API.
3) The EntityManager invokes the hibernate session under the hood, and if you need some specific features that are not available in the EntityManager , you can
obtain the session by calling:
ORSessionFactory sessionFactory = new Configuration ( ).configure ( ).buildSessionfactory ( );
Session session = entityManager .unwrap (Session .class );

4) EntityManagerFactory approach allows us to use callback method annotations like @PrePersist, @PostPersist, and @PreUpdate with no extra configuration.
Using similar callbacks SessionFactory requires extra ef fort.
More Hibernate Interview Questions & Answers
1. 15+ Hibernate interview questions & answers – Q8 – Q15
2. Hibernate mistakes
3. Hibernate cache First & second level interview questions and answers
4. Hibernate automatic dirty checking
5. Hibernate entities with auditable, soft delete & optimistic locking fields
6. Understanding Hibernate proxy objects
7. Hibernate custom data type
8. 8 JPA interview questions and answersSession session = (Session ) entityManager .getDelegate ();

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
