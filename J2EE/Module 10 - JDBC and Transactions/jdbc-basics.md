# JDBC Best Practices

> **Module**: JDBC and Transactions  
> **Topic**: JDBC Best Practices

---

## 📋 Table of Contents



- [Q1: What are JDBC Statements? What are dif ferent types of statements? How can you c](#q1)
- [Q2: What is the difference between statements and prepared statements? Which one wo](#q2)
- [Q3: What design pattern does JDBC use?](#q3)
- [Q4: How will you bootsrap the JDBC driver?](#q4)
- [Q5: How will you create a DataSource in Spring?](#q5)
- [Q6: Why lookup via JNDI for DataSources and other resources like JMS Queues, etc?](#q6)
- [Q7: What are the dif ferent ResultSet types, and what do they determine?](#q7)
- [Q8: What is Cursor Holdability?](#q8)
- [Q9: What is RowSet ? What is the difference between RowSet and ResultSet ? What are](#q9)
- [Q10: What is Metadata and why should you use it?](#q10)
- [Q11: What are database warnings and why do you need them?](#q11)
- [Q12: What is new in JDBC 3.0?](#q12)

---

## Q1: What are JDBC Statements? What are dif ferent types of statements? How can you create them?

**Answer:**

A statement object is responsible for sending the SQL statements to the Database. Statement objects are created from the connection object and then
executed.
The types of statements are:
Statement (regular statement as shown above).
PreparedStatement (more ef ficient than statement due to pre-compilation of SQL).
CallableStatement (to call stored procedures on the database).
To use prepar ed statement :
Callable statements are used for calling stored procedures.Statement stmt = myConnection .createStatement ();
ResultSet rs = stmt.executeQuery (“SELECT id, name FROM myTable where id =1245 ”); //to read
 or
stmt.executeUpdate (“INSER T INTO (field1 ,field2 ) values (1,3)”); //to insert
PreparedStatement prepStmt = myConnection .prepareStatement ("SELECT id, name FROM myT able where id = ? " );
prepStmt .setInt (1, 1245 );
CallableStatement calStmt = myConnection .prepareCall ("{call PROC_SHOWMYBOOKS}" );

---

## Q2: What is the difference between statements and prepared statements? Which one would you favor and why?

**Answer:**

#1. Pr epar ed statements offer better performance, as they are pre-compiled. Prepared statements reuse the same execution plan for dif ferent arguments rather
than creating a new execution plan every time. Prepared statements use bind arguments, which are sent to the database engine. This allows mapping dif ferent
requests with same prepared statement but dif ferent arguments to execute the same execution plan.
#2. Pr epar ed statements ar e mor e secur ed because they use bind variables, which can prevent SQL injection attack.
The most common type of SQL injection attack is SQL manipulation. The attacker attempts to modify the SQL statement by adding elements to the WHERE
clause or extending the SQL with the set operators like UNION, INTERSECT etc.
The attacker can manipulate the SQL as follows
The above “WHERE” clause is always true because of the operator precedence. The PreparedStatement can prevent this by using bind variables:ResultSet rs = cs.executeQuery ();
SELECT * FROM users
 WHERE username =’bob’
 AND accountid =1234 ;
SELECT * FROM users
 WHERE username =’bob’
 AND accountid =1234 OR ‘a’ = ‘a’ ;

---

## Q3: What design pattern does JDBC use?

**Answer:**

Adapter design pattern. An adapter adapts at run time like the decorator design pattern. Adapter design pattern is one of the structural design patterns and
its intent is to get two unrelated interfaces work together. Think of using a laptop in UK that was bought in Japan as the sockets are dif ferent, and you need an
adapter. So, the adapter ’s intent is to adapt between the Japanese laptop plug with UKs wall socket. The key point is that parties are dif ferent. Japanese laptop
used in third-party or external (i.e. UK) wall socket.
JDBC – Adapter Design Pattern
Adapter is used when you have an abstract interface, for example a JDBC API and you want to map that interface to another object which has similar functional
role, but a dif ferent interface, for example dif ferent JDBC drivers for dif ferent databases like Oracle, Sybase, DB2, SQL server, MySQL, etc. The JEE haveString strSQL = SELECT * FROM users where username =? AND accountid =?);
PreparedStatement pstmt = myConnection .prepareStatement (strSQL );
pstmt .setString (1,”bob”);
pstmt .setLong (2, 1234 );
pstmt .execute ();

multiple adaptors for JMS, JNDI, JDBC, JCA, etc. The drivers and implementations are generally provided by the third party vendors. For example, JMS
implementations provided by third-party vendors and open source providers web Methods, IBM MQ Series, ActiveMQ, etc.

---

## Q4: How will you bootsrap the JDBC driver?

**Answer:**

#1. Dynamic class loading with Class.forName(….) will load the driver and register it with the DriverManager. The driver jar file supplied by the Oracle
database vendor or MySQL vendor must be in the classpath. “oracle.jdbc.driver .OracleDriver” is class in the driver jar file.
#2. The DataSour ce interface provides an alternative to the DriverManager for making a connection. DataSource makes the code more portable than
DriverManager because it works with JNDI and it is created, deployed and managed separately from the application that uses it. If the DataSource location
changes, then there is no need to change the code but change the configuration properties in the server. This makes your application code easier to maintain.
DataSource allows the use of connection pooling and support for distributed transactions.
A DataSource is configured on the application server with the following properties
DataSource config
Once the DataSource has been set up, then you can get the connection object as follows:Class .forName (“oracle .jdbc.driver .OracleDriver ”); //dynamic class loading
String url = jdbc:oracle :thin:@hostname :1526 :myDB ;
Connection myConnection = DriverManager .getConnection (url, “username ”, “password ”);

---

## Q5: How will you create a DataSource in Spring?

**Answer:**

Using Apache commons-dbcp package that has the org.apache.commons.dbcp.BasicDataSource class. The pom.xml file for maven should declare the
dependency .
Configure the application server specific datasource file. For example, in JBoss application server configure my-ds.xml with JNDI.Context ctx = new InitialContext ();
DataSource ds = (DataSource )ctx.lookup ("jdbc/myDataSource" );
Connection myConnection = ds.getConnection (“username ”,”password ”);
<properties >
 <commons -dbcp .version >1.4</commons -dbcp .version >
</properties >
<dependencies >
<dependency >
<groupId >commons -dbcp </groupId >
<artifactId >commons -dbcp </artifactId >
<version >${commons -dbcp .version }</version >
</dependency >
</dependencies >
<datasources >
 <local -tx-datasource >
 <jndi-name >jdbc.dataSource .my_jndi </jndi-name >
 <use-java-context >false </use-java-context >
 <connection -url>jdbc:sybase :Tds:my-server :20345 /my_schema </connection -url>

Finally, the Spring configuration to use the JNDI name

---

## Q6: Why lookup via JNDI for DataSources and other resources like JMS Queues, etc?

**Answer:**

JNDI based creation allows you to move an application between environments like development to UA T and then to integration and finally to production. If
you configure each application server to use the same JNDI name, you can have dif ferent databases in each environment and not required to change your code.
You just pick up the same environment free W AR file and drop it in any environment. In other words, the environment details are externalized.

---

## Q7: What are the dif ferent ResultSet types, and what do they determine?

**Answer:**

The ResultSet types determine the ways in which the cursor can be manipulated, and how concurrent changes made to the underlying data source are
reflected by the ResultSet object.
The cursor manipulation can be determined by one of three differ ent ResultSet types: <driver -class >com.sybase .jdbc3 .jdbc.SybDriver </driver -class >
 <user-name >user</user-name >
 <password >password </password >
 <max-pool-size>50</max-pool-size>
 <exception -sorter -class -name >org.jboss .resource .adapter .jdbc.vendor .SybaseExceptionSorter </exception -sorter -class -name >
 <new-connection -sql>select count (1) from my_table </new-connection -sql>
 <check -valid -connection -sql>select count (1) from my_table </check -valid -connection -sql>
 </local -tx-datasource >
</datasources >
<bean id="datasource_abc" class ="org.springframework.jndi.JndiObjectFactoryBean" scope ="singleton" >
<property name ="jndiName" >
 <value >jdbc.dataSource .my_jndi </value >
 </property >
</bean >
<bean id="jdbcT emplate_abc" class ="org.springframework.jdbc.core.JdbcT emplate" >
 <property name ="dataSource" ref="datasource_abc" />
</bean >

TYPE_FOR WARD_ONL Y: The result set cannot be scrolled; its cursor moves forward only, from before the first row to after the last row. The rows contained
in the result set depend on how the underlying database generates the results. That is, it contains the rows that satisfy the query at either the time the query is
executed or as the rows are retrieved.
TYPE_SCROLL_INSENSITIVE: The result can be scrolled; its cursor can move both forward and backward relative to the current position, and it can move
to an absolute position. The result set is insensitive to changes made to the underlying data source while it is open. It contains the rows that satisfy the query at
either the time the query is executed or as the rows are retrieved.
TYPE_SCROLL_SENSITIVE: The result can be scrolled; its cursor can move both forward and backward relative to the current position, and it can move to
an absolute position. The result set reflects changes made to the underlying data source while the result set remains open.
The default ResultSet type is TYPE_FOR WARD_ONL Y. Generally, TYPE_SCROLL_INSENSITIVE is the preferred option. The data contained in the
ResultSet object is fixed (a snapshot) when the object is created.
ResultSet Concurr ency: The concurrency of a ResultSet object determines what level of update functionality is supported .
There are two concurrency levels:
CONCUR_READ_ONL Y: The ResultSet object cannot be updated using the ResultSet interface.
CONCUR_UPDA TABLE: The ResultSet object can be updated using the ResultSet interface.
The default ResultSet concurrency is CONCUR_READ_ONL Y.
ResultSet rs;
Connection con = null;
public void fetchResultSet ()
{
 try {
 if(con==null || con.isClosed ())
 {
 Class .forName ("sun.jdbc.odbc.JdbcOdbc" );
 con = DriverManager .getConnection ("jdbc:odbc:MyDB" ,"user" ,"password" ); 
 }
 Statement stmt = con.createStatement (ResultSet .TYPE_SCROLL_INSENSITIVE ,ResultSet .CONCUR_READ_ONL Y);
 String query = "select * from Stocktbl" ;
 rs = stmt.executeQuery (query );

---

## Q8: What is Cursor Holdability?

**Answer:**

The cursor holdability feature was added in JDBC 3.0. Calling the method con.commit can close the ResultSet objects that have been created during the
current transaction. In some cases, this behavior is not desired. The ResultSet property holdability gives the application control over whether ResultSet objects
(cursors) are closed when commit is called. The following ResultSet constants may be supplied to the Connection methods createStatement, prepareStatement,
and prepareCall:
HOLD_CURSORS_OVER_COMMIT : ResultSet cursors are not closed; they are holdable: they are held open when the method commit is called. Holdable
cursors might be ideal if your application uses mostly read-only ResultSet objects.
CLOSE_CURSORS_A T_COMMIT : ResultSet objects (cursors) are closed when the commit method is called. Closing cursors when this method is called can
result in better performance for some applications.
The default cursor holdability varies depending on your DBMS.
Note: Not all JDBC drivers and databases support holdable and non-holdable cursors. The following method, JDBCT utorialUtilities.cursorHoldabilitySupport,
outputs the default cursor holdability of ResultSet objects and whether HOLD_CURSORS_OVER_COMMIT and CLOSE_CURSORS_A T_COMMIT are
supported

---

## Q9: What is RowSet ? What is the difference between RowSet and ResultSet ? What are the advantages of using RowSet over ResultSet?

**Answer:**

RowSets are a JDBC 2.0 extension to the java.sql.ResultSet interface. Guess what, it makes life a lot easier for all JDBC programmers. No more
Connection objects, statement objects, just a single RowSet will do everything for you. RowSet object follows the JavaBeans model for properties and event
notification, it is a JavaBeans component that can be combined with other components in an application.
The ResultSet has an ‘open connection’ to the database whereas a RowSet works in a ‘disconnected’ fashion. It has the following advantages over a ResultSet. }catch (Exception ex)
 {
 System.out.println (ex);
 Logger .getLogger (StockScr .class .getName ()).log(Level .SEVERE, null, ex);
 try
 {
 if(con != null)
 {
 con.close ();
 }
 }catch (Exception x){}
 }

— Since a RowSet works in a disconnected mode, especially for “read-only” queries, it would have better performance in a highly concurrent system.
— Rowsets have many dif ferent implementations to fill dif ferent needs. These implementations fall into two broad categories, rowsets that are connected and
those that are disconnected.
— Rowsets make it easy to send tabular data over a network. They can also be used to provide scrollable result sets or updatable result sets in special cases
when the underlying JDBC driver does not support them.
RowSet disadvantages.
— Rowset keeps all the data from the query result in memory. This is very in-ef ficient for queries that return huge data.
There are 3 types of RowSets.
JdbcRowSet is a connected type of rowset as it maintains a connection to the data source using a JDBC driver .
CachedRowSet and WebRowSet are disconnected types of rowsets as they are connected to the data source only when reading data from it or writing data to it.JdbcRowSet jdbcRowSet = new JdbcRowSetImpl ();
dbcRowSet .setCommand ("SELECT * FROM Course);
dbcRowSet.setURL(" jdbc:hsqldb :hsql://localhost/mytestdb");
dbcRowSet .setUsername ("sa");
dbcRowSet .setPassword ("pwd" );
dbcRowSet .execute ();
ResultSet rs = stmt.executeQuery ("SELECT * FROM Course" );
CachedRowSet crset = new CachedRowSetImpl ();
crset .populate (rs);

---

## Q10: What is Metadata and why should you use it?

**Answer:**

JDBC API has 2 Metadata interfaces — DatabaseMetaData & ResultSetMetaData. The meta data means data about data, and provides comprehensive
information about the database as a whole. The implementation for this interface is implemented by database driver vendors to let users know the capabilities of
a Database.

---

## Q11: What are database warnings and why do you need them?

**Answer:**

Warnings are issued by a database to inform user of a problem which may not be very severe. Database warnings do not stop the execution of SQL
statements. W arnings are silently chained to the object. Y ou need warnings for the reporting purpose. W arnings may be retrieved from Connection, Statement,
and ResultSet objects.WebRowSet wrs = new WebRowSetImpl ();
wrs.populate (rs);
wrs.absolute (2)
wrs.updateString (1, "JNDI" );
ResultSet rs = stmt.executeQuery ("SELECT a, b, c FROM T ABLE2" );
ResultSetMetaData resultSetMeta = rs.getMetaData ();
nt numberOfColumns = resultSetMeta .getColumnCount ();
boolean b = resultSetMeta .isSearchable (3);
SQLWarning warning = conn .getW arnings ();
QLWarning nextW arning = warning .getNextW arning (); 
conn .clearW arnings ();
..
tmt.getW arnings ();
tmt.clearW arnings ();

---

## Q12: What is new in JDBC 3.0?

**Answer:**

#1.Savepoint support, which allows you to define, release, and rollback a transaction to a savepoint. T raditionally, database transactions have been “all or
nothing” types of events — start a transaction, insert some rows, do some updates, and then either commit or rollback. With JDBC 3.0, the transactional model
is now more flexible. An application might start a transaction, insert several rows and then create a savepoint. This savepoint serves as a bookmark. The
application might then perform some if/then/else type of logic such as updating a group of rows. The application might conclude that the updates made were a
bad idea but the initial inserts were okay. The application can rollback to the the bookmark (i.e. savepoint), and then commit the group of inserts as if the
updates have never been attempted.
#2. Ability to have multiple open ResultSet objects. JDBC 3.0 gives the programmer the flexibility to decide if he/she wants concurrent access to result sets
generated from procedures or if he/she wants the resources to be closed when a new result set is retrieved, which is the JDBC 2.0 compliant behavior .
#3. Ability to contr ol how pr epar ed statements ar e pooled and reused by connections with deployment configurations.
#4. Ability to pass parameters to CallableStatement objects by name .
#5.Ability to r etrieve auto generatet keys. Many databases have hidden columns (aka pseudo columns) that represent a unique key over every row in a table.
For example, Oracle and Informix have ROWID pseudo columns. An optional feature of the JDBC 3.0 specification is the ability to retrieve auto generated key
information for a row when the row is inserted into a table.
#6. JDBC 3.0 intr oduces a standard mechanism for updating BLOB and CLOB data. Even though JDBC 2.0 provided mechanisms to read BLOB and
CLOB data, but it lacked an update capability for those types.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »..
s.getW arnings ();
..

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03