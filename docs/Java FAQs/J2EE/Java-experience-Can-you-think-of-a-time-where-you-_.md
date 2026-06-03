# Java experience   Can you think of a time where you ... 

## Table of Contents

- [Q1: Can you think of a time where you accomplished QuickW ins for your company?](#q1)
- [Q2: Can you think of times where you were proud of your accomplishments ?](#q2)
- [Q3: Can you think of a time where you solved hard to debug “intermittent issues” ?](#q3)
- [Q4: Can you think of a time where you had to identify “performance issues and bench ](#q4)
- [Q5: Can you think of a time where you championed a continuos impr ovement program?](#q5)
- [Q6: Can you think of a time where you proactively prevented the likelyhood of embara](#q6)
- [Q7: Can you think of times where you properly though through security implications?](#q7)
- [Q8: Can you think of times where using the right tool and your “know how” icreased y](#q8)
- [Q8: Can you think of a time where you acted as a “change agent” ?](#q8)

---

## Q1: Can you think of a time where you accomplished QuickW ins for your company?

**Answer:**

The focus is to improve the overall ef fectiveness and usefulness of a system through small changes in a collaborative ef fort with the business.
Example: Spearheaded the “Quick W ins” project by working very closely with the business and end users to improve the current website’ s ranking from being
23rd to 6th in just 3 months.

---

## Q2: Can you think of times where you were proud of your accomplishments ?

**Answer:**

Reduced the over -night batch job runtime from 3 hours to 40 minutes by optimizing the SQL that was performing very badly in the query plan, and
replacing the pessimistic locking with optimistic locking.
On taking up the position as a Java technical lead, I set up a team of 8 developers to successfully complete the project on time and within budget.
Instrumental in designing and developing a Java/JEE based online insurance system, which serves 400 concurrent users using Spring, Hibernate, JSF ,
ajax, and jQuery .
Designed and developed the payment and claims module, which is capable of handling 120 requests per second and runs as a true 24 x 7 system module.
Championed the iterative and test driven development (TDD) with regular code reviews, bamboo based continuous integration, and sonar based code
coverage, which resulted in not only more maintainable and extensible code, but also roughly 30% drop in bugs.
Increased the number of JUnit tests from 30+ to 200+ in my watchful eye to improve the overall quality of the application.
Conceptualized and implemented a lar ge scale multi-threaded socket server to communicate with 450 retail stores and seamlessly integrated it with 3
other back end systems via W eb services and asynchronous messaging.

---

## Q3: Can you think of a time where you solved hard to debug “intermittent issues” ?

**Answer:**

It is always a challenge to isolate, reproduce, and fix intermittent issues. Y ou may have come across intermittent issues that are hard to reproduce and
debug. A novice developer will promptly mark those defects in the bug tracking system as “cannot be reproduced” without having the skill to isolate, reproduce
and then fix the issue. An experienced developer with good grasp on the key areas will not only have the skill to identify these potential issues by analyzing the
code, but also will have the competency to isolate, reproduce, and fix these issues:
Not adhering to language basics and contracts can lead to non-deterministic behaviors. For example, in Java, incorrect implementation of hashCode(),
equals(..), and compareT o(..) method contracts can lead to unpredictable
Servlets, Struts action classes, SimpleDateFormat, etc are not inherently thread-safe and can be accessed by multiple threads. Hence not using them in a
thread-safe manner can cause intermittent issues.

Some operations not only need to be thread-safe, but also must be atomic. Issues arising from incorrect transaction management can cause intermittent
issues by corrupting some records in the database tables. The database operations need to be atomic. If the transactions are carried across two distributed
systems, then the 2-phase commit transaction management needs to be used. If proper transaction isolation levels are not used, the flight seats can be
double booked due to dirty reads, phantom updates, or phantom inserts. Be aware of the ACID (Atomic, Consistent, Isolation, and Durability) properties
to ensure that the database transactions are processed reliably . The web services based transactions need to be using compensation based transaction
management as opposed to the ACID based transaction management.
You could also have intermittent issues due to proxy server or load balancer timeouts or connection leaks. Y our application might not be coded with
appropriate connection retries or appropriate time out values. Y ou could simulate connectivity issues by creating SSH tunnels to the actual destination
server .
For example , your application may have connectivity to a V ignette ServerB running on port 91 11. To simulate connectivity issues, you may create a
local SSH tunnel to a UNIX ServerA, which connects to V ignette application running on ServerB:91 11. Once you log on to ServerA via PutTTY (i.e. a
Unix client on W indows) as shown below , a tunnel will be opened to ServerB:91 11. Any calls to localhost:4000 will be forwarded to ServerB:91 11 via
ServerA. In your application, you could use the localhost:4000 instead of using ServerB:91 11 to connect to the V ignette application. This will enable
you to simulate the scenario of the V ignette server on ServerB being down by just destroying the tunnel, and not the actual server , which might be used
by others. By destroying the tunnel, you could improve your application code to graciously handle connectivity issues, timeouts, and retries. The
exception handling logic also needs to be verified so that that it does not expose any internal server details.

There could be general runtime production issues that either slow down or make a system to hang. In these situations, the general approach for
troubleshooting would be to analyze the thread dumps to isolate the threads which are causing the system to slow-down or hang. For example, a Java
thread dump gives you a snapshot of all threads running inside a Java V irtual Machine. There are graphical tools like Samurai to help you analyze the
thread dumps more ef fectively .
Application seems to consume 100% CPU and thr oughput has drastically r educed – Get a series of thread dumps, say 7 to 10 at a particular
interval, say 5 to 8 seconds and analyze these thread dumps by inspecting closely the “runnable” threads to ensure that if a particular thread is
progressing well. If a particular thread is executing the same method through all the thread dumps, then that particular method could be the root cause.
You can now continue your investigation by inspecting the code.
Application consumes very less CPU and r esponse times ar e very poor due to heavy I/O operations like file or database read/write operations – Get a
series of thread dumps and inspect for threads that are in “blocked” status. This analysis can also be used for situations where the application server
hangs due to running out of all runnable threads due to a deadlock or a thread is holding a lock on an object and never returns it while other threads are
waiting for the same lock. The solution to the above problems could vastly vary from fixing the thread safety issue(s) to reducing the size of
synchronization granularity , and from implementing appropriate caching strategies to setting the appropriate connection timeouts.
Intermittent issues could also arise due to environmental complexities. Y our application might be running under an uncontrolled environment. Lack of
communication among multiple projects using the same environment could cause intermittent issues. For example, security certificates or passwords
could have been modified by the system administrators. Database table or LDAP server schemas could have been modified by the other developers.
Messages published to a queue may have been consumed by another process listening on the same queue.
Speaking of intermittent issues, database deadlock issues are very common. Whenever you have competing DML (Data Manipulation Language)
running against the same data, you run the risk of a deadlock. When this happens, the database server identifies the problem and ends the deadlock by
automatically choosing one process and aborting the other process, allowing the unaborted process to continue. The aborted transaction is rolled back
and an error message is sent to the user of the aborted process. For example, in Sybase database
Generally , the transaction that requires the least amount of overhead to rollback is the transaction that is aborted. Y ou can deal with database deadlock
situations in two ways. The first option is to redesign the application so that the deadlock does not take place in the first place. This is the preferred
option, but at times not practical to redesign an existing application without significant ef fort. The second option is to retry the aborted task after
receiving a deadlock message, and it will most likely succeed during 2-5 additional retries. If the retry happens very frequently , the performance of the
application can be adversely impacted. Here are some general tips on how to avoid deadlocking
Aim for properly normalized database designs.Your server command (family id #%d, process id #%d) encountered a deadlock situation. Please re-run your command.

Avoid or minimize the use of cursors.
Keep transactions as short as possible, use lower isolation levels and minimize the number of round trips between your application and the
database server .
Reduce lock time. T ry to develop your application so that it grabs locks at the latest possible time, and then releases them at the very earliest
time.
Make relevant design or coding changes like single-threading related updates, and re-scheduling batch update jobs to low-update time period can
often remove deadlocks.

---

## Q4: Can you think of a time where you had to identify “performance issues and bench mark them” with the view to improve?

**Answer:**

I have never been in a project or or ganization that is yet to have any performance or scalability issues. It is a safe bet to talk up your achievements in fixing
performance or resource leak issues in job interviews. Unlike web security testing, performance testing is very prevalent in many applications. Premature
optimization of your code is bad as it can compromise on good design or writing maintainable and testable code. But one needs to be aware of potential causes
for performance issues that can occur due to major bottlenecks in a handful of places or minor inef ficiencies in thousands of places (i.e.death by thousand cuts ).
Let’s look at some of the common causes of performance issues.
Too many database calls and inef ficient SQL queries performing full table scan can cause performance issues. SQL statements need to be carefully
constructed and prepared statements need to be favored over ordinary statements, as it improves performance by pre-compiling the execution plan for
repeated calls. In some scenarios, more data is requested than actually is required by the current page. Eagerly fetching data from an ORM tool can bring
in more data than actually required. In other scenarios, too many fine grained calls are made to the database instead of eagerly fetching the required data
in 1 or fewer calls. At times, not using proper pagination strategies to divide and conquer how data is accessed or not carefully thinking through ways to
improve re-usability of the same data requested multiple times through caching strategies can lead to bad performance. Excessive or wrong caching
strategies to minimize remote calls could adversely impact performance due to increased garbage collection activity to make the memory available.
A frequently back tracking regular expression can adversely impact performance. Care should be taken to not run a web service or a web application that
allows users to supply their own regular expressions. People with little regular expression experience can come up with an exponentially complex
regular expressions.
Frequent garbage collections can adversely impact performance. T une GC and allocate adequate heap space.
Memory leaks and connection leaks (e.g. database connections, LDAP connections, sockets) can cause performance and scalability issues. W asteful
handling of these finite resources can adversely impact performance. These resources must be judiciously used for a request and promptly returned to
the pool to be reused by other users or requests. It should not be kept open for the whole session for a particular user because a user session can span
multiple clicks interleaved with long pauses and user think times.
Third party rich web frameworks can make your front end HTML source code to bloat. For example, overuse of some libraries or rich widgets can not
only make the HTML code bloated, but also its aggressive use of ajax/JavaScript can result in performance problems on the browser . This might not be a
problem for an internal application, but can be a problem for all external customer facing applications as some users may not have super fast internet
access. Some older versions of some browsers may also take longer time to render the bloated HTML code.
Not setting approprite service time outs.

Not performance testing your application, or performance testing with non-production like scenario — cut down data sets, no or fewer concurrent
access, etc
Once you identify a performance issue, proper test scripts need to be written and run to benchmark a particular application. Open source tools like BadBoy or
JMeter proxy server can be used to record the web actions, and the recorded scripts can be converted to JMeter scripts to perform stress/load tests to benchmark
the application. The initial test conditions and results need to be properly documented so that the results can be compared after improving or fixing the
performance issues. The load/stress tests can also be used to reproduce and fix intermittent issues like thread-safety , memory leak, connection leak, and database
contention issues that mostly occur above certain load.

---

## Q5: Can you think of a time where you championed a continuos impr ovement program?

**Answer:**

Organizations must develop a culture of continuous improvement in order to flourish. I am yet to work for an or ganization that did not have any
environmental or process related issues and challenges. Many of these tasks can be simplified, automated, and integrated to avoid monotonous and error prone
human intervention. So, experience with the agile practices, continuous integration and build, environmental improvements, etc will be of great asset to any
organization. A software must be architected for deployability . Deployability is a non-functional requirement that addresses how reliably and easily a
software can be deployed from development into the production environment. Firstly , for a software to be deployable, there must be minimal dif ferences
between environments. The more similar the environments, the simpler and more reliable it is to deploy the software. It is not possible to completely eliminate
any dif ferences between various environments, but certainly can be minimized. The continuous improvement programs can be championed as described below:
Describe the Issue --> Determine the Cause --> Resolve the Issue Follow -Up

Some or ganizations are very passionate about hiring professionals with experience in agile practices. If you had experience in facilitating agile practices, it will
be looked upon very favorably by those or ganizations.
It is a best practice to have project retrospective meetings after each major mile stones of a project to see what worked well, what did not work well, lessons
learned, and how could you improve on things that did not work well for the next milestone release.
Making technical changes are easier than changing human behavior . So, to champion continuous improvement processes, one needs to have good technical
skills complemented with great soft skills.

---

## Q6: Can you think of a time where you proactively prevented the likelyhood of embarassing mistakes?

**Answer:**

Here are some examples of the embarrassing mistakes that software professionals need to
be aware of.
Inadvertently spamming your clients with unintended emails during development or testing phases. A void these types of issues with proper
configuration files using internal email exchange servers and mocked up data using your email address to receive and send emails. A -DisProd=false
JVM ar gument check will make it more fool proof.
Hard coding production URLs directly or indirectly in your code base or performance test scripts. A void these issues by always externalizing server ,
URL, and other environment specific details to relevant configuration files.
Security holes that allow URL parameters to be manipulated to view others’ personal details and sensitive information. T est your application by
modifying the URL parameters to identify any obvious security holes. For example, if you are using a URL like http://myapp.com/accountId=123 , try
replacing the accountId to some other accountId.
Input search boxes break when special characters like ‘%’, &, etc are entered. T est your input boxes for special characters and implement proper client
side and server side input validation.
Other environmental issues like testing the application against a cut-down database to later find that the application does not perform or scale well with
the production size data. A void these issues with proper performance testing in a production like environment.
It is a known fact that the software artifacts need to be properly versioned. This is a mostly adhered standard practice. But when designing an application
with dynamic application configuration values or validation rules via database, some designers overlook the importance of versioning. In lar ger
applications, at any point in time there will be multiple streams of parallel development work going
on. Many development streams will be sharing the same database. So, if you don’ t have proper versioning, any changes to the configuration values or
validation rules can break other development streams. W ith proper versioning, multiple streams can use the relevant version numbers.
Some developers fail to understand or ignore the dif ference between a local call and a remote call. The remote calls have more performance overheads
due to network round trips and serialization/deserialization of data. So, avoid making remote calls (via RPC or web services) from a loop on the client
side. This will end up in many remote calls. It is better to have the loop on the service side and get the client to make a single remote call by passing a
collection of data. This collection of data can be looped through on the server side by making local calls within the same process.
Never hard code system error or warning messages, config parameters like host name, web service URLs, service timeouts, etc and application specific
business data like max account count, bulk data load threshold, feature on/of f choices, etc. The error/warning messages and UI labels need to be stored

in internationalizable files. Additional files can be added relevant to the locale. The system configuration parameters need to be configured via
environment specific properties files and stored separately from the deployable artifacts. The business specific configuration data can be stored either in
a config table in a database or in a configuration file stored outside the deployable artifact.

---

## Q7: Can you think of times where you properly though through security implications?

**Answer:**

For example, if your application is sending reports containing sensitive information to clients, it is imperative that the reports are adequately protected. This
can be achieved by sending the reports as password protected zip files. This involves additional considerations listed below to minimize security issues.
The generated report and the password to unzip the report(s) need to be sent out in separate emails.
Individual (i.e. per client) passwords need to be used and regularly (say every 3 months) recycled.
These individual passwords need to be stored encrypted in a database or a file system.
A master password needs to be used to encrypt and decrypt the individual passwords. Never store them in clear text.
The master password itself needs to be securely stored.

---

## Q8: Can you think of times where using the right tool and your “know how” icreased your productivity?

**Answer:**

For example, if your application is sending reports containing sensitive information to clients, it is imperative that the reports are adequately protected. This
can be achieved by sending the reports as password protected zip files. This involves additional considerations listed below to minimize security issues.
JMS based log4j logging to measure metrics, especially all the asynchronous processes in a lar ge trading application.
Use log4j JMS appender or a custom JMS appender to send log messages to a queue.
Use this appender in your application via Aspect Oriented Programming (AOP – e.g Spring AOP , AspectJ, etc) or dynamic proxy classes to non-
intrusively log relevant metrics to a queue. It is worth looking at Perf4j and context based logging with MDC (Mapped Diagnostic Contexts) or
NDC (Nested Diagnostic Contexts) to log on a per thread basis to correlate or link relevant operations. Perf4J is a great framework for
performance logging. It’ s non-intrusive and really fills the need for accurate performance logging
A stand-alone listener application needs to be developed to dequeue the performance metrics messages from the queue and write to a database or a
file system for further analysis and reporting purpose. This listener could be written in Java as a JMX service using JMS or via broker service like
webMethods, TIBCO, etc. This service needs to correlate related messages using a correlation id to determine the elapsed time at the various
points in the system.
Finally , relevant SQL (for database) or Splunk/regular expression (for flat files) based queries can be written to aggregate and report relevant
metrics in a customized way .
Tools like regexpal.com is very handy to quickly verify your regular expressions while working on a project. The regex keywords and the results are
highlighted in color for better clarity .
Notepad++ is a powerful text editor with many handy features like converting an unquoted CSV string to quoted csv string, converting comma separated
entries to “new line” separated entries, extracting column valuee, converting these column values to single quoted csv string to be used in SQL “in”
clasuse, etc.

Spreadsheets like MS Excel is handy to construct SQL queries with static and dynamic data. The Excel concatenate character “&” can be used to
construct insert statements from tabular data. The formula is constructed for the initial row 2 data, and then copied down for the remaining rows. Saving
you lots of typing. The “$” symbols are used to fix row , column or both.
SoapUI and Firefox plugins (e.g. poster plugin) to debug issues rtelating to web services.

---

## Q8: Can you think of a time where you acted as a “change agent” ?

**Answer:**

A change agent not only changes a system’ s or application’ s behavior but also people’ s behavior , attitude, and culture towards writing quality software.
People’ s behavior can be changed by enforcing good coding standards. One way to enforce these standards is through regular peer code reviews. Code review
sessions can catch more bugs earlier on in the software development life cycle. It gives junior developers to learn from more experienced professionals. Itinsert into person ("
&
$A$1
&
, "
&
$B$1
&
, "
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

motivates developers to avoid common mistakes, sloppy code, and inadequate unit test coverage. Automated code review tools like sonar can also be used to
enforce consistency in code, identify potential bugs and redundant code snippets. The peer code reviews can be better managed through tools like Crucible.
Substandard software development processes can also demotivate developers and adversely impact their behavior and attitude. The development processes need
to be continuously improved by adopting good tools and practices. A build and deploy process that you can repeat consistently across team members, across
environments, and across dif ferent versions of the software is critical to successfully shipping your software or
deploying it into production. This can make developers’ life a lot easier by allowing them to concentrate more on what they do best rather than wasting time on
fixing bad processes or performing administrative tasks.
Finally , a framework needs to be put in place for adequate documentation. Most software professionals don’ t like to write documentation. Proper guidelines and
templates need to be created to encourage developers to write simple enough, but not too simple documentation. No body is going to read complex
documentation. Documentation needs to be written with the tar get audience in mind. All the documents should be easily accessible. Good document
management systems/repositories allow developers to find documents quickly by project, contents, date, author , etc and saves lots of frustrations.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
