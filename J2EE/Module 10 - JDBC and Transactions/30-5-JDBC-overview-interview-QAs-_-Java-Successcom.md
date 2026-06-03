# 30 5 JDBC overview interview Q&As   Java Success.com

## Table of Contents

- [Q1: What is JDBC? How do you connect to a database?](#q1)
- [Q2: What is a datasour ce?](#q2)
- [Q3: Why should you prefer using a DataSource?](#q3)
- [Q4: Have you used a Data Access Object (DAO) pattern? Why is it a best practice to u](#q4)
- [Q5: What are the best practices relating to exception handling to make your DAOs mor](#q5)

---

## Q1: What is JDBC? How do you connect to a database?

**Answer:**

JDBC stands for Java DataBase Connectivity . It is an API which provides easy connection to a wide range of databases. T o connect to a database we need
to load the appropriate driver and then request for a connection object. The Class.forName(….) will load the driver and register it with the DriverManager .
The driver jar file (e.g. ojdbc14.jar) needs to be in the classpath.

---

## Q2: What is a datasour ce?

**Answer:**

The DataSour ce interface provides an alternative to the DriverManager for making a connection. DataSour ce makes the code more portable than the 
DriverManager because it works with JNDI and it is created, deployed and managed separately from the application that uses it. If the DataSour ce location
changes, then there is no need to change the code, but change the configuration properties in the server . This makes your application code easier to maintain.
DataSour ce allows the use of connection pooling and support for distributed transactions. A DataSour ce is not only a database but also can be a file or a
spreadsheet. A DataSour ce object can be bound to JNDI and an application can retrieve and use it to make a connection to a database. JEE application servers
provide tools to define your DataSour ce with a JNDI name. When the server starts it loads all the DataSour ces into the application server ’s JNDI service.
DataSource configuration properties are shown below:
JNDI Name = jdbc/myDataSource
URL = jdbc:oracle:thin:@hostname:1526:myDB
UserName, Password 
Implementation class name = oracle.jdbc.pool.OracleConnectionPoolDataSource
Jar file in the Classpath = ojdbc14.jar
Connection pooling settings like = minimum pool size, maximum pool size, connection timeout, statement cache size etc.
Once the DataSource has been set up, then you can get the connection object as follows:Class .forName (“oracle .jdbc.driver .OracleDriver ”); //dynamic class loading
String url = jdbc:oracle :thin:@hostname :1526 :myDB ;
Connection myConnection = DriverManager .getConnection (url, “username ”, “password ”);

---

## Q3: Why should you prefer using a DataSource?

**Answer:**

Best practice : In a very basic application a Connection obtained from a DataSource and a DriverManager are identical. But, the JEE best practice is to use
DataSource because of its portability , better performance due to pooling of valuable resources, and the JEE standard requires that applications use the
container ’s resource management facilities to obtain connections to resources. Every major web application container provides pooled database connection
management as part of its resource management framework.
Design Pattern : JDBC architecture decouples an abstraction from its implementation so that the implementation can vary independent of the abstraction. This is
an example of the bridge design pattern. The JDBC API provides the abstraction and the JDBC drivers provide the implementation. New drivers can be
plugged-in to the JDBC API without changing the client code.

---

## Q4: Have you used a Data Access Object (DAO) pattern? Why is it a best practice to use a DAO pattern

**Answer:**

A DAO class provides access to a particular data resource in the resour ce tier or data tier( e.g. relational database, XML, mainframe, etc) without coupling
the resource’ s API to the business logic in the middle tier . A tier is a physical machine, whereas a layer is logical.Context ctx = new InitialContext (); //JNDI context
DataSource ds = (DataSource )ctx.lookup ("jdbc/myDataSource" );
Connection myConnection = ds.getConnection (“username ”,”password ”);

jdbc overview
For example, you may have a EmployeeServiceImpl in the business logic layer with business logic, and uses EmployeeDAO in the data access layer for
accessing data in the data tier . EmployeeServiceImpl uses the interface EmployeeDAO as opposed to the implementation. This is the best practice of “coding to
interface not implementation” . If your data resource change from a database to a Mainframe system, then reimplementing EmployeeDAO for a dif ferent data
access mechanism (to use a mainframe Connector) would have little or no impact on any classes like EmployeeServiceImpl that uses EmployeeDAO . 
EmployeeDAOImpl can even decide to use hibernate framework instead of using the JDBC directly . The EmployeeServiceImpl will not be impacted as long as
the contract in EmployeeDAO is met.

Hibernate with jdbc overview
Inversion of control (IoC) frameworks like Spring framework promotes the design principle of “code to interface not implementation”.

---

## Q5: What are the best practices relating to exception handling to make your DAOs more robust and maintainable?

**Answer:**

If you catch an exception in your DAO code, never ignore it or swallow it because ignored exceptions are hard to troubleshoot. DAO class methods
should throw checked exceptions only if the caller can reasonably recover from the exception or reasonably handle it (e.g. retry operations in optimistic
concurrency control). If the caller cannot handle the exception in a meaningful way , consider throwing a runtime (i.e. unchecked) exception. For example,
Hibernate 3 exceptions are all runtime exceptions. 
DAO methods should not throw low level JDBC exceptions like java.sql.SQLException . A DAO should encapsulate JDBC rather than expose it to rest of
the application. Use chained exceptions to translate low-level exceptions into high-level checked exceptions or runtime exceptions. DAO methods should
not throw java.lang.Exception because it is too generic and does not convey any underlying problem. 
Log your exceptions, configuration information, query parameters, etc.

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
