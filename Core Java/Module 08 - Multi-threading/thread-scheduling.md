# Thread Scheduling & Daemon Threads

> **Module**: Multi-threading  
> **Topic**: Thread Scheduling & Daemon Threads

---

## 📋 Table of Contents



- [Q1: What are the different ways a Java thread gets blocked or suspended? How will y](#q1)
- [Q2: How can you sequence threads in Java?](#q2)
- [Q3: Can you guarantee the order of thread execution in Java?](#q3)
- [Q4: Can you call the run( ) directly instead of calling start() in Java thread?](#q4)
- [Q5: Can you restart a thread that is already started by calling the start( ) method ](#q5)
- [Q6: What is a daemon thread?](#q6)
- [Q7: Are threads daemon by default when created with new Thread(…)?](#q7)
- [Q8: How are threads prioritized?](#q8)
- [Q9: Is Java thread scheduling preemptive?](#q9)

---

## 🔹 Q1: What are the different ways a Java thread gets blocked or suspended? How will you debug Java threading issues?

**Answer:**

1. It has been put to sleep for a set amount of time
2. The thread is suspended by call to wait( ), and will become runnable on a notify or notifyAll message.public void run(){
 try{
 while (true){
 this.sleep (1000 );
 System.out.println ("looping while" );
 }
 }catch (InterruptedException ie){
 ie.printStackT race();
 }
class ConsumerProducer {
 private int count ;
 public synchronized void consume () {
 while (count == 0) {
 try {
 wait();

3. Using join( ) method, which means waiting for a thread to complete. In the example below, the main thread that spawned worker threads t1 and t2 will wait
on the t1.join() line until t1 has finished its work, and then will do the same for t2.join( ).
4. Threads will be blocked on acquiring an intrinsic or explicit lock. Understanding Java locks and synchronized keyword.
5. Threads can get blocked on a long running I/O operations. For example, on a call to long running database operation. Threads block on I/O so that other
threads may execute whilst the I/O operation is being performed. }
 catch (InterruptedException ie){}
 }
 count --; //consumed 
 } 
 public synchronized void produce () {
 count ++;
 notify (); //notify the waiting consumer that count is incremented
 }
Thread t1 = new Thread (new EventThread ("event-1" ));
 t1.start();
 Thread t2 = new Thread (new EventThread ("event-2" ));
 t2.start();
 while (true) {
 try {
 t1.join();
 t2.join(); 
 }
 catch (InterruptedException e) {
 e.printStackT race();
 }
 }

In Java 5 NIO (New I/O) was introduced to perform non-blocking I/O with selectors and channels.
Q. How do you debug threading issues like getting blocked?
A. 5 Ways to debug Java thread-safety issues .

---

## 🔹 Q2: How can you sequence threads in Java?

**Answer:**

Using a join( ) method, which means waiting for a thread to complete. In the example below, the main thread that spawned worker threads t1 and t2 will
wait on the t1.join() line until t1 has finished its work, and then will do the same for t2.join().class MyServer implements Runnable {
public void run() {
 try {
 ServerSocket ss = new ServerSocket (POR T);
 while (!Thread .interrupted ()){
 new Thread (new Handler (ss.accept ())).start();
 // one thread per socket connection
 // every thread created this way will essentially block for I/O
 }
 } catch (IOException ex) {
 ex.printStacktrace ();
 }
 //...
Thread t1 = new Thread (new EventThread ("event-1" )); 
1.start(); 
Thread t2 = new Thread (new EventThread ("event-2" )); 
2.start(); 

while (true) { 
 try { 
 t1.join(); 
 t2.join(); 
 } 
 catch (InterruptedException e) { 
 e.printStackT race(); 
 } 

multithreading join()

---

## 🔹 Q3: Can you guarantee the order of thread execution in Java?

**Answer:**

No. How the threads are run on depends on the Thread Scheduler. So, you cannot guarantee the order of execution. Calling start() doesn’ t mean run() will
be called immediately, it depends on thread scheduler when it chooses to run your thread.
However, when you use join(), it makes sure that as soon as a thread calls join,the current thread will be blocked until the thread you have called join is finished.

---

## 🔹 Q4: Can you call the run( ) directly instead of calling start() in Java thread?

**Answer:**

No. Calling run() directly just executes the code synchronously in the same thread without spawning a new worker thread, just as a normal method call.
The start() method starts the execution of the new thread and calls the run() method. The start() method returns immediately and the new thread normally
continues until the run() method returns.
Java Thread Object’ s constructor (e.g. new Thread) creates a Java Thread Object, but not an OS level thread – and the start() method creates an OS level thread.
So, never call run() method directly. t1.start( ) will internally call run( ).

---

## 🔹 Q5: Can you restart a thread that is already started by calling the start( ) method again?

**Answer:**

No. The Java API says “It is never legal to start a thread more than once”. throws an IllegalThreadStateException – if the thread was already started. A
thread’ s life-cycle completes once it completes execution.

---

## 🔹 Q6: What is a daemon thread?

**Answer:**

Daemon threads in Java are those threads which run in background.Background threads performing house keeping tasks. These threads continue to
execute even after the thr ead that spawned them exits. For example,
— JVM spawns a low priority daemon thread to perform Garbage collection.
— Thread.setDaemon(true) makes a thread daemon, but it can only be called before starting a thread in Java. It will throw IllegalThreadStateException if
corresponding Thread is already started and running, and you try to call setDaemon(true).

---

## 🔹 Q7: Are threads daemon by default when created with new Thread(…)?

**Answer:**

When code running in some thread creates a new Thread object, and weather it is a daemon or non daemon thread set equal to the thread that created it. The
main thread in Java that is created implicitly by the JVM is a Non Daemon.

---

## 🔹 Q8: How are threads prioritized?

**Answer:**

Every thread has a priority. Threads with higher priority are executed in preference to threads with lower priority by the thread scheduler. When code
running in some thread creates a new Thread object, the new thread has its priority initially set equal to the priority of the creating thread. This is similar to how
a thread is daemon or not is set. But, the daemon or not needs to be set befor e starting a thr ead, whereas you can modify a thr ead’s priority at any time
after its cr eation using the setPriority() method.
You have the setPriority ( ) method in the Thread class, but how the priority works depends on the underlying native platform like Windows, Linux, Solaries,
etc.

---

## 🔹 Q9: Is Java thread scheduling preemptive?

**Answer:**

English dictionary definition of preempt means “Act in advance”.
In general, there are two types of scheduling: non-pr eemptive scheduling, and preemptive scheduling. In non-preemptive scheduling, a thread runs until it
terminates, stops, blocks, suspends, or yields. In preemptive scheduling, even if the current thread is still running, a context switch will occur when its time
slice is used up.
The Java run-time system’ s thread scheduling algorithm is preemptive, which means if at any time a thread with a higher priority than all other “runnable”
threads becomes “runnable”, the run-time system chooses the new higher priority thread for execution. The new higher priority thread is said to preempt the
other threads.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »public static void main (String args[]) { 
 Thread .currentThread ().setPriority (Thread .MAX_PRIORITY ); 
 // ................... 
}

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