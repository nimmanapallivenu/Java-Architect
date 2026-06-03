# 11 In your Java experience, what are ... Q&As

## Table of Contents

- [Q1: In your Java experience, what are some of the common dilemmas you face when writ](#q1)
- [Q2: In your Java experience, what are some of the common mistakes developers, especi](#q2)
- [Q3: In your Java experience, what are some of the very useful new features in Java 7](#q3)
- [Q4: In your Java experience, what are some of the very useful new features in Java 8](#q4)
- [Q5: In your experience, will you favor Spring, JEE or both?](#q5)
- [Q6: In your experience, what are some of the must have tools apart from the IDEs lik](#q6)
- [Q7: In your experience, when will you favor XML over JSON for data transfer , and wh](#q7)
- [Q8: In your experience, what XML parsing libraries have you used, and why?](#q8)
- [Q9: In your experience, what are some of the widely used Unix commands as a Java dev](#q9)
- [Q10: In your experience, what libraries/frameworks have you used for the following sc](#q10)
- [Q11: In your experience, what are some of the mistakes or slackness you have noticed ](#q11)

---

## Q1: In your Java experience, what are some of the common dilemmas you face when writing unit tests?

**Answer:**

Whether to fix the code or the test. When you write unit tests, sometimes you feel compelled to change your code just to facilitate the test. For example,
when you need to test a private method or attribute. Doing so is a bad idea. If you ever feel tempted to make a private method public purely for testing
purposes, don’ t do it. T esting is meant to improve the quality of your code, not decrease it. Having said this, in most cases, thinking about the test first in a
TDD (.i.e. T est Driven Development) will help you refactor and write better code. Also, mock frameworks like PowerMock , let you mock private
methods, static methods, constructors, final classes and methods, etc. Care must be taken in testing private methods. Inside a class you can end up with a
lot of private methods manipulating instance or class variables without having them passed as parameters. This leads to high method coupling, and
something not recommended. Instead use private methods that take explicit parameters and provide explicit return values.
Whether to use mocks or not. The quality unit tests need to be repeatable. Some non-deterministic or environmental conditions can make your code
fragile. Mocking these scenarios can make your code more robust. For example, a CircusService class can use a mock CircusDao to avoid test cases
failing due to data related issues. Alternatively , the test cases can use frameworks like DBUnit to insert data during test set up and remove data during
the test tear down to provide stable data for the test cases. Some test cases rely on W eb service calls, and availability of these services are non-
deterministic, and it makes more sense to mock those services.
How to test non-functional r equir ements? For example, a non-functional requirement like 95 percent of all W eb-based transactions should complete
within 2 seconds. Y our approach here would be to simply run the test code many times in a loop and compare the transaction times with tar get time, and
keeping track of the number of passes and fails. If at the end of the test less than 95 percent of the transactions fail, then fail the test too. There are
scenarios where @T est(timeout=5) becomes handy to fail a test case that takes longer than 5 seconds to execute.
Testing multi-thr ead code . This can be quite challenging and sometimes an impossible task. If the tests are too complex, then review your code and
design. There are ways to program for multi-threading that are more conducive for testing. For example, making your objects immutable where possible,
reducing the number of instances where multiple threads interact with the same instance, etc. There are frameworks like GroboUtils and
Multithr eadedTC aiming to expand the testing possibilities of Java with support for multi-threading and hierarchical unit testing. The goal of jconch
project is to provide safe set of implementations for common tasks in multi-threaded Java applications. The SerialExecutorService class is one of them.

---

## Q2: In your Java experience, what are some of the common mistakes developers, especially beginners make?

**Answer:**

Mistake #1: Using floating point data types like float or double for monetary calculations. This can lead to rounding issues.
Mistake #2: Using floating point variables like float or double in loops to compare for equality . This can lead to infinite loops.
Mistake #3: Not properly implementing the equals(..) and hashCode( ) methods. Can you pick if anything wrong with the following Person class.
Mistake #4: Getting a ConcurrentModificationException when trying to modify (i.e. adding or removing an item) a collection while iterating.

Mistake #5: Common exception handling mistakes.
a) Sweeping exceptions under the carpet by doing nothing.
b) Inconsistent use of checked and unchecked (i.e. Runtime) exceptions.
c) Not realizing that “exceptions are polymorphic in nature and more specific exceptions need to be caught before the generic exceptions”.
d) W iping out the stack trace.
e) Unnecessary Exception T ransformation.
Mistake #6: Using System.out.println(… ) statements for debugging without making use of the debugging capabilities provided in IDE tools. If you need to log,
use log4j library instead.
Mistake #7: Reinventing the wheel by writing your own logic when there are already well written and proven APIs and libraries are available. When coding,
always have Core Java APIs, Apache APIs, Spring framework APIs, Google Gauva library APIs, and relevant reference documentations handy to reuse them
instead of writing your own half baked solutions.
Mistake #8: Resource leak issues are reported when resources are allocated but not properly disposed (i.e. closed) after use.
Mistake #9: Comparing two objects ( == instead of .equals)
Mistake #10: Confusion over passing by value, and passing by reference as Java has both primitives like int, float, etc and objects.
Mistake #1 1: NullPointer exceptions being thrown at wrong places due to not writing proper fail fast code with proper input validation and being unaware of
the implicit auto unboxing.
Mistake #12: Not properly understanding and using Generics. Especially , using of the wildcards.
Mistake #13: Forgetting that Java is zero-indexed, using deprecated methods and APIs (e.g. Java Date or Calendar) that are deemed confusing and prone to
errors.
Mistake #14: Not writing thread-safe code with proper synchronization or thread-local objects. For example, the formatter objects like SimpleDateFormat is not
thread-safe, and you need to provide thread-safety .

---

## Q3: In your Java experience, what are some of the very useful new features in Java 7?

**Answer:**

#1. AutoCloseable interface: Java 5 introduced the Closeable interface and Java 7 has introduced the AutoCloseable interface to avoid the unsightly
try/catch/finally(within finally try/catch) blocks to close a resource. It also prevents potential resource leaks due to not properly closing a resource. The
java.io.InputStream and java.io.OutputStream now implements the AutoCloseable interface.

#2. Multi-catch to avoid code duplication.
#3. Fork and Join: Java 7 has incorporated the feature that would distribute the work across multiple cores and then join them to return the result set as a Fork
and Join framework. he ef fective use of parallel cores in a Java program has always been a challenge. It’ s a divide-and-conquer algorithm where Fork-Join
breaks the task at hand into mini-tasks until the mini-task is simple enough that it can be solved without further breakups. One important concept to note in this
framework is that ideally no worker thread is idle. They implement a work-stealing algorithm in that idle workers “steal” the work from those workers who are
busy.
#4. The NIO 2.0 has come forward with many enhancements . It’s also introduced new classes to ease the life of a developer when working with multiple file
systems with classes and interfaces such as Path , Paths , FileSystem , FileSystems and others./ Java 7 -- more concise 1 1 lines as opposed to 20 lines
ry (InputStream is = new FileInputStream (new File("c://temp/simple.txt" ));
 InputStreamReader isr = new InputStreamReader (is);
 BufferedReader br2 = new BufferedReader (isr);) {
 String read;
 while ((read = br2.readLine ()) != null) {
 System .out.println (read);
 }
catch (IOException ioe) {
 ioe.printStackT race();
/ Java 7 '|' separated multi-catch
ry {
someMethod ();
} catch (CustomException1 |CustomException2 ex) {
ex.printStackT race();
}

#5. Impr oved type infer ence for generic instance creation to make it less verbose .

---

## Q4: In your Java experience, what are some of the very useful new features in Java 8?

**Answer:**

#1: Interface can have static and default methods. This tries to solve the diamond (aka multiple inheriance) issue. In the past it was essentially impossible for
Java libraries to add methods to interfaces. Adding a method to an interface would mean breaking all existing code that implements the interface. Now , as long
as a sensible default implementation of a method can be provided, library maintainers can add methods to these interfaces. The static methods serve as utility
methods, hence you don’ t need a separate helper class. Y ou no longer need to define an interface and a helper class as in Collection interface and Collections
utility class and Path interface and Paths utility or helper class, etc.
#2: Lambda expr ession for supporting closur es, which simplifies your code. In the past, you need to write anonymous classes. Lambda expressions are
anonymous methods which are intended to replace the bulkiness of anonymous inner classes with a much more compact mechanism.
Important: In OOP , x = x+ 5 makes sense, but in mathematics or functional programming, you can’ t say x = x + 5 because if x were to be 2, you can’ t say that 2
= 2 + 5. In functional programming you need to say f(x) -> x + 5. This is why you have “->” in lmbda expression
#3 Java 8 adopts Joda as its native date & time framework . Due to what was perceived as a design flaw in Joda, Java 8 implemented its own new date / time
API from scratch. The new APIs were designed with simplicity in mind./no duplication of generic inference on the RHS. the angle bracket is empty
Map<String , List<Employee >> mapEmployees = new HashMap <>();
button2 .addActionListener (testObj ::doAnotherSomething ); //instance method reference
/input is e and result is println(..) and doW ork() functional interface method invocation
button3 .addActionListener ((e) -> {System .out.println ("B3 clicked ..." ); testObj .doWork();});

#4 Java 8 corr ects its lack of good support for JavaScript by introducing a new JVM JavaScript engine named “ Nashorn “. Together with a new command
line program called “jjs” it is trivial to write applications in Javascript which in turn can utilize any JVM based library .
#5 Parallel pr ocessing . In pre Java 8, the call to Arrays.sort( ) uses the mer ge sort algorithm sequentially . Java 8, introduces a new API for sorting the array in
parallel with Arrays.parallelSort( ) . Arrays.parallelSort( ) uses Fork/Join framework intr oduced in Java 7 to assign the sorting tasks to multiple threads
available in the thread pool. Fork/Join implements a work stealing algorithm where an idle thread can steal tasks queued up in another thread.

---

## Q5: In your experience, will you favor Spring, JEE or both?

**Answer:**

Definitely not both as they will add extra complexity to your project. The answer depends on if you are enhancing an existing project or building a new
project. If you are enhancing an existing project written in Spring, then it makes sense to stick to Spring.
If you are building a brand new project, then it makes more sense to use JEE. J2EE was bad, and Spring & Hibernate filled the gap. Now , JEE has adopted the
Spring and Hibernate frameworks’ POJO driven light weight container approach. There are many reasons to use JEE.
#1. JEE is a set of standard specifications , hence it is vendor -independent, and several implementations can be plugged in. For example, JP A. Many big and
small providers provide implementations to these JEE standards.
#2. T esting in JEE is impr oved with Lightweight application servers and frameworks such as Arquillian .
#3. JEE 7 has support for asynchr onous Servlet to push data asynchronously from the server to the client, and has support for non blocking IO, which gives
better scalability .
#4. Even though JEE application servers are bit more heavy weight than deploying a Spring based application to a W eb container , the JEE application servers
have come a long way to be more light weight than used to be. The JEE now has web pr ofile to make your JEE deployment more modular .
#5. JEE now has batch jobs framework as well.

---

## Q6: In your experience, what are some of the must have tools apart from the IDEs like eclipse, text editor like Notepad++, and Unix emulators like MobaXterm,
Putty , WinSCP , etc?

**Answer:**

#1. A number of code quality tools like Sonar , which is a very powerful tool covering 7 axes of code quality as shown below .

Sonar
#2. Database administration and SQL execution tools such as db-visualizer , SQuirr eL SQL , SQL Developer , DBArtisan , or Toad.There are command-line
tools like iSQL for Sybase and SQLPlus for Oracle. BCP (stands for Bulk CoPy) and SQL Loader are ETL (i.e. Extract T ransform and Load) tools to import
data from a flat file into a database and export data from a database into a flat file.
#3. Using continuous integration servers (on a clean separate machine) like Bamboo , Hudson , CruiseContr ol, etc to continuously integrate and test your code.
This is required for the developers to continuously build, deploy , test, and deliver .
#4. Performance testing tools like JMeter for testing web applications, web services (both SOAP and RESTful), and SQL query performance.
#5. Penetration testing or “PEN test” tools like Google’ s Skipfish , Firefox plugin “ tamperdata “, etc to identify security holes in your web application.
#6. Splunk , which is an enterprise-grade software tool for collecting and analyzing “machine data” like log files, feed files, and other big data in terra bytes.
You can upload logs from your websites and let Splunk index them, and produce reports with graphs to analyze the reports. This is very useful in capturing start
and finish times from asynchronous processes to calculate elapsed times to debugging complex issues.
#7 N ow a days, W eb services and JMS based messaging are used very widely . So,
— soapUI is a functional testing tool for SOA and W eb Service testing. soapUI provides complete test coverage – from SOAP and REST -based W eb services, to
JMS enterprise messaging layers, databases, Rich Internet Applications, and much more.
— HermesJMS is an extensible console that helps you interact with JMS providers making it simple to publish and edit messages, browse or search queues and
topics, copy messages around and delete them.
#8 Relevant browser plugins and add-ons to perform client side debugging. For example, Firefox plugins. JavaScript based MVC frameworks are very popular .

---

## Q7: In your experience, when will you favor XML over JSON for data transfer , and when will you favor JSON over XML?

**Answer:**

Favor XML over JSON
When you need to validate your messages using XSDs , Schematr on, etc
When you need to transform your messages using XSL T. For example, XML to HTML, etc
When you need to inter -operate with environments that don’ t support JSON
When you need to have a lots of strong mark-ups.
Favor JSON over XML
When messages don’ t need to be validated or transformed
When messages predominantly have data and no marked up texts
When messaging end-points have JSON support. For example, JSON/HTTP , JSON/SMTP , etc
JSON is less verbose, simpler , and performs better .
XML Data
JSON Data

---

## Q8: In your experience, what XML parsing libraries have you used, and why?

**Answer:**

<Employee >
 <name type="first" >Peter </name >
 <age>25</age>
</Employee >
{"name" :"Peter" , "nameT ype":"first" , "age" :"25"}

#1. DOM Parser loads the whole XML structure into memory , and you can read and write. This is useful for smaller XML documents which don’ t impact
memory usage. JDOM is another alternative to DOM4J.
#2. SAX Parser to only read an XML document. Good to parse lar ge XML documents. SAX ‘pushes’ XML events, leaving it up to you to determine where the
XML events belong in your code logic.
#3. StAx Reader/W riter works with a datastr eam oriented interface . The program asks for the next element when it’ s ready just like a cursor/iterator . StAX
‘pulls’ XML events, leaving it up to you to determine where in your program / data to receive the XML events. In general, StAX is as ef fecient as SAX.
#4. JAXB for object to XML and XML to Object mapping. V ery widely used in W eb services. The POJOs are annotated to map XML or JSON to object and
vice versa. MOXy implementation of JAXB is powerful as it supports XPath for mapping, and can be used for both XML and JSON.
Java 6 onwards, there is built in support for all the dif ferent types of parses listed above. In general, will be favoring StAX, and move to SAX if performance is
a real concern. When using OXM (Object T o Xml Mapping) then JAXB is used.

---

## Q9: In your experience, what are some of the widely used Unix commands as a Java developer?

**Answer:**

#1. Find with gr ep to sear ch for files with a certain text . For example, search all properties files where there are “inbox” search texts defined.
to list all the log files that have “job-no-300”
#2. Identify the jar file that has a particular class or r esour ce file . For example, to find the jar files that has MyConnection.class filer . Handy for identifying
class loading issues for batch job.$find . -type file -name "*.properties" | xargs grep inbox \{\}; #
$find . -type f -name "*.properties" -exec grep "inbox" {} /dev/null \;
grep -l "job-no-300" *.log

#3. Cr eating a symbolic link
Now , myapp.properties -> myapp-test1.properties. Handy when you have dif ferent environmental properties files, and can use a symbolic link to connect to
particular environment like test1, test2, test3, etc.
#4. Executing a shell command
nohup ensures that it runs even after you logout and “&” is for running in the background.
#5. history command to reuse some of the commands you already used.find . -name '*.jar' -print0 | xargs -0 -I '{}' sh -c 'jar tf {} | grep MyConnection .class
n -s myapp -test1 .properties myapp .properties
$nohup ./myapp .sh -env test1 -logile myapp .log &
$history
$history | grep mkdir
$!cd

#6. Checking a log file.
#7. Java pr ocess contr ol commands . Kill command to get Java thr ead dumps . Network stats, etc.
#8. ar chiving files in the current folder with tar . Transfer files with scp. Verify network connectivity with ping, etc$!35
$less -200 myapp .log
ail -f myapp .log
$ps -ef | grep java
$vmstat
$kill-3 12345 #kill a process with pid 12345
$netstat -a | grep 444
ar cvzf myapp .tgz . #z means gzip. c - create
ar xvzf myapp .tgz #untar and un gzip x - extract
scp myappl .tgz myuser @myserver .com:/home /myuser # copy to a server
scp myuser @myserver .com:/home /user/myapp .tgz . # copy from a server
$ping hostname
$telnet hostname 25 #hostname and port number
$wget http://www .myapp.com/downloads/script.txt

---

## Q10: In your experience, what libraries/frameworks have you used for the following scenarios

**Answer:**

1. RESTful web service : JAX-RS libraries Apache CXF , RESTEasy with MOXy JAXB implementation, Jersey , etc.
2. SOAP web service : JAX-WS libraries Apache CXF , Apache AXIS, etc.
3. Parsing JSON data : Jackson, or g.json, etc.
4. Generating r eports in PDF , CSV , XML, etc : JasperReports with iReport, OpenCSV , Apache FOP using XSL-FO, and XSL T to transform XML to HTML,
etc
5. Externalizing the business rules : Drools.
6. Converting Java domain objects to DT Os: Dozer
7. Performing code r eviews : Crucible
8. Performing continuous integration : Jenkins, Bamboo, etc
9. Performing ETL operations : Spring batch, IBM Datastage, etc
10. W riting technical specs : Confluence.
11. Drawing conceptual, UML, and ER diagrams : Glif fy diagrams, V isio diagrams, W indows snipping tool for screenshots, etc

---

## Q11: In your experience, what are some of the mistakes or slackness you have noticed regarding exception handling?

**Answer:**

#1. Use of System.out.println(…) and ex.printStackT race(…) as opposed to using the log4j framework with log.info(), etc.
#2. Ignoring the exceptions. In few rare scenarios, it is desired to do nothing with an exception, but in general this is a very bad practice and can hide issues.

#3. Inconsistent use of checked and unchecked (i.e. Runtime) exceptions . Document a consistent exception handling strategy . In general favor unchecked
(i.e. Runtime) exceptions, which you don’ t have to handle with catch and throw clauses. Use checked exceptions in a rare scenarios where you can recover from
the exception like deadlock or service retries. In this scenario, you will catch a checked exception like java.io.IOException and wait a few seconds as configured
with the retry interval, and retry the service configured by the retry counts.
#4. Not knowing the fact that the exceptions ar e polymorphic in nature and more specific exceptions need to be caught before the generic exceptions.
#5. W iping out the stack trace.
#6. Unnecessary Exception T ransformation.
In a layered application, many times each layer catches exception and throws new type of exception. Sometimes it is absolutely unnecessary to transform an
exception. An unchecked exception can be automatically bubbled all the way upto the GUI Layer , and then handled at the GUI layer with a log.error(ex) for the
log file and a generic info like “An expected error has occurred, and please contact support on xxxxx” to the user . Internal details like stack trace with host
names, database table names, etc should not be shown to the user as it can be exploited to cause security threats.
Data Access Layer –> Business Service Layer –> GUI Layer
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »ry{
 //....
}catch (SQLException sqe){
// do nothing. Exception is burried.
}
ry{
 //....
}catch (IOException ioe){
 //ioe stack is lost
 throw new MyException ("Problem in data reading." );
}

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
