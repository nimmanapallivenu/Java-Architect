# 48. jvisualvm to sample Java heap memory   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

Java does not have a sizeof operator like C++ does. Java uses automatic memory management known as the Garbage Collection, hence it is not that important to evaluate
size of various objects. But, for the purpose of learning & fixing any potential memory issues, I have used “jvisualvm”, which is a very handy & free profiling tool that
gets shipped with the JDK. This compliments Java primitives & objects – memory consumption interview Q&A
Step 1: Java code to “sample with jvisualvm”
Never ending while loop is used so that the application stay alive to sample the Java memory to see how much memory does “MyW rapper” object occupy .
Step 2: Start jvisualvm
Run the above code as a stand-alone Java application:mport java.util.concurrent .TimeUnit ;
public class ObjectSize {
 
 public static void main (String [] args) throws InterruptedException {
 
 MyW rapper five = new MyW rapper (5);
 
 while (true) {
 TimeUnit .SECONDS .sleep (10);
 System .out.println (five);
 }
 }
 
 //inner class
 static class MyW rapper {
 int number ; // 4 bytes each
 public MyW rapper (int number ) {
 this.number = number ;
 }
 }

1) jps will give the process id of the
2) jvisualvm will start the profiler that is shipped with JDK.
1208 is the pid (i.e process id) of the JVM in which “ObjectSize” is running.
Step 3: jvisualvm GUI opens up as shown below$ jps
247
1208 ObjectSize
1209 Jps
$ jvisualvm

VisualVM
You can see ObjectSize with pid 1208.

Step 4: jvisualvm tabs
Double click on “ ObjectSize with pid 1208.” You will get the following screen, and select the “Sampler” tab.
jvisualvm sampler

Step 5: jvisualvm memory sampling
Click on the “memory” button,

jvisualvm histogram
Filter “MyW rapper” by typing it at the bottom

MyW rapper Object on the JVM heap
Step 6: Why 16 bytes when primitive int data is only 4 bytes?
The Object metadata (aka header information) consumes memory in the heap as described below
1) Class information: 32 bits = 4 bytes .
2) Flags : array or not, hashCode, etc : 32 bits = 4 bytes .
3) Lock information: synchronization 32 bits = 4 bytes .
int number = 32 bits = 4 bytes .
So, total 12 bytes of meta data + 4 bytes of data = 16 bytes .
How about an array that can hold 10 MyWrapper objects
mport java.util.concurrent .TimeUnit ;
public class ObjectSize {
 
 public static void main (String [] args) throws InterruptedException {
 
 MyW rapper [] five = new MyW rapper [10];
 five[0] = new MyW rapper (0);
 
 while (true) {
 TimeUnit .SECONDS .sleep (10);
 System .out.println (five);
 }
 }
 
 static class MyW rapper {
 int number ;
 public MyW rapper (int number ) {
 this.number = number ;
 } 
 }

How much memory does the above MyWrapper[ ] take?
Follow the same steps as above.
MyW rapper[ ] heap histogram
1) The “MyW rapper” object takes “16 bytes” as before
2) The array MyW rapper [ ] takes 4 bytes * 10 = 40 bytes for 10 elements.
The remaining 16 bytes are for the Object meta data (aka array header information).

1) Class information: 32 bits = 4 bytes .
2) Flags: array or not, hashCode, etc: 32 bits = 4 bytes .
3) Lock information: synchronization: 32 bits = 4 bytes .
4) Size of the array 32 bits = 4 bytes .
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »



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

