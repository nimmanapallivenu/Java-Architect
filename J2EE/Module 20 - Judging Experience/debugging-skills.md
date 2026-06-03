# Debugging Skills

> **Module**: Judging Experience  
> **Topic**: Debugging Skills

---

## 📋 Table of Contents



- [Q1: How would you go about debugging thread-safety or concurrency issues?](#q1)
- [Q2: How will go about remotely debugging Java?](#q2)
- [Q4: How will you go about profiling the JVM for memory and CPU usage?](#q4)
- [Q5: How will you go about debugging web services and web applications?](#q5)
- [Q6: How will you go about debugging Hibernate or other ORM tools?](#q6)
- [Q7: What is a JAR hell issue, and how will you go about resolving it?](#q7)

---

## Q1: How would you go about debugging thread-safety or concurrency issues?

**Answer:**

#1: Manually r eviewing the code for any obvious thread-safety issues. Good knowledge of multi-threading is required.
#2: List all possible causes and add extensive log statements and write test cases to prove or disprove your theories. The log statements will have something
like
While debugging java program if you want to copy the stack of a thread which hit the break point and suspended you do so by “Copy Stack” option.
#3: Using your IDE debugging capability by setting a conditional br eak point. For example, in Eclipse IDE, you can add a condition like
Thread.currentThread().getName().equals(“Thread-0”).
og.info(Thread .currentThread ().getName () + " produced: " + count ); 
System.out.println (Thread .currentThread ().getName () + " consumed: " + consumed );

Eclipse Debug – Conditional
#4: Thr ead dumps are very useful for diagnosing synchronization problems such as deadlocks. The trick is to take 5 or 6 sets of thread dumps at an interval of
5 seconds between each to have a log file that has 25 to 30 seconds worth of run-time action. For thread dumps, use kill -3 in Unix and CTRL+BREAK in
Windows. There are tools like Thread Dump Analyzer (TDA), Samurai, etc. to derive useful information from the thread dumps to find where the problem is.
For example, Samurai colors idle threads in grey, blocked threads in red, and running threads in green. Y ou must pay more attention to those red threads.
Creating a thread dump in windows
Step 1 : Note down the process id: 4800. Connect to 4800, and
jconsole
Step 2: You can detect any deadlocks by clicking on the “Detect Deadlock” button in the threads tab.

Step 3: To get a thread dump, open a DOS command prompt and type jstat [pid]
This will produce a stack trace. The stack trace looks something likestat 4800

Thread Dump
You need to pay attention to blocked threads, and there are tools like Thread Dump Analyzer (TDA), Samurai, etc to analyze thread dumps.
jstack and jconsole are provided with your JDK installation under jdk[version]/bin.
#5: Ther e are static analysis tools like Sonar, Thr eadSafe, etc for catching concurrency bugs at compile-time by analyzing the byte code. Sonar produces
reports with recommendations.

---

## Q2: How will go about remotely debugging Java?

**Answer:**

It is increasingly essential with the globalization to be able to debug a Java application that is deployed remotely, in another country or city. You will come
across scenarios where an application might be running fine in your sandbox (i.e. local desktop), but might be buggy when running in another environment or
country .
Say you want to remotely debug an application called MyApp.jar that is running remotely, you can set up your desktop to be able to debug it by enabling the
remote debugging as shown below:
The above command tells the MyApp.jar to start a server socket on port 888, and publish the debugging messages using the jdwp, which stands for Java Debug
Wire Protocol. The IDEs like eclipse can be configured to tap in to remote debugging as shown below .
In eclipse, select Run –> Debug Configurations. Right click on “Remote Java Application”, and select “New”. Fill in the config details as shown below and
apply the changes. Click on the “Debug” button to start debugging the remote application within eclipse. Y ou will have to attach the source files from within
eclipse.ava -Xdebug -Xrunjdwp :transport =dt_socket ,address =888,server =y -jar MyApp .jar

Remote Debugging
The “diagnostic-core” is the project with the source files within eclipse. The “MyApp” is the remote debug configuration that can be stopped and started within
eclipse. The connection properties provide information as to where to look for debug messages exposed via jdwp.

Q3 What are the dif ferent debugging options your IDEs like eclipse provide?
A3.
#1. You can get the debugger to stop at a particular exception. For example, stop at NullpointerException .
#2. You can add break points and inspect values at run time or modify values.
#3. You can add conditional break points to when thread-0 is running or balance > 500.00.
#4. You can add a watch on variables. Y ou could even add expressions to stop when a variable has a particular value like null. As you step through one line at a
time, you can see the watch values changing.

---

## Q4: How will you go about profiling the JVM for memory and CPU usage?

**Answer:**

#1. V isualVM is a visual tool integrating several commandline JDK tools and lightweight profiling capabilities. Designed for both production and development
time use, it further enhances the capability of monitoring and performance analysis for the Java SE platform. Y ou can start the visual vm by double clicking on
%JAVA_HOME%/bin/jvisualvm.exe from Java 1.6 version onwards.
#2. There are number of tools both commercial and open-source. There are command line tools that get shipped with Java like hprof, jconsole, jhat, and jmap .
Here is an example with hprof.
When you run the above command in binary format (i.e. format=b), it produces a heap dump file called “java.hprof” when the program exits or a control
character (Ctrl-\ or Ctrl-Br eak on WIN32 and QUIT signal is received kill -QUIT on Unix machines) is pressed, which can be opened in eclipse memory
analysis tool (MA T) for further analysis and leak detection.ava -agentlib :hprof =heap =all,thread =y,format =b test.ProfilingT est

Eclipse Memory Analyzer
A Java heap dump is an image of the complete Java object graph at a certain point in time. It includes all objects, fields, primitive types and object references.
It is possible to instruct the JVM to create a heap dump automatically in case of a OutOfMemoryError with the JVM option -
XX:+HeapDumpOnOutOfMemoryErr or. You could also dump the heap on ctrl+brk key stroke by setting the JVM argument -
XX:+HeapDumpOnCtrlBr eak.
The above heap dump can also be analyzed instantly with the jhat tool that is shipped with your java.

The above command starts a web server, and the report can be viewed via an internet browser using http://localhost:7000.
Heap Profiling Reporthat java.hprof

#3. Memory is cheap and abundant on modern servers, but garbage collector pauses is a serious obstacle for using larger memory sizes. You should configure
GC to gther some stats by enable diagnostic options -XX:+PrintGCDetails -XX:+PrintT enuringDistribution -XX:+PrintGCT imestamps .
#4.The following example uses an hpr of tool that comes with Java to determine the cpu times. The profiles are dumped to “java.hprof.txt” file when the
program exits or a control character Ctrl-\ or Ctrl-Br eak (WIN32) depending on platform is pressed. On Solaris OS and Linux a profile is also generated when
a QUIT signal is received ( kill -QUIT ). The process id can be determined with the “jps” command. The command shown below executes the “ProfilingT est”
class with the hprof agent for measuring CPU times.
The profiled results will be dumped to a file named “java.hprof.txt”.
PerfAnal is a GUI based analysis tool to analyze the above results.ava -agentlib :hprof =cpu=times test.ProfilingT est
CPU TIME (ms) BEGIN (total = 1953 ) Thu Sep 15 12:26:50 2011
rank self accum count trace method
 1 51.20 % 51.20 % 1 301025 ProfilingT est.invokeMethod3
 2 31.23 % 82.44 % 1 301015 ProfilingT est.invokeMethod2
 3 15.21 % 97.64 % 1 301005 ProfilingT est.invokeMethod1
 4 0.82% 98.46 % 1 300396 java.net.URLClassLoader $1.run
 5 0.77% 99.23 % 2 300707 java.io.Win32FileSystem .normalize
 6 0.77% 100.00 % 268 300309 java.lang.StringBuilder .append
CPU TIME (ms) END

Performance Analysis
#5. When profiling, you need to create pr oper load with concurr ent thr eads. You can use tools like JMeter create scripts that apply load. The performance,
memory leak, and concurrency issues usually surface under load.ava -jar PerfAnal .jar java.hprof .txt

#6. Metrics by Yammer provides runtime metrics and statistics for all kind of applications. Y ou can measure request/response cycles of web apps and provide
histograms of the measured values. It can output results to log4j, JMX, JSON, console, CSV, and more.
#7. You can also gather stats by writing your own AOP or dynamic pr oxy based utilities. For example,
mport java.util.Arrays ;
mport org.aspectj .lang.ProceedingJoinPoint ;
mport org.aspectj .lang.Signature ;
mport org.aspectj .lang.annotation .Around ;
mport org.aspectj .lang.annotation .Aspect ;
mport org.springframework .stereotype .Component ;
@Aspect
@Component
public class PerformanceProfilingAspect {
@Around ("execution( * com.myapp.*.* (..) )" )
public Object invoke (final ProceedingJoinPoint pjp) throws Throwable {
Signature signature = pjp.getSignature ();
Object [] args = pjp.getAr gs();
String argList = Arrays .toString (args);
System.out.println ("Start executing: " + signature .getDeclaringT ypeName () + "." + signature .getName () + "(" + argList + ")");
long s = System .nanoT ime();
//invoke the actual method
Object proceed = pjp.proceed (args);
long e = System .nanoT ime();
System.out.println ("Completed after: " + signature .getDeclaringT ypeName () + "." + signature .getName () + "(" + argList
 + ") ended after " + ((double ) (e - s)/1000000 ) + "ms" );
return proceed ;

---

## Q5: How will you go about debugging web services and web applications?

**Answer:**

A W eb service makes things slightly harder and often these services are deployed inside a container such as Apache T omcat. So, debugging involves 1)
client side, 2) messages sent across the wire 3) server side.
Client side
#1. Browser debugging tools like Firefox plugin Firebug, Google chrome tools/developer tools, and Internet explorer Firebug lite to debug JavaScripts, style
sheets (i.e. CSS), request parameters, DOM tree, etc.
#2. SoapUI and Firefox poster plugin are handy to send W eb service requests to the server .
Messages sent acr oss
#1. Web debugging tools like Fiddler2 for IE, Firebug NET tab for Firefox, JMeter HTTP proxy server, Charles web debugging, etc act as a proxy to capture
request parameters, request/response content, headers, cookies, etc for debugging purpose.
Debugging messages and payloads via proxies
#2. Network packet snif fing tools like Wireshark, Kismet, tcpdump, etc for analyzing the network protocols & issues. Developing applications that interact
with web services presents a unique set of problems like not knowing exactly what message was sent to the server, or what response was received. Some of the
most dif ficult bugs to track down are caused by a disconnect between what you think you are sending to the server, and what is actually going across the wire.
These tools are commonly called “packet snif fers” and capture all network packets that move across your network interface. Examining the contents of these
packets and the order in which they were sent and received can be a useful debugging technique.

Wire snif fing tools
#3. TCPMon comes as another option to capture the outgoing and incoming SOAP envelopes.
Server side
#1. Remote Java debugging as described above in

---

## Q6: How will you go about debugging Hibernate or other ORM tools?

**Answer:**

#1. Turning on the showSQL option to log generated SQL statements.
#2. SQL proxy drivers like P6SPY, log4jdbc, etc to log generated SQL statements, execution times, etc.
Sniffing SQL
#3. Run you SQL via any DB tool query planner for its ef ficiency .

---

## Q7: What is a JAR hell issue, and how will you go about resolving it?

**Answer:**

An industrial strength Java project will have 100+ jar files. How often have you come across a Java application that requires dif ferent versions of the same
library? How often do you see exceptions like NoSuchMethodErr or or IllegalargumentException. Here are some tips to solve the JAR hell problem.
#1. In Maven, due to its transitive dependencies behavior, multiple versions of same jar could be pulled in. Y ou need to determine which jar is bringing in the
this duplicate or wrong version of the jar and exclude it in the pom.xml file. The verbose flag instructs the dependency tree to display conflicting dependencies
that were omitted from the resolved dependency tree. For example, to see why commons-io 2.0 was chosen over commons-io 2.4.

The 3 handy commands to solve jar hell issues in maven are mvn dependency:tr ee, mvn dependency:analyze, and mvn help:effective-pom. The IDEs like
eclipse provide tools to analyze dependencies. In eclipse, double click on a pom.xml file, and then select the “Dependency Hierarchy” tab to analyze why a
particular jar was chosen.
#2. Determining, which jars has a particular “Class” file. Static analysis. Unix command “ find” and grep
In W indows or DOS
#3. To identify from which jar a particular class was loaded from, add the following snippet of code to a location where it gets executed. Run-time analysis. At
run time it will print the jar file from which “FileUtils” was loaded.mvn dependency :tree -Dverbose -Dincludes =commons -io
find ./lib -name "*.jar" -exec sh -c 'jar -tf {}|grep -H --label {} ' org.apache .commons .io.FileUtils '' \;
for %i in (*.jar) do @jar tvf %i | find "org/apache/commons/io/FileUtils.class"
mport java.security .CodeSource ;
public class WhichJarLoadedT est {
public static void main (String [] args) {
Class klass = org.apache .commons .io.FileUtils .class ;

#4. Go to findJAR.com and search for the class file. For example, I want to find the jar file that has or g.apache.commons.io.FileUtils .
findjar .com
You can drill through to find a Maven download link.CodeSource codeSource = klass .getProtectionDomain ().getCodeSource ();
if (codeSource != null) {
 System.out.println (codeSource .getLocation ());
}

#5. uber -jar is an “ over-jar“, and uber is the German word for above or over. uber -jar is defined as one that contains both your package and all its dependencies
in one single JAR file (Note: jars cannot have other jars). The advantage is that you can distribute your uber -jar and not care at all whether or not dependencies
are installed at the destination, as your uber -jar actually has no dependencies. Maven has a plugin known as the “Apache Maven Shade Plugin “.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03