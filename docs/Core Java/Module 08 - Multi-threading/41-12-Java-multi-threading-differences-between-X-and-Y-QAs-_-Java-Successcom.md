# 41. 12 Java multi threading differences between X and Y Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: What is difference between ‘Executor .submit()’ and ‘Executor .execute()’ metho](#q1)
- [Q2: What are the differences among CyclicBarrier , CountDownLatch and join in Java?](#q2)
- [Q3: What is the difference between wait() and sleep() in Java?](#q3)
- [Q4: What is the difference between Runnable and Callable<V> interfaces in Java?](#q4)
- [Q5: What is the difference between start() and run() methods?](#q5)
- [Q6: What is the difference between daemon and non-daemon threads?](#q6)
- [Q7: What is the difference between intrinsic lock and extrinsic locks in Java?](#q7)
- [Q8: What are the differences among Atomicity , Visibility , and Ordering in Java mu](#q8)
- [Q9: What is the difference between volatile and synchronized keywords in Java?](#q9)
- [Q10: What are the different thread states?](#q10)
- [Q11: What is the difference between synchronized method and synchronized block?](#q11)
- [Q12: What is the difference between a mutex and a semaphor e?](#q12)

---

## 🔹 Q1: What is difference between ‘Executor .submit()’ and ‘Executor .execute()’ method ?

**Answer:**

The difference is that execute() doesn’ t return a “Future” object, so you can’ t wait for the completion of the Runnable, and get any exception it throws. The
submit(…) method accepts Callable<V> and returns an instance of Future<V>, which you can use later in caller (or client) to retrieve the results of
asynchronous computation.

---

## 🔹 Q2: What are the differences among CyclicBarrier , CountDownLatch and join in Java?

**Answer:**

join: “t1.join()” makes the current thread to wait for thread “t1” to complete.
join() waits for one thread to complete whereas CountDownLatch.await() allows N threads to wait until the countdown reaches 0. It can be used to make sure N
threads start doing something at the same time.
The main difference between CyclicBarrier and CountDownLatch is that CyclicBarrier can be reused by calling reset() method which resets the barrier to its
initial state whereas a CountDownLatch cannot be reused.

---

## 🔹 Q3: What is the difference between wait() and sleep() in Java?

**Answer:**

A waiting thread can be “woken up” by another thread calling notify on the monitor (i.e. lock) which is being waited on whereas a sleep cannot.
A wait (and notify/notifyAll) must happen within a block synchronized on the monitor (i.e. lock) object whereas sleep does not.
wait() is called on an object, whereas a sleep is called on a thread.

---

## 🔹 Q4: What is the difference between Runnable and Callable<V> interfaces in Java?

**Answer:**

The Callable<V> was introuduced in Java 5, and it is very similar to Runnable, except for:
A Callable instance returns a result of type V , whereas a Runnable instance doesn’ t.
A Callable instance may throw checked exceptions, whereas a Runnable instance can’ t throw checked exceptions.
A Callable implementation needs to implement a call() method and a Runnable interface needs implement a run() method.

---

## 🔹 Q5: What is the difference between start() and run() methods?

**Answer:**

The start() method starts the execution of the new thread and calls the run() method. The start() method returns immediately and the new thread normally
continues until the run() method returns.<T> Future <T> submit (Callable <T> task);
Future <?> submit (Runnable task);
<T> Future <T> submit (Runnable task, T result );

Calling run() directly just executes the code synchronously in the same thread without spawning a new worker thread, just as a normal method call.

---

## 🔹 Q6: What is the difference between daemon and non-daemon threads?

**Answer:**

Daemon threads in Java are those threads which run in background. Background threads performing house keeping tasks. These threads continue to execute
even after the thread that spawned them exits or dies. For example, the JVM Garbage collector thread is a daemon thread. GC thread is a low priority thread
which runs intermittently in the back ground doing the garbage collection operation.
When an application starts running, it creates a non-daemon thread, whose job is to execute the main() method. The JVM or process exits when the last non-
daemon thread exits.

---

## 🔹 Q7: What is the difference between intrinsic lock and extrinsic locks in Java?

**Answer:**

Each Java class and object (i.e. instance of a class) has an intrinsic lock (aka monitor). Don’ t confuse this with explicit lock utility classes that were added
in Java 1.5.
Explicit locks are laid out with 2 interfaces Lock and ReadW riteLock . It is more complicated to use it properly , and incorrect usage can lead to unexpected
issues leading to deadlocks, thread starvation, etc. So, you need to remember the following best practices when using explicit locks.
Release the explicit locks in a finally block.
Favor intrinsic locks where possible to avoid bugs and to keep your code cleaner and easier to maintain.
Use tryLock( ) if you don’ t want a thread waiting indefinitely to acquire a lock. This is similar to how databases prevent dead locks with wait lock
timeouts.
When using ReentrantLocks for frequent concurrent reads and occasional writes, be mindful of the possibility that a writer could wait a very long time
sometimes forever) if there are constantly read locks held by other threads.
Both intrinsic and explicit locks in Java are reentrant . Reentrancy means that locks are acquired on a per -thread rather than per -invocation basis. In Java, both
intrinsic and explicit locks are re-entrant.

---

## 🔹 Q8: What are the differences among Atomicity , Visibility , and Ordering in Java multithreading?

**Answer:**

Atomicity means an operation will either be completed or not done at all. Other threads will not be able to see the operation “in progress” — it will never
be viewed in a partially complete state.
Visibility determines when the ef fects of one thread can be seen by another . In the absence of proper synchronization, the JVM may decide that you are reading
a variable that you don’ t have to read again, and it can eliminate the repeated read as an optimization strategy .
Ordering determines when actions in one thread can be seen to occur out of order with respect to another . JVM will have a lot of freedom to reorder code in the
absence of synchronization.

---

## 🔹 Q9: What is the difference between volatile and synchronized keywords in Java?

**Answer:**

The volatile keyword guarantees ordering, and prevents compiler or JVM from reordering of the code.
The volatile keyword is applied to variables of both primitives and objects, whereas the synchronized keyword is applied to only objects.

The volatile keyword only guarantees visibility and ordering , but not atomicity , whereas the synchronized keyword can guarantee both visibility and
atomicity if done properly . So, the volatile variable has a limited use, and cannot be used in compound operations like incrementing a variable.

---

## 🔹 Q10: What are the different thread states?

**Answer:**

The state chart diagram below describes the thread states.

---

## 🔹 Q11: What is the difference between synchronized method and synchronized block?

**Answer:**

In Java programming, each object has a lock . A thread can acquire the lock for an object by using the synchr onized keyword. The synchronized keyword
can be applied in method level (coarse grained lock – can af fect performance adversely) or block level of code (fine grained lock). Often using a lock on a
method level is too coarse. Why lock up a piece of code that does not access any shared resources by locking up an entire method. Since each object has a lock,
dummy objects can be created to implement block level synchronization. The block level is more ef ficient because it does not lock the whole method.
method level Vs block level locking
The JVM uses locks in conjunction with monitors . A monitor is basically a guardian who watches over a sequence of synchronized code and making sure only
one thread at a time executes a synchronized piece of code. Each monitor is associated with an object reference. When a thread arrives at the first instruction in a
block of code it must obtain a lock on the referenced object. The thread is not allowed to execute the code until it obtains the lock. Once it has obtained the lock,
the thread enters the block of protected code. When the thread leaves the block, no matter how it leaves the block, it releases the lock on the associated object.

---

## 🔹 Q12: What is the difference between a mutex and a semaphor e?

**Answer:**

A Semaphore can be counted down from say 10, whilst a mutex can only count to 1. For example, a block of code can be executed simultaneously by 10

threads by setting the semaphores to 10. W ith mutex only 1 thread can execute a block of code at a time.
Q. Why synchronization is important?
A. Without synchronization, it is possible for one thread to modify a shared object while another thread is in the process of using or updating that object’ s value.
This often causes dirty data and leads to significant errors. The disadvantage of synchronization is that it can cause deadlocks when two threads are waiting on
each other to do something. Also synchronized code has the overhead of acquiring lock, which can adversely af fect the performance.
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

