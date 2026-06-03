# Locks & Synchronized

> **Module**: Multi-threading  
> **Topic**: Locks & Synchronized

---

## 📋 Table of Contents



- [Q1: What does re-entrancy mean regarding intrinsic or explicit locks?](#q1)
- [Q2: If 2 different threads hit 2 different synchronized methods in an object at th](#q2)
- [Q3: Why synchronization is important?](#q3)
- [Q4: What is the disadvantage of synchronization?](#q4)
- [Q5: When every object has an intrinsic lock in Java, why were explicit lock utility ](#q5)
- [Q6: How are explicit locks laid out in Java?](#q6)
- [Q7: What are the disadvantages of explicit locks?](#q7)

---

## 🔹 Q1: What does re-entrancy mean regarding intrinsic or explicit locks?

**Answer:**

Re-entrancy means that locks are acquired on a per-thread rather than per -invocation basis.
public synchronized void method1 (){
//intrinsic lock is acquired
operation1 (); //ok to enter this synchronized method
 //as locks are on per thread basis

In Java, both intrinsic and explicit locks are re-entrant.

---

## 🔹 Q2: If 2 different threads hit 2 different synchronized methods in an object at the same time will they both continue?

**Answer:**

No. Only one thread can acquire the lock in a synchronized method of an object. Each object has a synchronization lock. No 2 synchronized methods within
an object can run at the same time. One synchronized method should wait for the other synchronized method to release the lock. This is demonstrated here with
method level lock. Same concept is applicable for block level locks as well.
operation2 (); //ok to enter this synchronized method
 //as locks are on per thread basis
//intrinsic lock is released 
public synchronized void operation1 (){
 //process 1
public synchronized void operation2 (){
 //process 1

Java synchronization

---

## 🔹 Q3: Why synchronization is important?

**Answer:**

Without synchronization, it is possible for one thread to modify a shared object while another thread is in the process of using or updating that object’ s
value. This often causes dirty data and leads to significant errors.

---

## 🔹 Q4: What is the disadvantage of synchronization?

**Answer:**

The disadvantage of synchronization is that it can cause deadlocks when two threads are waiting on each other to do something. Also, synchronized code
has the overhead of acquiring lock, and preventing concurrent access, which can adversely af fect performance.

---

## 🔹 Q5: When every object has an intrinsic lock in Java, why were explicit lock utility classes introduced in Java 5?

**Answer:**

An intrinsic locking mechanism is a clean approach in terms of writing code, and is pretty good for most of the use-cases. But, intrinsic locking mechanism
do have some limitations in certain scenarios:
It is not possible to have more control, for example, read concurrently when not writing.
Intrinsic locks must be released in the same block in which they are acquired.

It is not possible to interrupt a thread waiting to acquire a lock.
It is not possible to attempt to acquire a lock without waiting for it forever .

---

## 🔹 Q6: How are explicit locks laid out in Java?

**Answer:**

Laid out with 2 interfaces Lock and ReadW riteLock .
java.util.concurr ent.locks.Lock – simplest case of a lock which can be acquired and released.
The implementation class of the above Lock interface is java.util.concurrent.locks.ReentrantLock, which has the same basic behavior and semantics as the
intrinsic lock each Java object has.
java.util.concurr ent.locks.ReadW riteLock – a lock implementation that has both read and write lock types – multiple read locks can be held at a time
unless the exclusive write lock is held.void lock(); //acquires the lock
void lockInterruptibly () throws InterruptedException; //acquires the lock unless current thread
 //is interrupted
boolean tryLock (); //Acquires the lock only if it is free
 //at the time of invocation.
boolean tryLock (long time, TimeUnit unit) throws InterruptedException; //Acquires the lock if it
 //is free within the given
 // waiting time
 //and the current thread
 //has not been interrupted
/returns the lock used for reading
Lock readLock ();
/returns the lock used for writing.
Lock writeLock ();

---

## 🔹 Q7: What are the disadvantages of explicit locks?

**Answer:**

It is more complicated to use it properly, and incorrect usage can lead to unexpected issues leading to deadlocks, thread starvation, etc. So, you need to
remember the following best practices when using explicit locks.
Release the explicit locks in a finally block.
Favor intrinsic locks where possible to avoid bugs and to keep your code cleaner and easier to maintain.
Use tryLock( ) if you don’ t want a thread waiting indefinitely to acquire a lock. This is similar to how databases prevent dead locks with wait lock
timeouts.
When using ReentrantLocks for frequent concurrent reads and occasional writes, be mindful of the possibility that a writer could wait a very long time
sometimes forever) if there are constantly read locks held by other threads.
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