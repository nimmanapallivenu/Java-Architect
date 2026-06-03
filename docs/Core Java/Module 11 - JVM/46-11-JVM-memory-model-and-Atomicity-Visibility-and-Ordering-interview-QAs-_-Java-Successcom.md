# 46. 11 JVM memory model and Atomicity Visibility and Ordering interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: Why is it important to understand the difference between the “ JVM memory model](#q1)
- [Q2: What are the 3 common causes of concurrency issues in a multi-threaded applicati](#q2)
- [Q3: Is the following code atomic ?](#q3)
- [Q4: How will you fix the above “atomicity” issue due to concurrent access?](#q4)
- [Q5: Is the following code atomic ?mport java.util.concurrent .atomic .AtomicInteger ](#q5)
- [Q6: Is there anything wrong with the following code?](#q6)
- [Q7: What is a volatile key word in Java?](#q7)
- [Q8: How does volatile keyword differ from the synchr onized keyword? ★](#q8)
- [Q9: Is there anything wrong with the following code?/...
public void process (){
 wh](#q9)
- [Q10: What do you understand by the term “ Compar e-And-Swap ” approach used by the at](#q10)
- [Q11: What is the downside of CAS approach?](#q11)

---

## 🔹 Q1: Why is it important to understand the difference between the “ JVM memory model ” and the computer “hardware memory model”?

**Answer:**

3 reasons why it is important to understand the JVM memory model and computer hardware memory model are:
1) JVM memory model and the physical hardware memory model architectures are different. The hardware memory architecture does not distinguish between thread stacks
and heap. The hardware memory model has global RAM memory and local CPU registers & cache memory for each thread.
2) Without synchronization or volatile keyword declaration compiler and CPU processors are allowed to reorder reads and writes as an optimization causing race conditions.
Volatile or synchronized reads/writes cannot be reordered. There may be limited special conditions to reorder inside the synchronized code.
3) Without synchronization or volatile declaration, the JVM makes no guarantee about flushing writes from CPU cache or CPU registers to the main or global memory causing
visibility issues.
In short, without synchronized or volatile keywords, the JVM makes no guarantees about reorder or visibility . The following diagram is based on jenkov .com – Java Memory
Model .

JVM memory Vs. physical memory

---

## 🔹 Q2: What are the 3 common causes of concurrency issues in a multi-threaded application? What is synchronization?

**Answer:**

1) Atomicity is all about indivisible operations. This means all compound operations will either be completed or not done at all. Other threads will not be able to see the
operations “in progress”, and will NOT be viewed in a partially completed state.
2) Visibility determines when the ef fects of one thread can be seen by another . In the absence of proper synchronization, the JVM may decide that you are reading a variable
that you don’ t have to read again, and it can eliminate the repeated read as an optimization strategy .
3) Ordering determines when actions in one thread can be seen to occur out of order with respect to another . JVM will have a lot of freedom to reorder code in the absence of
synchronization.
Concurrency issues in Java can be fixed with a number of different ways
1) Carefully using synchr onized keyword at block level, method level, or class level.
2) Using the combination of “ volatile ” for variables and block level synchr onized keywords.
3) Using the Atomic classes like AtomicInteger , as atomic classes are inherently thread-safe.
4) Favoring immutable objects as they are inherently thread-safe. Once constructed, immutable objects cannot be modified.
5) Using the explicit locks, concurrent collection classes like Concurr entHashmap , Semaphores, CountDownLatch, CyclicBarrier , etc provided by the java.util.concurr ent
package.

---

## 🔹 Q3: Is the following code atomic ?

**Answer:**

No.public class CompoundOperation {
 
 private int counter = 0;
 
 public int increment () {
 return ++counter ;
 }
 //....

Why not?
Because as far as the Java compiler is concerned “++counter” is a compound operation consisting of 3 operations, and evaluated as “ read-modify-write ” as shown below:
So, if two threads enter the method “increment()” at the same time (i.e concurrently), a race condition is created. For example, when “counter = 10;”, then there is a chance
that both threads won’ t see each others’ changes and result in the counter value being “1 1” (i.e. WRONG) instead of “12” (i.e. CORRECT). In other words, one count is lost
due to the race condition of the compound operations.

---

## 🔹 Q4: How will you fix the above “atomicity” issue due to concurrent access?

**Answer:**

There are two approaches to fix the above atomicity issue.
1) Optimistic locking with the “synchr onized” keyword
Always optimistically lock regardless of there is a race condition or not due to access from multiple threads.emp = count ; //read
emp = temp + 1; //modify
count = temp ;
public class CompoundOperation {
 
 private int counter = 0;
 
 public synchronized int increment () {
 return ++counter ;
 }
 //....

2) Pessimistic CAS (Compar e-And-Swap) appr oach used in the AtomicXXXX classes
Be pessimistic, and deal with the race condition when it happens using the CAS approach implemented in the AtomicXXXX classes like AtomicInteger , AtomicBoolean,
AtomicLong, in the “java.util.concurrent.atomic” package and the new Java 8 class LongAdder .
CAS is a lock free algorithm and can be more ef ficient than using the optimistic locking with the “synchronized” keyword.

---

## 🔹 Q5: Is the following code atomic ?mport java.util.concurrent .atomic .AtomicInteger ;
public class CompoundOperation {
 
 private AtomicInteger counter = new AtomicInteger ();
 
 public int increment () {
 return counter .incrementAndGet ();
 }
 //....
mport java.math .BigDecimal ;
class Balance {
 private BigDecimal balance ;
 synchronized BigDecimal getBalance () {
 return balance ;
 }
 synchronized void setBalance (BigDecimal x) throws IllegalStateException {
 balance = x;

**Answer:**

No. T o make it atomic you need to add “ synchr onized ” modifier to “ deposit ” and “ withdraw ” methods. Otherwise two threads can concurrently access these methods
and corrupt the balance. Atomicity is guaranteed only when all the threads use synchronization correctly .

---

## 🔹 Q6: Is there anything wrong with the following code?

**Answer:**

Yes. It has a visibility problem. If two threads concurrently access “work” or “stopW ork” methods concurrently , both may not see others changes to the “done” variable.
This can cause infinite looping. So, to ensure visibility between multiple threads, you have to use a mechanism that provides synchr onization between the two threads. You
have to declare “done” to be volatile .
Conceptually , all actions on volatiles happen in a single order , where each read sees the last write in that order . if (x.compareT o(BigDecimal .ZERO ) == -1) {
 throw new IllegalStateException ("Negative Balance" );
 }
 }
 void deposit (BigDecimal x) {
 BigDecimal b = getBalance ();
 setBalance (b.add(x));
 }
 void withdraw (BigDecimal x) {
 BigDecimal b = getBalance ();
 setBalance (b.subtract (x));
 }
class Vsibility {
boolean done = false ;
void work () {
 while (!done ) {
 // do work
 }
}
void stopW ork() {
 done = true;
}

---

## 🔹 Q7: What is a volatile key word in Java?

**Answer:**

The volatile keyword is used with object and primitive variable references to indicate that a variable’ s value will be modified by different threads.
Volatile
means
The value of this variable will never be cached locally within the thread, and all the reads and writes must go to the main memory to be visible to the other threads . In
other words the keyword volatile guarantees visibility .
From JDK 5 onwards , writing to a volatile variable happens before reading from a volatile variable. In other words, the volatile keyword guarantees ordering , and
prevents compiler or JVM from reordering the code.

---

## 🔹 Q8: How does volatile keyword differ from the synchr onized keyword? ★

**Answer:**

1. The volatile keyword is applied to variables of both primitives and objects , whereas the synchronized keyword is applied to only objects.
2. The volatile keyword only guarantees visibility and ordering, but NOT atomicity , whereas the synchronized keyword can guarantee both visibility and atomicity if done
properly . So, the volatile variable has a limited use, and cannot be used in compound operations like incrementing a variable.
Wrong use of volatile in a compound operation
Right use of volatile. Example1 :volatile int counter = 0;
public void increment (){
 counter ++;
}
volatile boolean status = false ;

Or in lazy singleton. Example2 : Double checked locking
Important : Synchronized keyword (i.e. locking) can guarantee both visibility and atomicity , whereas volatile variables can only guarantee visibility . A synchronized block
can be used in place of volatile but the inverse is not true.
So, if you are not sure where to use, then use the “synchronized” keyword, and stay clear of the volatile modifier .

---

## 🔹 Q9: Is there anything wrong with the following code?/...
public void process (){
 while (!status ){
 //....
 }
public final Class MySingleton {
 private static volatile MySingleton instance = null;
 private MySingleton ( ){}
 public static MySingleton getInstance () {
 if(instance == null) {
 synchronized (MySingleton .class ) {
 if(instance == null) {
 instance = new MySingleton ();
 }
 }
 }
 return instance ;
 }

**Answer:**

Yes. The “threadB” method can return a wrong boolean value due to ordering issue. T o fix it, you need to
1) add the “ volatile ” modifier to both “cacheableFirstLevel” and “cacheableSecondtLevel” variable declarations or
2) add “ synchr onized ” modifier to both “threadA()” and “threadB()” method declarations.
to ensure that the operations are performed in order by concurrent threads.

---

## 🔹 Q10: What do you understand by the term “ Compar e-And-Swap ” approach used by the atomic classes like “AtomicInteger”?

**Answer:**

The AtomicXxxxx classes implementations internally use
1) Volatile variables to ensure visibility and ordering .
2) Utilizes direct machine level code instructions to ensure atomicity by setting the value with minimum ef fect on the execution of other threads.

---

## 🔹 Q11: What is the downside of CAS approach?

**Answer:**

CAS is a pessimistic approach as mentioned earlier where is assumes that race condition is rare and deals with the race condition when it occurs by retrying. Hence, the
downside of CAS approach is that under high contention scenarios this retry approach can turn into a spin lock , where the thread has to continuously try and set the value in
an infinite loop, until it succeeds.class Ordering {
private boolean cacheableFirstLevel = false ;
private boolean cacheableSecondtLevel false ;
public void threadA () {
 cacheableFirstLevel = true;
 cacheableSecondtLevel = true;
}
public boolean threadB () {
 boolean read1 = cacheableSecondtLevel ;
 //.…. some processing logic using read1
 boolean read2 = cacheableFirstLevel ; 
 //.…. some processing logic using read1 & read2
 boolean read3 = readyT oProcess ; 
 return read1 && !read2 && read3; 
 }

This is why “java.util.concurrent.atomic. LongAdder ” class was added in Java 8. The usage of this class is similar to the AtomicLong, but internally when the initial CAS call
fails due to a race condition, it stores the delta in an internal cell object allocated for that thread. It then adds the value of pending cells to the sum when “intV alue()” is called.
This reduces the need to go back and CAS or block other threads.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »mport java.util.concurrent .atomic .LongAdder ;
public class CompoundOperation {
 
 private LongAdder counter = new LongAdder ();
 
 public int increment () {
 counter .increment ();
 return counter .intValue();
 }
 //....

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

