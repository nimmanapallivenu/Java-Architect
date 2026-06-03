# 7 15+ Java multithreading interview questions & answers

## Table of Contents

- [Q1: What is a thread?](#q1)
- [Q2: What are the JVM or system created threads?](#q2)
- [Q3: What is the key dif ference between a process and a thread?](#q3)
- [Q4: Explain dif ferent ways of creating a thread?](#q4)
- [Q5: Which approach would you favor and why?](#q5)
- [Q6: What design pattern does the executor framework use?](#q6)
- [Q7: What is the dif ference between yield and sleep? What is the dif ference between](#q7)
- [Q8: Why is locking of a method or block of code for thread safety is called “ synchr](#q8)
- [Q9: Can you explain what an intrinsic lock or monitor is?](#q9)
- [Q10: What does re-entrancy mean regarding intrinsic or explicit locks?](#q10)
- [Q11: If you have a circular reference of objects, but you no longer reference it from](#q11)
- [Q12: When you have automatic memory management in Java via GC, why do you still get m](#q12)
- [Q13: Why is synchronization important?](#q13)
- [Q14: What is the disadvantage of synchronization?](#q14)
- [Q15: How does thread synchronization occurs inside a monitor? What levels of synchron](#q15)
- [Q16: How will you go about writing a thread-safe & lazily initialized singleton class](#q16)
- [Q17: Why is ThreadLocal useful, and what is the consequence of abusing it?](#q17)

---

## Q1: What is a thread?

**Answer:**

It is a thread of execution in a program. The JVM (i.e. process) allows an application to have multiple threads of execution running concurr ently .
Java Vs OS Thread
In the Hotspot JVM there is a direct mapping between a Java Thread and a native operating system (i.e. OS) Thread.
The native OS thread is created after preparing the state for a Java thread involving thread local storage, allocation of buf fers, creating the synchronization objects, stacks and the program counter .
The OS is responsible for scheduling all threads and dispatching them to any available CPU. In a multi-core CPU, you will get real parallelism . When you have more threads than the number of CPU cores,
the CPU switches from executing one thread to executing another , and it needs to save the local data, program pointer , etc. of the current thread, and load the local data, program pointer , etc. of the next thread
to execute. This switch is called context switching , which is not cheap, and you should NOT context switch more than necessary .
When a thread terminates, all resources for both the native and Java thread are released.

---

## Q2: What are the JVM or system created threads?

**Answer:**

The main thr ead and a number of backgr ound thr eads .
1) main thr ead, which is created as part of invoking the main(…) method
2) VM backgr ound thr ead to perform major GC (i.e. Garbage Collection), thread dumps (e.g. kill -QUIT in Unix), thread suspension, etc. Major GC evicts the objects that are promoted to the “ Tenur ed”
part of the heap./main thread gets created, which later can spawn other worker threads
public static void main (String [ ] args) {
 .... 
}

Java heap sections
3) Garbage Collection low priority background thread for minor GC activities. Minor GC evicts the objects in the “ Eden ” and “ Survivor ” part of the heap.
4) Compiler backgr ound thr ead to compile byte code to native code at run-time.
5) Other background threads such as signal dispatcher thread and periodic task thread.

---

## Q3: What is the key dif ference between a process and a thread?

**Answer:**

A process is an execution of a program but a thread is a single execution sequence within the process. A process can contain multiple threads. A thread is sometimes called a lightweight process.
Process Vs Threads
A JVM runs in a single process and each process will have its own heap. Threads run within a process (i.e. a JVM process), and share the heap belonging to that process. This is why several threads may access
the same object. Threads share the same heap and have their own stack space . This is how one thread’ s invocation of a method and its local variables are kept thread safe from other threads. But the heap is
not thread-safe and must be synchronized for thread safety , which is depicted below:

Java Stack Vs Heap memory & thread-safety
and learn more about 3 Java Multithreading basics – Heap Vs Stack, Thread-safety & Synchronization with Java code .
If you are an experienced Java developer?
More advanced & practical Java multi-threading interview Q&As

---

## Q4: Explain dif ferent ways of creating a thread?

**Answer:**

In addition to the JVM created threads, application developers can create new worker thr eads in Java. Threads can be created in a number of dif ferent ways.

Creating a Thread in Java
1) Extending the java.lang.Thread class.
2) Implementing the java.lang.Runnable interface.
3) Implementing the java.util.concurrent.Callable interface with the java.util.concurrent.Executor framework to pool the threads. The java.util.concurrent package was added in Java 5. [ 7 basic Java
Executor framework Interview Q&As with Future & CompletableFuture ]
4) Using the Fork/Join Pool . Java 7 fork and join tutorial with a diagram and an example .
5) Java 8 – BaseStream.parallel() – Use this for low CPU intensive tasks. All parallel streams use common fork-join thread pool, and if you submit a long-running task, you ef fectively block all threads in
the pool.
6) The actor model , which is also known as Reactive Programming using frameworks like Akka . Simple Akka tutorial in Java step by step
Note: Learn more about ExecutorService Vs Fork/Join & Future Vs CompletableFuture Interview Q&As .
1. Extending the java.lang.Thr ead class
mport java.util.concurrent .atomic .AtomicInteger ;
class Counter extends Thread {
 AtomicInteger count = new AtomicInteger ();

Output:
 // method where the thread execution will start
 public void run() {
 // logic to execute in a thread
 // e.g. performing a count task
 System .out.println (Thread .currentThread ().getName () + " is executing..." + count .incrementAndGet ());
 }
 // let’ s see how to start the threads
 public static void main (String [] args) {
 System .out.println (Thread .currentThread ().getName () + " is executing..." );
 Counter counter = new Counter ();
 Thread t1 = new Thread (counter );
 Thread t2 = new Thread (counter );
 t1.start(); // start the thread. This calls the run() method.
 t2.start(); // start the thread. This calls the run() method.
 }
main is executing ...
Thread -1 is executing ...1
Thread -2 is executing ...2

Pictorial depiction what the code above and below do.
2. Implementing the java.lang.Runnable interface. The Thread class takes a runnable object as a constructor ar gument.
Output:
3. Implementing the java.util.concurr ent.Callable interface with the executor service framework.mport java.util.concurrent .atomic .AtomicInteger ;
class Counter implements Runnable {
 AtomicInteger count = new AtomicInteger ();
 // method where the thread execution will start
 public void run() {
 // logic to execute in a thread
 // e.g. performing a count task
 System .out.println (Thread .currentThread ().getName () + " is executing..." + count .incrementAndGet ());
 }
 // let’ s see how to start the threads
 public static void main (String [] args) {
 System .out.println (Thread .currentThread ().getName () + " is executing..." );
 Counter counter = new Counter ();
 Thread t1 = new Thread (counter );
 Thread t2 = new Thread (counter );
 t1.start(); // start the thread. This calls the run() method.
 t2.start(); // start the thread. This calls the run() method.
 }
main is executing ...
Thread -0 is executing ...1
Thread -1 is executing ...2

Executor Framework
mport java.util.concurrent .Callable ;
mport java.util.concurrent .ExecutionException ;
mport java.util.concurrent .ExecutorService ;
mport java.util.concurrent .Executors ;
mport java.util.concurrent .Future ;
mport java.util.concurrent .atomic .AtomicInteger ;
class Counter implements Callable {
 private static final int THREAD_POOL_SIZE = 2;
 private AtomicInteger count = new AtomicInteger ();
 // method where the thread execution takes place
 public String call() {
 return Thread .currentThread ().getName () + " executing ..." + count .incrementAndGet (); //Consumer
 }
 public static void main (String [] args) throws InterruptedException ,
 ExecutionException {
 // create a pool of 2 threads

Output:
4. Java 8 – BaseStream.parallel()

---

## Q5: Which approach would you favor and why?

**Answer:**

Favor Callable interface with the Executor framework for thread pooling. ExecutorService executor = Executors
 .newFixedThreadPool (THREAD_POOL_SIZE );
 Counter counter = new Counter ();
 
 Future future1 = executor .submit (counter ); //Producer
 Future future2 = executor .submit (counter ); //Producer
 System .out.println (Thread .currentThread ().getName () + " executing ..." );
 //asynchronously get from the worker threads
 System .out.println (future1 .get());
 System .out.println (future2 .get());
 }
main executing ...
pool-1-thread -1 executing ...1
pool-1-thread -2 executing ...2
Employee [] employees = { new Employee ("John" , 45000 ), new Employee ("Peter" , 65000.00 ), new Employee ("Sam" , 85000.00 )};
 
List inputList = Arrays .asList (employees );
 
List newMutatedList = inputList .stream ().parallel ()
 .map(x -> new Employee (x.getName (), x.getSalary () * 1.05)) // returns a new list
 .collect (Collectors .toList ());
 
System .out.println (inputList ); //[Employee [name=John, salary=45000.0], Employee [name=Peter , salary=65000.0], Employee [name=Sam, salary=85000.0]]
System .out.println (newMutatedList ); //[Employee [name=John, salary=47250.0], Employee [name=Peter , salary=68250.0], Employee [name=Sam, salary=89250.0]]

1) The thread pool is more ef ficient. Even though the threads are light-weighted than creating a process, creating them utilizes a lot of resources. Also, creating a new thread for each task will consume more
stack memory as each thread will have its own stack and also the CPU will spend more time in context switching. Creating a lot many threads with no bounds to the maximum threshold can cause application to
run out of heap memory . So, creating a Thread Pool is a better solution as a finite number of threads can be pooled and reused. The runnable or callable tasks will be placed in a queue, and the finite number of
threads in the pool will take turns to process the tasks in the queue.
2) The Runnable or Callable interface is preferred over extending the Thread class, as it does not require your object to inherit a thread because when you need multiple inheritance, only interfaces can help
you. Java class can extend only one class, but can implement many interfaces.
3. The Runnable interface’ s void run( ) method has no way of returning any result back to the main thread. The executor framework introduced the Callable interface that returns a value from its call( ) method.
This means the asynchronous task will be able to return a value once it is done executing.

---

## Q6: What design pattern does the executor framework use?

**Answer:**

The java.util.concurrent. Executor is based on the producer -consumer design pattern , where threads that submit tasks are producers and the threads that execute tasks are consumers . In the above
examples, the main thread is the producer as it loops through and submits tasks to the worker threads. The “Counter” is the consumer that executes the tasks submitted by the main thread.

---

## Q7: What is the dif ference between yield and sleep? What is the dif ference between the methods sleep( ) and wait( )?

**Answer:**

When a task invokes yield( ), it changes from running state to runnable state. When a task invokes sleep ( ), it changes from running state to waiting/sleeping state.
The method wait(1000) causes the current thread to wait up to one second for a signal (i.e. notify()/notifyAll()) from other threads. A thread could wait less than 1 second if it receives the notify( ) or notifyAll(
) method call. The call to sleep(1000) causes the current thread to sleep for t least 1 second.
Threads performing tasks by talking to each other

---

## Q8: Why is locking of a method or block of code for thread safety is called “ synchr onized ” and not “lock” or “locked”?

**Answer:**

When a method or block of code is locked with the reserved “synchronized” key word in Java, the memory (i.e. heap) where the shared data is kept is synchronized. This means,

JVM memory Vs. physical memory
When a synchronized block or method is entered after the lock has been acquired by a thread, it first reads (i.e. synchr onizes ) any changes to the locked object from the main heap memory to ensure that the
thread that has the lock has the current info before start executing.
After the synchronized block has completed and the thread is ready to relinquish the lock, all the changes that were made to the object that was locked is written or flushed back (i.e. synchr onized ) to the
main heap memory so that the other threads that acquire the lock next has the current info.

This is why it is called “synchronized” and not “locked”. This is also the reason why the immutable objects are inherently thread-safe and does not require any synchronization. Once created, the immutable
objects cannot be modified.
Learn more about the memory model & the synchronization at: 10+ Atomicity , Visibility , and Ordering interview Q&A in Java multi-threading

---

## Q9: Can you explain what an intrinsic lock or monitor is?

**Answer:**

7 Things you must know about Java locks and synchronized key word .

---

## Q10: What does re-entrancy mean regarding intrinsic or explicit locks?

**Answer:**

Re-entrancy means that locks are acquired on a per-thread rather than per-invocation basis. In Java, both intrinsic and explicit locks are re-entrant .

---

## Q11: If you have a circular reference of objects, but you no longer reference it from an execution thread, will this object be a potential candidate for garbage collection?

**Answer:**

Yes. Refer diagram below . The key call out here is that “ you no longer r eference it fr om an execution thr ead“. If it is reachable or can be referenced then it cannot be Garbage Collected, and can cause
memory leaks.
public synchronized void method1 (){
//intrinsic lock is acquired
operation1 (); //ok to enter this synchronized method
 //as locks are on per thread basis
operation2 (); //ok to enter this synchronized method
 //as locks are on per thread basis
//intrinsic lock is released 
public synchronized void operation1 (){
 //process 1
public synchronized void operation2 (){
 //process 1

Java GC cyclic reference
GC Roots (i.e. Garbage Collector Roots) are objects special for GC. GC considers objects as “garbage” if they aren’ t reachable through a chain starting at a GC Root.

Even though the collected objects have a cyclic reference, they’re still garbage if they’re cut of f from the GC Root. There are several kinds of GC Roots like Stack local (shown in the above diagram), Live
thread, classes loaded by system class loader , JNI Local, etc.

---

## Q12: When you have automatic memory management in Java via GC, why do you still get memory leaks in Java?

**Answer:**

Firstly , a memory leak is all about existence of objects that are no longer required, but still remain in memory and cannot be evicted because they are reachable from other live objects, which is at least
one GC Root object. This happens due to a bug in your application itself or a bug in a framework or library you are using.
In Java, memory leak can occur due to
1) Long living objects having r eference to short living objects , causing the memory to slowly grow . For example, singleton classes referring to short lived objects. This prevents short-lived objects being
garbage collected.
2) Impr oper use of thr ead-local variables . The thread-local variables will not be removed by the garbage collector as long as the thread itself is alive. So, when threads are pooled and kept alive forever , the
object might never be removed by the garbage collector .
3) Using mutable static fields to hold data caches , and not explicitly clearing them. The mutable static fields and collections need to be explicitly cleared.
4) Objects with cir cular r eferences that are reachable from a GC Root object.
GC uses “ reference counting “. Whenever a reference to an object is added its reference count is increased by 1. Whenever a reference to an object is removed, the reference count is decreased by 1. If “A”
references object B and B references object A, then both of their reference counts can never be less than 1, which means they will never get collected if they are reachable from a GC Root.
5) JNI (Java Native Interface) memory leaks.

---

## Q13: Why is synchronization important?

**Answer:**

Without synchronization, it is possible for one thread to modify a shared object while another thread is in the process of using or updating that object’ s value. This often causes dirty data and leads to
significant errors. 7 Things you must know about Java locks and synchronized key word .

---

## Q14: What is the disadvantage of synchronization?

**Answer:**

The disadvantage of synchronization is that it can cause deadlocks when two threads are waiting on each other to do something. Also, synchronized code has the overhead of acquiring lock, and
preventing concurrent access, which can adversely af fect performance.
Inter thread communication & thread dead-lock explained Q&As & tutorial style

---

## Q15: How does thread synchronization occurs inside a monitor? What levels of synchronization can you apply? What is the dif ference between synchronized method and synchronized block?

**Answer:**

In Java programming, each object has a lock. A thread can acquire the lock for an object by using the synchronized keyword. The synchronized keyword can be applied in method level (coarse grained
lock – can af fect performance adversely) or block level of code (fine grained lock). Often using a lock on a method level is too coarse. Why lock up a piece of code that does not access any shared resources by
locking up an entire method. Since each object has a lock, dummy objects can be created to implement block level synchronization. The block level is more ef ficient because it does not lock the whole method.

coarse grained Vs fine grained locks
The JVM uses locks in conjunction with monitors. A monitor is basically a guardian who watches over a sequence of synchronized code and making sure only one thread at a time executes a synchronized
piece of code. Each monitor is associated with an object reference. When a thread arrives at the first instruction in a block of code it must obtain a lock on the referenced object. The thread is not allowed to
execute the code until it obtains the lock. Once it has obtained the lock, the thread enters the block of protected code. When the thread leaves the block, no matter how it leaves the block, it releases the lock on
the associated object. For static methods, you acquire a class level lock.

---

## Q16: How will you go about writing a thread-safe & lazily initialized singleton class?

**Answer:**

Using a “ Double checked locking ” pattern. Explained in detail at Singleton design pattern in Java & 5 key follow up Interview Q&As

---

## Q17: Why is ThreadLocal useful, and what is the consequence of abusing it?

**Answer:**

Allows you to create per-thread-singleton . Many frameworks use ThreadLocal to maintain some context related to the current thread.
Having said this, you must be very careful when using ThreadLocal as they are per thread static/global variable, and can cause memory leaks .
Learn more at Java ThreadLocal Interview questions & answers
CPU Vs. GPU Vs. multi-node clusters for r eal parallelism
For example, a CPU with 2 cores and 2 hyper -threads (i.e. A CPU thread as opposed to an OS thread) per core could theoretically speedup up 4 times due to hardware parallelism. Server -grade CPUs will
easily have 64 or more cores. CPUs support parallelism by assigning threads to dif ferent tasks and coordinating their activities through hardware-supported locking primitives in shared memory . GPUs will take
parallelism to a whole new level as in 1000s of cores.
CPU and GPU are core hardware components in modern computing made of millions of transistors which process thousands of operations each second. GPU stands for Graphics Processing Unit, which is a
specialised kind of CPU built for the purpose of video rendering and displaying graphics. Although a GPU runs at a lower speed than a CPU, it has more processing cores as in thousands of cores so that precise
calculations can be made for thousands of individual pixels at a time, making it possible for games to display their complex 3D graphics.
Computation intensive applications are tapping into GPUs for MPP (i.e. Massively Parallel Processing) applications:

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
