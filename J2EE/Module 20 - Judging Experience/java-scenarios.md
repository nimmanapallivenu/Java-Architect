# Java Scenario-Based Questions

> **Module**: Judging Experience  
> **Topic**: Java Scenario-Based Questions

---

## 📋 Table of Contents



- [Q1: Scenario : You need to load stock exchange security codes from a database and ca](#q1)
- [Q2: Scenario : If you have a requirement to generate reports or feed files by pullin](#q2)
- [Q3: Scenario : You need to find and change a text from “Client” to “Customer” in 300](#q3)
- [Q4: Scenario : You are asked to design an application, which validates data with 100](#q4)
- [Q5: Scenario : Reference counting where a shared resource is incremented or decremen](#q5)
- [Q6: Scenario : If you are working in an online trading application, you may want the](#q6)
- [Q7: Scenario : You are required to change the logic of a module that many other modu](#q7)
- [Q8: Scenario : You have a number of applications like cash, mortgages, investments, ](#q8)
- [Q9: Scenario : You have a requirement to maintain a history of insertion, modificati](#q9)
- [Q10: Scenario : A main thread creates 3 database connections and assigns each of thos](#q10)
- [Q11: Scenario : What if the above scenario has a specific requirement where the 3 chi](#q11)
- [Q12: Scenario : Looping through a list of items and removing an item in the list coul](#q12)
- [Q13: Scenario : Two users try to modify the same record in the database. In this scen](#q13)
- [Q14: Scenario : How will you go about identifying duplicate records in a table?](#q14)
- [Q15: Scenario : How would you go about deleting the duplicate records?](#q15)
- [Q16: Scenario : How will you go about writing an SQL query for the following scenario](#q16)
- [Q17: Scenario : How will you extract rule_name values from a tabular data shown below](#q17)
- [Q18: Scenario : How will you determine in your Java application if it is running in p](#q18)

---

## Q1: Scenario : You need to load stock exchange security codes from a database and cache them for performance. The security codes need to be refreshed say every
30 minutes. This cached data needs to be populated and refreshed by a single writer thread and read by several reader threads. How will you ensure that your
read/write solution is scalable and thread safe?

**Answer:**

Solution :
Option 1 : The java.util.concurrent.locks package provides classes that implement read/write locks where the read lock can be executed in parallel by multiple
threads and the write lock can be held by only a single thread. The ReadW riteLock interface maintains a pair of associated locks, one for read-only and one for
writing. The readLock( ) may be held simultaneously by multiple reader threads, while the writeLock( ) is exclusive. In general, this implementation improves
performance and scalability when compared to the mutex locks (i.e. via synchronized key word) when
1. there are more reads and read duration compared writes and write duration.
2. It also depends on the system you are running on — for example multi-core processors for better parallelism.
The Concurr entHashmap is another example where improved performance can be achieved when you have more reads than writes. The ConcurrentHashmap
allows concurrent reads and locks only the buckets that are used to modify or insert data.
Option 2 : Making use of caching frameworks like EHCache, OSCache, etc. Caching frameworks take care of better memory management with LRU(Least
Recently Used) and FIFO(First In First Out) eviction strategies, disk overflow, data expiration and many other optional advanced features, as opposed to writing
your own. Having said this, there are cases where you might want to write your own simple caching solution.

---

## Q2: Scenario : If you have a requirement to generate reports or feed files by pulling out millions of records from a database, what questions will you ask, and how
will you go about designing it?

**Answer:**

Questions you need to ask :
— Should we restrict the online reports for only last 12 months of data to minimize the report size and get better performance, and provide report/feed for data
older than 12 months via of fline processing?
— Should we generate the reports asynchronously? Reports can be generated asynchronously and once ready can be emailed or downloaded via W eb at a later
time.
— What report generation framework to use like Jasper Reports, Open CSV, XSL-FO with Apache FOP, etc depending on the required output formats?
— How to handle exceptional scenarios? — send an error email, use a monitoring system like Tivoli or Nagios to raise production support tickets, etc?
— Security requirements. Are we sending feed/report with sensitive data via email? Do we need proper access control to restrict who can generate what for inline
reports? Sensitive report information needs to be encrypted.

— Should we schedule the of fline reports to run during of f peak?
— Archival and pur ging of the older reports. What is the report retention period for the requirements relating to auditing and compliance purpose? How big are the
feed files and should they be gzipped?
Solutions : An online application with a requirement to produce time consuming reports or a business process (e.g. re-balancing accounts, aggregating hierarchical
information, etc) could benefit from making these long running operations asynchronous. Once the reports or the long running business process is completed, the
outcome can be communicated to the user via emails or asynchronously refreshing the web page via techniques known as “ server push (JEE Async Servlet)” or
“client pull (Refresh meta tag)”. A typical example would be
a) A user makes a request for an aggregate report or a business process like re-balancing his/her portfolios.
b) The user input can be saved to a database table for a separate process to periodically pick it up and process it asynchronously .
c) The user could now continue to perform other functionality of the website without being blocked.
d) A separate process running on the same machine or dif ferent machine can periodically scan the table for any entries and produce the necessary reports or
execute the relevant business process. This could be a scheduled job that runs once during of f-peak or every 10 minutes. This depends on the business requirement.
e) Once the report or the process is completed, notify the user via emails or making the report available online to be downloaded.
Apache Camel can be used to create an asynchronous route. The high-level diagram of a possible solution using the Apache Camel. This framework is written to
address the Enterprise Integration Patterns (i.e. EIP).

Apache Camel Routes

---

## Q3: Scenario : You need to find and change a text from “Client” to “Customer” in 300+ html files.

**Answer:**

Solution : Harness the power of Unix.

---

## Q4: Scenario : You are asked to design an application, which validates data with 100+ rules to comply with the government requirements and tax laws. These
compliance requirements can change and the application need to quickly and easily adapt to changing requirements.

**Answer:**

Solution : Harness the power of Rules Engines like Drools. Drools is a popular open source business rules and work flow engine. It helps you externalize the
rules in database tables and excel spreadsheets as opposed to embedding within the Java code. The rules are executed in the form of when given a($condition) then
execute the ($consequence). The business will be the custodian of these rules that can be easily viewed on an excel spreadsheet or via querying the database tables.
A GUI could be built to maintain these rules that reside in a database.

---

## Q5: Scenario : Reference counting where a shared resource is incremented or decremented. The increment/decrement/test operations must be thread safe. For
example, a counter that keeps track of the number of active logged in users by incrementing the count when users log in and decrementing the count when the
users log out. Sometimes you want to allow a finite number of concurrent accesses say 3.

**Answer:**

Solution :
Mutex : is a single key to a toilet. One person can have the key and occupy the toilet at the time. When finished, the person gives (or releases) the key to the next
person in the queue. In Java, every object has a mutex and only a single thread can get hold of a mutex.
Semaphor e: Is a number of free identical toilet keys. For example, having 3 toilets with identical locks and keys. The semaphore count is set to 3 at beginning and
then the count is decremented as people are acquiring the key to the toilets. If all toilets are full, i.e. there are no free keys left, the semaphore count is 0. Now ,
when one person leaves the toilet, semaphore is increased to 1 (one free key), and given to the next person in the queue.

---

## Q6: Scenario : If you are working in an online trading application, you may want the functionality to queue trades and process them when the stock market opens.
You also asynchronously receive the order statuses like partially-filled, rejected, fully filled, etc from the stock market.

**Answer:**

Solution : The message oriented middle-wares provide features like guaranteed delivery with store-and-forward mechanism, no duplicates, and transaction
management for enterprise level program-to-program communications by sending and receiving messages asynchronously (or synchronously). The diagram below
gives a big picture.#!/bin/sh
 
for file in $(grep -il "Client" *.html)
do
sed -e "s/Client/Customer/ig" $file > /tmp/tempfile .tmp
mv /tmp/tempfile .tmp $file
done

When using Message Oriented Middle-wares (MOM) to facilitate asynchronous processing
1) The producer (i.e T rading Engine) that submits user requests and consumer (i.e. FIX Router) that converts the messages to FIX protocol and send FIX messages
to the Stock Exchange system retain processing control and do not block. In other words, they continue processing regardless of the state of others. Queue depths
need to be properly set, and the messages need to be durable. Message correlation ids are used to pair request and response.

2) MOM creates looser coupling among systems, provides delivery guarantees, prevents message losses, scales well by decoupling performance characteristics of
each system, has high availability and does not require same time availability of all sub-systems. So, MOM is ideal for geographically dispersed systems requiring
flexibility, scalability, and reliability .
3) You may also require to perform logging, auditing and performance metrics gathering asynchronously and non-intrusively. For example, you could send the log
messages from log4j to a queue to be processed later asynchronously by a separate process running on the same machine or a separate machine. The performance
metrics can be processed asynchronously as well.
For example, a trading application may have a number of synchronous and asynchronous moving parts and metrics needs to be recorded for various operations like
placing a trade on to a queue, receiving asynchronous responses from the stock market, correlating order ids, linking similar order ids, etc. A custom metrics
gathering solution can be accomplished by logging the relevant metrics to a database and then running relevant aggregate queries or writing to a file system and
then running PERL based text searches to aggregate the results to a “csv” based file to be opened and analyzed in a spreadsheet with graphs. In my view, writing to
a database provides a greater flexibility. For example, in Java, the following approach can be used.

Asynchronous logging
— Use log4j JMS appender or a custom JMS appender to send log messages to a queue.
— Use this appender in your application via Aspect Oriented Programming (AOP – e.g Spring AOP, AspectJ, etc) or dynamic proxy classes to non-intrusively log
relevant metrics to a queue. It is worth looking at Perf4j and context based logging with MDC (Mapped Diagnostic Contexts) or NDC (Nested Diagnostic
Contexts) to log on a per thread basis to correlate or link relevant operations.
— A stand-alone listener application needs to be developed to dequeue the performance metrics messages from the queue and write to a database or a file system
for further analysis and reporting purpose. This listener could be written in Java as a JMX service using JMS or via broker service like webMethods, TIBCO, etc.

— Finally, relevant SQL or regular expression based queries can be written to aggregate and report relevant metrics in a customized way .

---

## Q7: Scenario : You are required to change the logic of a module that many other modules have dependency on. How would you go about making the changes
without impacting dependent systems.

**Answer:**

Solution : You need to firstly perform an impact analysis. Impact analysis is about being able to tell which pieces of code, packages, modules, and projects
use given piece of code, packages, modules, and projects, or vice versa is a very dif ficult thing.
Performing an impact analysis is not a trivial task, and there is not a single tool that can cater for every scenario. Y ou can make use of some static analysis tools
like IDEs (e.g. eclipse), JRipples, X-Ray, etc. But, unfortunately applying just static analysis alone not enough, especially in Java and other modern languages
whereas lots of things can happen in run time via reflections, dynamic class loading & configuration, polymorphism, byte code injection, proxies, etc.
a) In eclipse Ctrl+Shift+g c an be used to search for references
b) You can perform a general “File Search” for keywords on all projects in the work-space.
c) You can use Notepad++ editor and select Search –> Find in files. Y ou can search for a URL or any keyword across a number of files within a folder .
There are instances where you need to perform impact analysis across stored procedures, various services, URLs, environment properties, batch processes, etc.
This will require a wider analysis across projects and repositories.
Search within your code repository like Subversion (SVN) :
There are SVN tools like SVN Search or SVNQuery which performs SVN repository indexing and searching.
Tools like FishEye can be used to search across various code repositories. FisheEye is not tar geted for any special programming language. It just supports various
version control systems and the concept of text files being changed over time by various people. Handy for text searches like environment based properties files to
change a URL or host name from A to B.
Grep the Unix/Linux environment where your application is deployed.Y ou can perform a search on the file system where your application(s) are deployed.
Analyze acr oss various log files. It is also not easy to monitor service oriented architectures. Y ou can use tools like Splunk to trace transactions across the IT
stack while being tested by the testers to proactively identify any issues related to change. Splunk goes across multiple log files.svn list -R file:///subversion/repository | grep filename #unix
svn list -R file:///subversion/repository | findstr filename #windows

Conduct impact analysis sessions acr oss cr oss functional and system teams and communicate the changes. Brain storm major areas af fected and document
them. Have a manual test plan that covers the impact systems to be tested. Collaborate with cross functional teams and identify any gaps in your analysis. Have a
proper review and sign-of f process. Get more developers to do peer code reviews.
Have pr oper documentation with high level ar chitectur e diagrams and dependency graphs wher e possible. As the number systems grow, so does the
complexity. A typical enterprise Java application makes use of dif ferent database servers, messaging servers, ERP systems, BPM systems, W ork flow systems,
SOA architectures, etc. Use online document management systems like Confluence or W iki, which enables search for various documents.

---

## Q8: Scenario : You have a number of applications like cash, mortgages, investments, etc where a user has to login separately into each system. For example, a user
with all 3 products has to log in 3 times to access all 3 products. Is there a way to improve on this login process?

**Answer:**

Solution : You need to implement the SSO (Single Sign On). There are SSO systems like SiteMinder, Tivoli Access Manager (T AM), etc. As shown below ,
SiteMinder is configured to intercept the calls to authenticate the user. Once the user is authenticated, a HTTP header “SM_USER” is added with the authenticated
user name. For example “123”. The user header is passed to Spring 3 security. The “Security .jar” is a custom component that knows how to retrieve user roles for a
given user like 123 from a database or LDAP server. This custom component is responsible for creating a UserDetails Spring object that contains the roles as
authorities. Once you have the authorities or roles for a given user, you can restrict your application URLs and functions to provide proper access control.

SSO with SiteMinder

---

## Q9: Scenario : You have a requirement to maintain a history of insertion, modification, and deletion to the “Orders” table. How will you go about accomplishing
this?

**Answer:**

Solution : Create database triggers on insert, update, and delete.
Triggers give you control just before data is changed and just after the data is changed. This allows for:
— Auditing.
— Validation and business security checking if so is desired. Because of this type of control, you can do tasks such as column formatting before and after inserts
into database.
Triggers are handy for scenarios where:
— Auditing information in a separate table by recording the changes. Some tables are required to be audited as part of the non-functional requirement for changes.
— Automatically signaling other programs that action needs to take place when changes are made to a table.
— Collecting and maintaining aggregate or statistical data.

---

## Q10: Scenario : A main thread creates 3 database connections and assigns each of those connection to 3 dif ferent child threads that are spawned from the main
thread. The main thread must wait while all the child threads are completed and then close all the database connections. So, how will you accomplish this?

**Answer:**

Solution : This where the CountDownLatch comes in handy as you already know that there are finite (i.e 3) number of threads. The CountDownLatch can
be used by the main thread to wait on the child threads. A CountDownLatch will be created with 3 being the count.
The main thread will spawn new threads and wait on the count to reach 0 with the awit( ) method.CountDownLatch countDownLatch = new CountDownLatch (MAX_THREADS );
countDownLatch .await ();

As the each child thread process within the run( ) method, as the child thread completes processing, the count can be decremented with
countDownLatch .countDown ();

CountDownLatch

---

## Q11: Scenario : What if the above scenario has a specific requirement where the 3 children threads need to wait between each other? For example, if each of the 3
threads need to perform 2 tasks. Before the task 2 can be started by a thread, all the 3 child threads must finish task 1. The task 1 can be reading data from the
database and task 2 could be performing some computation and finally these computations need to be consolidated and written back by a single thread.

**Answer:**

Solution : A CyclicBarrier can be used if the number of children threads are know upfront and to implement waiting amongst child threads until all of them
finish. This is useful where parallel threads need to perform a job which requires sequential execution. It has methods like cyclicBarrier .await( ) and
cyclicBarrier .reset( );
CountDownLatch

CyclicBarrier
It acts as a barrier point for a number of worker threads to wait for each other to complete their tasks. The barrier is called cyclic because it can be re-used after all
the waiting worker threads are released for the next barrier point.
When to use CountDownLatch Vs CyclicBarrier
A CountDownLatch is initialized with a counter. Threads can then either count down on the latch or wait for it to reach 0. When the latch reaches 0, all waiting
threads can resume.
If you want a set of threads to repeatedly meet at a common point, you are better served by using a CyclicBarrier. For example, start a bunch of threads, meet, do
some stuf f like data consolidation or amalgamation, meet again, validate some assertions, and do this repeatedly .
A given CountDownLatch can only be used once, making it inconvenient for operations that occur in steps, with intermediate results from the dif ferent threads
needing to be consolidated between steps. The CountDownLatch also doesn’ t explicitly allow one thread to tell the others to “stop waiting”, which is sometimes
useful, for example, if an error occurs in one of the threads.
The CyclicBarrier is generally more useful than CountDownLatch in scenarios where:

— a multithreaded operation occurs in steps or iterations, and
— a single-threaded operation is required between steps/iterations, for example, to combine the results of the previous multithreaded steps.

---

## Q12: Scenario : Looping through a list of items and removing an item in the list could lead to “ Concurr entModificationException “.

**Answer:**

Scenario : 1) Instead of removing from the list, use an iterator and remove from the iterator to prevent the exception.
2) Use the CopyOnW riteArrayList instead of an ArrayList .

---

## Q13: Scenario : Two users try to modify the same record in the database. In this scenario, you want one modification to go through and the other modification to
notify the user as shown below .
“This record was not updated as the record you are trying to update has been updated by another user. Try refreshing your data, and update again.”

**Answer:**

Solution : This will require a number of steps.
Step 1 : You would require a “ version number” or a “ timestamp ” column in the database table to detect concurrent modifications.
Step 2 : When a record is initially read, the time stamp or version number also read.
@Override
public List<AdjustmentDetail > getAdjustmentRecords (final AdjustmentCriteria criteria )
{
 String sql = "select a.detailid, a.portfolioCd, a.accountCd, a.PositionIndicator, a.cashV alue, TmStamp = convert(int,substring(a.T imestamp,5,4))" +
 "from AdjustmentDetail a " +
 "Where a.portfoliocd = ? " +
 "and a.valuationDttm = ? " +
 "and a.inactiveFlag = 'N' " ;
 
 List<Object > parametersList = new ArrayList <Object >();
 parametersList .add(criteria .getPortfolioCode ());
 parametersList .add(criteria .getValuationDate ());
 
 Object [] parameters = parametersList .toArray (new Object [parametersList .size()]);
 
 List<AdjustmentDetail > adjustments = jdbcT emplateSybase .query (sql, parameters ,
 new RowMapper <AdjustmentDetail >()

Step 3 : After the record has been modified, when ready to update the record, do a select query first to read the time stamp or version number for the same record to
ensure that it has not been modified. if the “timestamp” or the “version number” has changed, you need to throw the above exception and abort modifying the
record as it had been modified by another user. {
 public AdjustmentDetail mapRow (ResultSet rs, int rowNum ) throws SQLException
 {
 AdjustmentDetail record = new AdjustmentDetail ();
 record .setDetailId (BigInteger .valueOf (rs.getLong ("DetailId" )));
 record .setPortfolioCode (criteria .getPortfolioCode ());
 record .setAccountcd (rs.getString ("accountCd" ));
 record .setAmount (rs.getBigDecimal ("cashV alue" ));
 record .setPositionIndicator (rs.getString ("PositionIndicator" ));
 record .setTimestamp (rs.getInt ("TmStamp" )); // timestamp to detect any later modifications
 return record ;
 }
 });
 return adjustments ;
 
@Override
public AdjustmentDetail modifyAdjustment (AdjustmentDetail adjDetail )
 {
 if (adjDetail == null) {
 throw new RuntimeException ("adjDetail is null" );
 }
 
 int noOfRecords = 0;
 String inactiveFlag ;
 
 try {
 //check if the record has been modified.
 Integer adjustmentModifiedT imestamp = getAdjustmentModifiedT imestamp (adjDetail .getDetailId ());
 
 //logic to modify adjustments go here

now the sample method that retrieves the timestamp.
Hibernate provides this “ Optimistic Concurr ency Contr ol” support. //every time the record is modified, the timestamp or version number is incremented.
 }
 catch (Exception e)
 {
 logger .error ("Error updating adjustment detail: ", e);
 }
 
 if (noOfRecords == 0) throw new ValidationException ("The adjustment was not updated. It may be the record you are trying to update has been updated by
another user .Try refreshing your data and update again." );
 
 logger .info("No of adjustment details updated = " + noOfRecords );
 return adjDetail ;
/retrieve the timestamp value for the given <em>detailid</em> to detect if it has been modified.
private Integer getAdjustmentModifiedT imestamp (BigInteger adjustmentDetailId ) {
String sql = "SELECT TmStamp = convert(int,substring(T imestamp,5,4)) from AdjustmentDetail where DetailId = ?" ;
List<Object > parametersList = new ArrayList <Object >();
parametersList .add(cashForecastDetaillId .intValue());
Object [] parameters = parametersList .toArray (new Object [parametersList .size()]);
List<Integer > ts = jdbcT emplateSybase .query (sql, parameters, new RowMapper <Integer >() {
 public Integer mapRow (ResultSet rs, int rowNum ) throws SQLException {
 Integer tsValue = rs.getInt ("TmStamp" );
 return tsValue;
 }
});
return ts.get(0);

---

## Q14: Scenario : How will you go about identifying duplicate records in a table?

**Answer:**

Solution : Using an aggregate SQL query. The following SQL query will do the trick.
Note : The Select columns that are not aggregated with count(..), sum(…), etc need to be in the “group by” clause.

---

## Q15: Scenario : How would you go about deleting the duplicate records?

**Answer:**

Solution : You could do it in a number of steps as shown below .
1) Create a temporary table.
2) Insert the unique records into the temporary table.
3) Delete the records from the original table.
4) Insert the saved single records from the temporary table back to the original table.

---

## Q16: Scenario : How will you go about writing an SQL query for the following scenario?
Valuation table with the following columns portfolioid, accountid, balance, inactiveflag, valuationdttm, and typecd.
The portfolio table has columns portfolioid, and portfoliocd.
The account table has columns accountid and accountcd.
Write an SQL query to extract out the accountcd and the corresponding balance for a given portfoliocd and valuationdttm. Please note that there will be multiple
balance records for each account, and your query must only extract out a single balance record per account based on the rule ‘extract the record with minimum
value for typecd’.

**Answer:**

Solution : With the help of “ inner joins ” to join tables and a “ correlated subquery “.
As you can see in the sample answer below, inner joins are used to join with the relevant tables. A sub query is used to calculate the min(typecd) to extract the
record with minimum value for typecd .SELECT code, user_name, COUNT (user_name ) AS NumOccurrences
FROM tbl_user
GROUP BY code, user_name
HAVING ( COUNT (user_name ) > 1 )

---

## Q17: Scenario : How will you extract rule_name values from a tabular data shown below and convert it to an SQL query as shown below as well?
This data could come from an excel spread sheet, word document, or copied from a confluence or wiki page. The SQL we need is:elect acc.accountcd, val.balance
from valuation val
 inner join portfolio pf on pf.portfolioid = val.portfolioid
 inner join account acc on acc.accountid = val.accountid
where pf.portfoliocd = 'QW234'
and val.valuationdttm = '28 Dec 2012'
and val.inactiveflag = 'N'
and acc.inactiveflag = 'N'
and val.typecd = (select min(val2.typecd ) from valuation val2
 where val2.valuationdttm = val.valuationdttm
 and val2.inactiveflag = 'N'
 and val2.accountid = val.accountid
 group by accountid )
order by acc.accountcd
d type rule_nmae bean_name
633 ALL asx100_rule SECURITY_V ALIDA TION
632 ALL asx200_rule SECURITY_V ALIDA TION
634 ALL ETF_rule SECURITY_V ALIDA TION
635 ALL managed_fund_rule SECURITY_V ALIDA TION
Select * from rules_table where rule_name in ('asx100_rule', 'asx200_rule', 'ETF_rule', 'managed_fund_rule' );

**Answer:**

Solution : Power of Notepad++ and regular expr essions .
Step 1 : Copy the data to Notepad++ and delete the header row by highlighting it and pressing the delete button.
Step 2 : You need to now remove all the columns except rule_name column. T o do this place the cursor LHS of first 633 value and press Alt + Shift + Highlight
keys together and highlight the columns you want to remove with the mouse. Do the same for the last column as well.
Step 3 : Next step is to remove any leading or trailing spaces. Use regex based find and replace command. Pressing CTR+ F will bring the Find dialog. Y ou can
also select it from the “Search” menu at the top.
Notepad++
Find What: [\s]+
Replace with: ,
Step 4 : Remove the new line characters or carriage return by finding and replacing with the “Extended …” option turned on.

Find What: \r
Replace with:
Note : Replace new lines with nothing.
Step 5 : You need to put a single quote (‘) around the entries for the SQL query. Regex is agin back to the rescue.
Find What: ([^,]*)(,?)
Replace with: ‘\1’\2
The parentheses ‘( )’ are used to capture the values. and \1 and \2 represent both the captured values. The ‘ is add before \1 and \2. Where \1 is the value like
“asx100_rule” and \2 is “,”. The * means 0 or many, and ? means 0 or 1.
You can now take the single line text and put it in your where clause. This is very handy when you have to work with larger data.
Note: Excel spreadsheets are very handy in constructing more tedious to write SQL queries.
When you have some data in tabular (e.g. Excel spreadsheet) format and would like to insert into a database table, you need to write an SQL insert query .
Manually writing SQL query for multiple records can be cumbersome. This is where Excel spreadsheet comes in handy as demonstrated below. A single SQL
query can be copied down where the formulas get copied with incrementing column numbers.
The Excel concatenate character & is used to achieve this. The $ means fix. $a1 means fix excel column A. When you copy the formula, the row numbers will be
incremented like 2,3,4, etc, but the column will remain fixed to A. In the example below
$A$1 = first_name
$B$1 = surname
$C$1 = age
Note : Both column and row are prefixed with $, which means both are fixed.

Generate SQL queries with Excel spreadsheets
The Excel formula is:
The above Excel expression is easier to understand if broken down as shown below where the concatenation character & plays a major role in combining static text
within quotes with dynamic formulas like $A$1.="insert into person (" &$A$1&", "&$B$1&", "&$C$1&") values ('" &$A2&"','"&$B2&"',"&$C2&")"
insert into person ("
&
$A$1
&
, "
&
$B$1
&

---

## Q18: Scenario : How will you determine in your Java application if it is running in production mode or not. This is required when you want to do things slightly
differently when you are in non prod. For example, in non prod, you will send emails to yourself or your team. In prod environment to actual external clients.

**Answer:**

Solution : Have a JVM property like isProd set to true or false.
In non prod JEE containers or environments it will be set to false, and in prod environment to true.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit », "
$C$1
&
) values ('"
&
$A2
&
','"
&
$B2
&
',"
&
$C2
&
)"
DisProd =true

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03