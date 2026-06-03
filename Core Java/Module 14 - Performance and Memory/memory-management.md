# Memory Management

> **Module**: Performance and Memory  
> **Topic**: Memory Management

---

## 📋 Table of Contents



- [Q1: Are memory leaks possible in Java, which has memory management via automatic Gar](#q1)
- [Q2: What causes memory leaks in your Java applications?](#q2)
- [Q3: How will you go about creating a memory leak in Java?](#q3)
- [Q4: How will you fix the above memory leak?](#q4)
- [Q6: In real applications, how do you know that you have a memory leak?](#q6)
- [Q7: How will you go about profiling your Java application for memory usage?](#q7)
- [Q8: What causes the java.lang.OutOfMemoryError: PermGen space memory leak?](#q8)

---

## 🔹 Q1: Are memory leaks possible in Java, which has memory management via automatic Garbage Collection?

**Answer:**

Memory and resource leaks are possible in any robust application. In managed languages such as Java and C#, the developers do not have to worry too
much about memory management as the garbage collector (GC) will do this job for you. The garbage collectors can be tuned with different algorithms and hints
depending on the behavior of the application. This does not mean that the garbage collected languages are immuned from memory leaks. Memory leaks can
occur in managed languages when objects of longer life cycle (e.g. static or global variables, singleton classes, etc) hold on to objects of a shorter life cycle. This
prevents the objects with short life cycle being garbage collected. The developers must remember to remove the references to the short-lived objects from the
long-lived objects.

---

## 🔹 Q2: What causes memory leaks in your Java applications?

**Answer:**

— Long living objects having reference to short living objects, causing the memory to slowly grow. For example, singleton classes referring to short lived
objects. This prevents short-lived objects being garbage collected.
— Improper use of thread-local variables. The thread-local variables will not be removed by the garbage collector as long as the thread itself is alive. So, when
threads are pooled and kept alive forever, the object might never be removed by the garbage collector .
— Using mutable static fields to hold data caches, and not explicitly clearing them. The mutable static fields and collections need to be explicitly cleared.
— Objects with circular or bidirectional references that are accessible from a thread.
— JNI memory leaks.

---

## 🔹 Q3: How will you go about creating a memory leak in Java?

**Answer:**

In Java, memory leaks are possible under a number of scenarios. Here is a typical example where hashCode( ) and equals( ) methods are not implemented
for the Key class that is used to store key/value pairs in a HashMap. This will end up creating a large number of duplicate objects. All memory leaks in Java end
up with java.lang.OutOfMemoryError, and it is a matter of time. The following code agressively creates the OutOfMemoryError via an endless loop for
demonstration purpose.
If you are not familiar with the significance of equals( ) and hashCode ( ) methods in Java learn how to define proper key class in Java.
mport java.util.HashMap ;
mport java.util.Map;
public class MemoryLeak {

public static void main (String [] args) {
 Map<Key, String > map = new HashMap <Key, String >(1000 );
 int counter = 0;
 while (true) {
 // creates duplicate objects due to bad Key class
 map.put(new Key("dummyKey" ), "value" );
 counter ++;
 if (counter % 1000 == 0) {
 System.out.println ("map size: " + map.size());
 System.out.println ("Free memory after count " + counter
 + " is " + getFreeMemory () + "MB" );
 
 sleep (1000 );
 }
 }
/ inner class key without hashcode() or equals() -- bad implementation
tatic class Key {
 private String key;
 public Key(String key) {
 this.key = key;
 }
/delay for a given period in milli seconds
public static void sleep (long sleepFor ) {
 try {
 Thread .sleep (sleepFor );
 } catch (InterruptedException e) {
 e.printStackT race();
 }
/get available memory in MB
public static long getFreeMemory () {
 return Runtime .getRuntime ().freeMemory () / (1024 * 1024 );

Output:
As you could see, the size of the map keeps growing with the same objects and the available memory keeps coming down from 4MB to 0MB. At the end, the
program dies with an OutOfMemoryErr or.

---

## 🔹 Q4: How will you fix the above memory leak?

**Answer:**

By providing proper implentation for the key class as shown below with the equals() and hashCode() methods.map size: 1000
Free memory after count 1000 is 4MB
map size: 2000
Free memory after count 2000 is 4MB
map size: 1396000
Free memory after count 1396000 is 2MB
map size: 1397000
Free memory after count 1397000 is 2MB
map size: 1398000
Free memory after count 1398000 is 2MB
map size: 1399000
Free memory after count 1399000 is 1MB
map size: 1400000
Free memory after count 1400000 is 1MB
map size: 1401000
Free memory after count 1401000 is 1MB
....
....
map size: 1452000
Free memory after count 1452000 is 0MB
map size: 1453000
Free memory after count 1453000 is 0MB
Exception in thread "main" java.lang.OutOfMemoryError : Java heap space
at java.util.HashMap .addEntry (HashMap .java:753)
at java.util.HashMap .put(HashMap .java:385)
at MemoryLeak .main (MemoryLeak .java:10)

---

## 🔹 Q6: In real applications, how do you know that you have a memory leak?

**Answer:**

If you profile your application, you can notice a graph like a saw tooth. Here is how you can determine this with the help of jconsole for the above bad key
class example. All you have to to do is while your MemoryLeak class is running, get the Java process id by typingtatic class Key {
 private String key;
 public Key(String key) {
 this.key = key;
 }
 @Override
 public boolean equals (Object obj) {
 if (obj instanceof Key)
 return key.equals (((Key) obj).key);
 else
 return false ;
 }
 @Override
 public int hashCode () {
 return key.hashCode ();
 }
C:\>jps
5808 Jps
4568 MemoryLeak
3860 Main

Now, open up the jconsole as shown below on a command line
C:\>jconsole 4568

Memory Leak

---

## 🔹 Q7: How will you go about profiling your Java application for memory usage?

**Answer:**

There are number of tools both commercial and open-source. There are command line tools that get shipped with Java like hprof, jconsole, jhat, and
jmap. Visual VM is a graphical tool shipped with Java 6 on wards.
There are commercial tools that can be used in production environment like YourKit for Java, JProfiler for Java, etc and for larger distributed and clustered
systems with large number of nodes there are profilers like CA W iley Introscope for Java, ClearStone for Java, and HP Performance managers .

---

## 🔹 Q8: What causes the java.lang.OutOfMemoryError: PermGen space memory leak?

**Answer:**

Every JVM nowadays uses a separate region of memory, called the Permanent Generation (or PermGen for short), to hold internal representations of java
classes entailing the fields and more.
— Methods of a class (including the bytecodes)
— Names of the classes (in the form of an object that points to a string also in the permanent generation)
— Constant pool information (data read from the class file, see chapter 4 of the JVM specification for all the details).
— Object arrays and type arrays associated with a class (e.g., an object array containing references to methods).
— Internal objects created by the JVM (java/lang/Object or java/lang/exception for instance)
— Information used for optimization by the compilers (JIT s)
The diagram below shows the PermGen space with “ Perm ”
Java heap sections

The PermGen space memory leak can be caused by Leaking Threads, Leaking Drivers, and not allocating enough PermGen Space via JVM config.
One possible scenario for a classloader leak is through long running thr eads. This happens when your application or or a 3rd party library used by your
application starts some long running thread. An example of this could be a timer thread whose job is to execute some code periodically .
Another typical case of a leak can be caused by database drivers.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03


---

## 📚 Related Topics

- [Java Overview](../Module%2001%20-%20Java%20Overview/)
- [Java Data Types](../Module%2002%20-%20Java%20Data%20Types/)
- [OOP Concepts](../Module%2006%20-%20OOP%20and%20FP/)

---

## 💡 Key Takeaways

Review the questions above and ensure you understand:
- Core concepts and their practical applications
- Real-world scenarios and use cases
- Best practices and common pitfalls

---

**[⬆ Back to Top](#)**