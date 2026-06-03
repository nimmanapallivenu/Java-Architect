# 02  15 key considerations for low latency Java   Java Success.com

## Table of Contents

Extends Writing low latency applications in Java interview Q&As . If the job description says “ low latency ” application, then be prepared. Even if “low latency” experience is not a requirement, all employers
like candidates who can not only build systems that perform well, but also can stress test (e.g. JMeter), profile (e.g. jvisualvm ) & performance tune.
Tip #1 : Use a RTSJ (Real T ime Specification for Java ) JVM. IBM, Oracle, and other smaller vendors have implemented this, but it comes at a cost. Oracle’ s JavaR T, IBM’ s real-time W ebSper e, and aicas
JamaicaVM to name a few popular ones. In real time JVM, instead of writing java.lang.Thr ead you just have to write, javax.r ealtime.RealtimeThr ead. Azul Zing is another JVM for a more predictable
performance.
Tip #2: Big O notation for algorithms : Ensure all your data structures related algorithms are O(1) or at least O(log n). This is probably the biggest cause of performance issues. Make sure that you have
performance tests with real size data. Also, make sure that your algorithms are cache friendly . It is imperative to use proper cache strategies to minimize garbage collection pauses by having proper cache expiry
strategy , using weak references for cache, reducing cache by carefully deciding what to cache, increasing the cache size along with the heap memory to reduce object eviction from cache, etc. Understanding
Big O notations through Java examples
Tip #3 : Lock fr ee algorithms . Use lock free algorithms and non blocking I/Os. Even the most well designed concurrent application that uses locks is at risk of blocking. The java.util.concurr ent package
allows concurrent reads (e.g. ConcurrentHashmap). Lock free algorithms like CAS (Compare And Swap) can improve performance. 5 Java Concurrency interview Q&As .
It is important to select the right language like Java, Scala, C++1 1 or Go, which support a strong memory model to enable lock free programming. Though the scripting languages are getting faster , every
millisecond matters in building a low latency application and cannot af ford to have the overhead of an interpreted language.
The Java NIO (i.e.New I/O) supports non-blocking I/Os. 15 Java old I/O and NIO (i.e. New I/O) interview Q&As
Blocking is not good for low latency applications. Minimize context switching among threads by having threads not more than the number of CPU cores in your machine.
Tip #4 : Keep it all in memory & colocate data & computation : Disk & network I/O will adversely impact latency . It is imperative to manage the data in memory via Cacheing. A persistent event
logs need to be maintained to rebuild the state after a machine or process restart. Y ou can also use an in-memory database like Redis or MongoDB, but beware that you can loose some data on crash due to
background syncing of data to disk from memory .
Even though network hops are faster than disk seek, network I/O do add to the overhead. When compute your data in a distributed manner using frameworks lik Spark, Kafka, etc, partition your data properly to
minimise any shuf fling across various nodes.
Tip #5 : Reduce memory size : Reduce the number of objects you create. Apply the flyweight design pattern where applicable. Favor stateless objects. Where applicable write immutable objects that can be
shared between threads. Fewer objects mean lesser GC.
Tip #6: Tune your JVM : Tune your JVM with appropriate heap sizes and GC configuration. Before tuning profile your application with real life data. Basically you want to minimize GC pause durations
and increase GC thr oughput . GC throughput is a measure of % of time not spent on GC over a long period of time. Specialist GC collectors like the Azul collector can in many cases solve this problem for
you out of the box, but for many you who use the Oracle’ s GC, you need to understand how GC works and tune it to minimize the duration of the pauses. The default JVM options optimize for throughput, and
latencies could be improved by switching to the Concurr ent Garbarge Collector .
GC tuning is very application specific. It is imperative to understand how your application uses the garbage collection. Memory is cheap and abundant on modern servers, but garbage collector pauses is a
serious obstacle for using lar ger memory sizes. Y ou should configure the GC to minimize the number & duration of the pauses.
Enable diagnostic options (-XX:+PrintGCDetails -XX:+PrintT enuringDistribution -XX:+PrintGCT imestamps).
Decide the total amount of memory you can af ford for the JVM by graphing your own performance metric against young generation sizes to find the best setting.
Make plenty of memory available to the younger (i.e eden) generation. The default is calculated from NewRatio and the -Xmx setting.
Make the survival space to be same size as Eden (-XX:SurvivorRatio=1) and increase new space to account for growth of the survivor spaces (-XX:MaxNewSize= -XX:NewSize= ) 
Larger younger generation spaces increase the spacing between full GCs. But young space collections could take a proportionally longer time. In general, keep the eden size between one fourth and one
third the maximum heap size. The old generation must be lar ger than the new generation.
Tip #7 : Favor primitives to wrapper classes to eliminate auto-boxing and un-boxing , especially in situations where the getter and setter methods are called very frequently for the wrapper classes
like Integer , Float, Double, etc the performance is going to be adversely impacted due to auto boxing and unboxing. The operations like x++ will also provide poor performance if x is an Integer and not an
int. So, avoid using wrappers in performance critical loops. Java primitives & objects – memory consumption interview Q&As

Tip #8 : Good caching strategy and applying the short-cir cuit pattern
Short-circuit pattern is handy for I/O related patterns like socket or URL based, database operations, and complex File I/O operations. I/O operations need to complete within a short amount of time, but with
low latency W eb sites, the short-cir cuit pattern can be applied to time-out long running I/O tasks , and then can either display an error message or the show cached r esults .
Tip #9 : Coding best practices to avoid performance issues due to death by 1000 cuts.
When using arrays it is always ef ficient to copy arrays using System.arraycopy( ) than using a loop.
When using short circuit operators place the expression which is likely to evaluate to false on extreme left if the expression contains &&.
Do not use exception handling inside loops.
Avoid using method calls to check for termination condition in a loop.
Short-circuit equals( ) in lar ge object graphs where it compares for identity first
Tip #10 : Experience and knowledge with some of the libraries like
NIO-based scalable server applications by directly using java.nio package or framework like Apache MINA, Netty , Grizzly , etc.
Actor based concurrency frameworks like Akka . Concurrency options in terms of increasing abstractions: threads –> Executors –> Fork/Join –> Actor model.
FIX protocol and commercial FIX libraries like Camer on FIX .
Use Java 5 concurrency utilities, and locks.
Lock free Java disruptor library for high throughput.
Chronicle Java library for low latency and high throughput, which almost uses no heap, hence has trivial impact on GC.
Trove collection libraries for primitives. Alternative for the JDK wrapper classes like java.lang.Integer for primitives requiring less space and providing better performance.
Javolution library with real-time classes. For example, Javolution XML provides real-time marshaling and unmarshaling.
These libraries are aimed at providing reduced memory size, less impact on GC, lock free concurrent processing, data structure algorithmic ef ficiency , etc.
Tip #1 1: Sequence your reads & batch your writes: Storage systems like rotational disk, flash based disk, or memory perform better when the data is sequentially read as opposed to randomly read.
Sequential reads to memory trigger the use of pre-fetching at the RAM & CPU cache level. One way to achieve this is by using an array of primitive values.
Batch writes are more ef ficient as they perform multiple operations with the same overhead as a single operation. Each write will batch all the data that arrived since the last write was issued.
Tip #12: How is your data stor ed? Are you using a SQL database? How will that scale? Can you use a NoSQL data store instead. Transactional systems need SQL for transaction demarcation.
Relational and NoSQL data models are very dif ferent.
SQL Model:@Override
public boolean equals (Object other ) {
 if (this == other ) return true;
 if (other == null) return false ;
 // Rest of equality logic...
}

The relational model takes data and store them in many normalized interrelated tables that contain rows and columns. T ables relate with each other through foreign keys. When looking up data, the desired
information needs to be collected by joining many related tables and combined before it can be provided to the application.
NoSQL Model 
NoSQL databases have a very dif ferent model. NoSQL databases have been built from the ground up to be distributed, scale-out technologies and therefore fit better with the highly distributed nature of the
three-tier Internet architecture. A document-oriented NoSQL database takes the data you want to store and aggregates it into documents using the JSON format. Each JSON document can be thought of as an
object to be used by your application. This might relate to data aggregated from 10+ tables in an SQL model.
Tip #13 : Pay attention to network round trips, payload sizes and type, protocols used, service timeouts and retries.
Tip #14 : What volume of data are you dealing with? If you are using Gigabytes to terabytes of data, you are entering the space of big data and need to start thinking about
1) DFS (Distributed File Systems), e.g. HDFS (Hadoop Distributed File System) where your 250GB input file can be split into 50 or more input splits and stored in 10 or more commodity hardware (normal
cheap hardware systems). Y ou can use cheap hardware because the same data is split and replicated across multiple systems as the data nodes . The “ name node ” keeps the meta data of what split data node is
stored in which systems.
2) Map Reduce concept where “50 input splits” will have 50 mappers mapping data as key/value pairs on 50 dif ferent “data nodes”. The mapped “key/value” data is passed to the “ reducers ” to aggregate the
data and produce the output files . Unlike the number of mappers, the number of reducers will depend on the number of output files to be produced. If it produces 1 output file, then there will be only one
“reducer”.
Tip #15 : WebSockets is a bidirectional communications channel as opposed to HTTP , which is unidirectional. Once the connection is established, W ebSocket data frames can be sent back and forth between
the client and the server in full-duplex mode via a single socket (i.e. same TCP connection for the lifecycle of W ebSocket connection). In HTTP , typically a new TCP connection is initiated for a request and
terminated after the response is received.
HTTP protocol is not only more verbose (e.g. require headers), but also either client can talk to server or server can talk to client, whereas W ebSockets are duplex in nature allowing client and server to talk
independent of each other . When one side closes the channel, the connection closes.
Related links
1. Java Garbage Collection interview questions & answers to ascertain your depth of Java knowledge

jconsole memory graph – with memory leak
2. jvisualvm to detect memory leak – a quick tutorial style Java demo

JVM heap sampling



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
