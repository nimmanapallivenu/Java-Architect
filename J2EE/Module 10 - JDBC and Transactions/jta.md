# Java Transaction API (JTA)

> **Module**: JDBC and Transactions  
> **Topic**: Java Transaction API (JTA)

---

## 📋 Table of Contents



- [Q1: What is a T ransaction? What does setAutoCommit do?](#q1)
- [Q2: What is transaction demarcation? What are the dif ferent ways of defining transa](#q2)
- [Q3: What support does Spring framework have for transaction management?](#q3)
- [Q4: What is a distributed (aka JT A/XA) transaction? How does it dif fer from a loca](#q4)
- [Q5: What is two-phase commit?](#q5)
- [Q6: What do you understand by JT A and JTS?](#q6)
- [Q7: What is a XA resource?](#q7)
- [Q8: Why JT A transactions are more powerful than JDBC transactions?](#q8)

---

## Q1: What is a T ransaction? What does setAutoCommit do?

**Answer:**

A transaction is a set of operations that should be completed as a unit. If one operation fails then all the other operations fail as well. For example if you
transfer funds between two accounts there will be two operations in the set
1. Withdraw money from one account.
2. Deposit money into other account.
These two operations should be completed as a single unit. Otherwise your money will get lost if the withdrawal is successful and the deposit fails. There are
four characteristics (ACID properties) for a T ransaction.
ACID properties
Transactions maintain data integrity. A transaction has a beginning and an end like everything else in life. The setAutocommit(….), commit( ) and rollback( )
are used for marking the transactions (known as transaction demarcation). When a connection is created, it is in auto-commit mode. This means that each
individual SQL statement is treated as a transaction and will be automatically committed immediately after it is executed. The way to allow two or more
statements to be grouped into a transaction is to disable auto-commit mode:
ry{
 Connection myConnection = dataSource .getConnection ();
 // set autoCommit to false
 myConnection .setAutoCommit (false );
 withdrawMoneyFromFirstAccount (.............); //operation 1
 depositMoneyIntoSecondAccount (.............); //operation 2
 myConnection .commit ();

The above code ensures that both operation 1 and operation 2 succeed or fail as an atomic unit and consequently leaves the database in a consistent state. Also,
turning auto-commit of f will provide better performance.

---

## Q2: What is transaction demarcation? What are the dif ferent ways of defining transactional boundaries?

**Answer:**

Data Access Objects (DAO) are transactional objects. Each operation associated with CRUD operations like Create, Update and/or Delete operations
should be associated with transactions. T ransaction demarcation is the manner in which transaction boundaries are defined. There are two approaches for
transaction demarcation.
There are 2 types of transaction demarcation
1. Declarative transaction demarcation
2. Programmatic transaction demarcation
1. Declarative transaction demar cation:
The programmer declaratively specifies the transaction boundaries using transaction attributes for an EJB via ejb-jar .xml deployment descriptor .
Note: Spring framework has support for declarative transaction demarcation by specifying transaction attributes via Spring config files. If you choose Spring
framework to mark the transaction boundaries, then you need to turn of f transaction demarcation in your EJB by marking it as NotSupported.
Q. How are these declarative transactions know when to rollback?
EJBs: When the EJB container manages the transaction, it is automatically rolled back when a System Exception occurs. This is possible because the container
can intercept “System Exception”. However, when an “Application Exception” occurs, the container does not intercept it and therefore leaves it to the code to
roll back using ctx.setRollbackOnly() method.catch (Exception sqle){
try{
 myConnection .rollback ();
}catch ( Exception e){}
finally {
 try{
 if( conn != null) {
 conn .close ();
 }
} catch ( Exception e) {}

Spring Framework: Provided @T ransactional annotation in the service layer .
2. Programmatic transaction demar cation:
The programmer is responsible for coding transaction logic as shown above. The application controls the transaction via an API like JDBC API, JT A API,
Hibernate API etc. JDBC transactions are controlled using the java.sql.Connection object. There are two modes: auto-commit and manual commit. Following
methods are provided in the JDBC API via non-XA java.sql.Connection class for programmatically controlling transactions:
For XA-Connections use the following methods on javax.transaction.UserT ransaction.

---

## Q3: What support does Spring framework have for transaction management?

**Answer:**

1. Spring supports both global transactions across multiple transactional resources through JT A and resouce specific local transaction associated with a JDBC
connection. It provides a consistent programming model to support both local and global transactions. Y ou write your code once, and it can benefit from
different transaction management strategies in dif ferent environmen.public void setAutoCommit (boolean mode );
public boolean getAutoCommit ();
public void commit ();
public void rollback ();
public void begin ();
public void commit ();
public void rollback ();
public int getStatus ();
public void setRollbackOnly ();
public void setTransactionT imeOut (int)

2. Supports both declarative transaction management via AOP and programmatic transaction management with the Spring Framework transaction abstraction
which can run over any underlying transaction infrastructure. With the preferred declarative model, developers typically write little or no code related to
transaction management.

---

## Q4: What is a distributed (aka JT A/XA) transaction? How does it dif fer from a local transaction?

**Answer:**

There are two types of transactions:
Local transaction: Transaction is within the same database. As we have seen above, with JDBC transaction demarcation, you can combine multiple SQL
statements into a single transaction, but the transactional scope is limited to a single database connection. A JDBC transaction cannot span multiple databases.
Distributed T ransaction (aka Global T ransaction, JT A/XA transaction ): The transactions that constitute a distributed transaction might be in the same
database, but more typically are in dif ferent databases and often in dif ferent locations. For example, A distributed transaction might consist of money being
transferred from an account in one bank to an account in another bank. Y ou would not want either transaction committed without assurance that both will
complete successfully. The Java T ransaction API (JT A) and its sibling Java T ransaction Service (JTS), provide distributed transaction services for the JEE
platform. A distributed transaction (aka JT A/XA transaction) involves a transaction manager and one or more resource managers. A resource manager
represents any kind of data store. The transaction manager is responsible for coordinating communication between your application and all the resource
managers. A transaction manager decides whether to commit or rollback at the end of the transaction in a distributed system. A resource manager is responsible
for controlling of accessing the common resources in the distributed system.

---

## Q5: What is two-phase commit?

**Answer:**

two-phase commit is an approach for committing a distributed transaction in 2 phases.

---

## Q6: What do you understand by JT A and JTS?

**Answer:**

JTA is a high level transaction interface which allows transaction demarcation in a manner that is independent of the transaction manager implementation.
JTS specifies the implementation of a T ransaction Manager which supports the JT A. The code developed by developers does not call the JTS methods directly ,
but only invokes the JT A methods. The JT A internally invokes the JTS routines.

---

## Q7: What is a XA resource?

**Answer:**

The XA specification defines how an application program uses a transaction manager to coordinate distributed transactions across multiple resource
managers. Any resource manager that adheres to XA specification can participate in a transaction coordinated by an XA-compliant transaction manager .

JTA XA T ransaction
JTA transaction demarcation requires a JDBC driver that implements XA interfaces like javax.sql.XADatasource, javax.sql.XAConnection and
javax.sql.XAResource. A driver that implements these interfaces will be able to participate in JT A transactions. Y ou will also require to set up the
XADatasource using your application server specific configuration files, but once you get a handle on the DataSource via JNDI lookup, you can get a XA
connection via javax.sql.DataSource.getConnection() in a similar manner you get a non-XA connections. XA connections are dif ferent from non-XA
connections and do not support JDBC’ s auto-commit feature. Y ou cannot also use the commit( ), rollback( ) methods on the java.sql.Connection class for the
XA connections. A J2EE component can begin a transaction programmatically using javax.transaction.UserT ransaction interface or it can also be started
declaratively by the EJB container if an EJB bean uses container managed transaction. For explicit (i.e. programmatic) JT A/XA transaction you should use the
UserT ransaction.begin( ), UserT ransaction.commit() and UserT ransaction.rollback() methods. For example:
/ programmatic JT A transaction
nitialContext ctx = new InitialContext ();
UserT ransaction utx = (UserT ransaction )ctx.lookup (“java:comp /UserT ransaction ”);
ry {
 //…
 utx.begin ();
 //….

---

## Q8: Why JT A transactions are more powerful than JDBC transactions?

**Answer:**

JTA transactions are more powerful than JDBC transactions because a JDBC transaction is limited to a single database whereas a JT A transaction can have
multiple participants like:
— JDBC connections.
— JMS queues/topics.
— Enterprise JavaBeans (EJBs).
— Resource adapters that comply with JEE Connector Architecture (JCA) specification.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » DataSource ds = getXADatasource ();
 Connection con = ds.getConnection (); // get a XAconnection.
 PreparedStatement pstmt = con.prepareStatement (“UPDA TE Employee emp where emp.id =?”);
 pstmt .setInt (1, 12456 );
 pstmt .executeUpdate ();
 
 utx.commit ();//transaction manager uses two-phase commit protocol to end transaction 
catch (SQLException sqle){
 utx.rollback ();
 throw new RuntimeException (sqle);

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03