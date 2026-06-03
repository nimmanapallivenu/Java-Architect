# 02  jvisualvm to detect memory leak   a quick tutorial style Java demo   Java Success.com

## Table of Contents

This is a companion post to 8 Java Memory Management Interview Q&A demonstrating a memory leak scenario step by step with jvisualvm, which is a free profiling tool that gets shipped with JDK.
Step 1: Code that causes memory leak
Here is a sample code that causes memory leak. It is shown with a never ending while loop for the demo purpose, but in a real production application this could be a logic within a method that gets accessed
very frequently .
In the above code the “Key” class that is used for storing values into a map is not properly implemented by overriding equals() & hashCode(), hence it will be using Object class’ s implementation which uses
the memory location of each new object created.
Step 2: Start jvisualvmmport java.util.HashMap ;
mport java.util.Map;
mport java.util.concurrent .TimeUnit ;
public class MemoryLeakDemo {
 public static void main (String [] args) throws InterruptedException {
 Map<Key, String > map = new HashMap <Key, String >(1000 );
 int counter = 0;
 while (true) {
 // creates duplicate objects due to bad Key class
 map.put(new Key("dummyKey" ), "value" );
 counter ++;
 if (counter % 1000 == 0) {
 System .out.println ("map size: " + map.size());
 TimeUnit .SECONDS .sleep (2);
 }
 }
 }
 // inner class key without hashcode() or equals() -- bad implementation
 static class Key {
 private String key;
 public Key(String key) {
 this.key = key;
 }
 }

Step 3: Run the code & monitor jvisualvm
Monitoring the heap – when started running the application
After 35 minutes:
Monitoring the JVM heap after 35 minutes
As, you can see the JVM heap memory usage is keep going up, The “saw tooth” like diagram shown above indicates memory leak. The memory used has gone up from 23MB to 43MB within 35 minutes.$ jvisualvm

Step 4: Uncontrolled creation of the instances of Key class is the culprit
As you can see 714K instances created at this point.
Sampling the JVM heap
Step 5: How to fix the code?
Implement the hashCode() & equals() method to the “Key” class and run the code and profile with jvisualvm.
mport java.util.HashMap ;
mport java.util.Map;
mport java.util.Objects ;
mport java.util.concurrent .TimeUnit ;
public class MemoryLeakDemo {
 public static void main (String [] args) throws InterruptedException {
 Map<Key, String > map = new HashMap <Key, String >(1000 );
 int counter = 0;
 while (true) {
 // creates duplicate objects due to bad Key class
 map.put(new Key("dummyKey" ), "value" );
 counter ++;
 if (counter % 1000 == 0) {
 System .out.println ("map size: " + map.size());
 TimeUnit .SECONDS .sleep (2);
 }
 }
 }

Step 6: jvisualvm sampling after fixing the code
Even though the instances count shows 13,000 instances, it was because the GC has not been kicked in yet. Click on the “Perform GC” button a few times and you will see the count go down to 1. // inner class key without hashcode() or equals() properly implemented with Java 8 "Objects" class
 static class Key {
 private String key;
 public Key(String key) {
 this.key = key;
 }
 @Override
 public int hashCode () {
 return Objects .hash(key); // Java 8 Objects class
 }
 @Override
 public boolean equals (Object obj) {
 if (obj == null) {
 return false ;
 }
 if (getClass () != obj.getClass ()) {
 return false ;
 }
 Key other = (Key) obj;
 return Objects .equals (this.key, other .key); // Java 8 Objects class
 }
 }

JVM heap sampling
Step 7: jvisualvm heap memory monitoring after fixing the code

Heap Memory Monitoring
As you can see, the memory usage is fully under control without any leaks. The key objects created in the while loop periodically gets garbage collected.



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
