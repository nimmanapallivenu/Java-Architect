# wait() & notify() Methods

> **Module**: Java Objects  
> **Topic**: wait() & notify() Methods

---

## 📋 Table of Contents



- [Q1: How do Java threads communicate with each other?](#q1)
- [Q2: What must you know about using wait () and notifyAll( ) properly?](#q2)
- [Q3: Why wait/notify methods are in the java.lang.Object class and not in the java.la](#q3)

---

## 🔹 Q1: How do Java threads communicate with each other?

**Answer:**

In inter process communication, two or more processes communicate with each other using
— Pipes (e.g. Unix processes ps -ef | grep Java).
— Sockets(Java processes using java.io.Reader and Java.io.W riter, RMI, etc).
— Serialized data is passed between processes.
In inter thread communication, you can use both pipes and sockets, and in addition, use shared memory if happens within the same process. In Java, every
object extends the java.lang.Object class, which has 3 final methods for inter -thread communication. Those methods are wait( ), notify ( ), and notifyAll( ), and
these methods are used to provide an ef ficient way for threads to communicate with each other .
Here is a very simple example, demonstrating wait/notify methods from the object class communicate with each other via an object.
Here is an example of a producer thread (thread-0) producing by incrementing the counter from 0, and the consumer thread (i.e. thread-1) consumes by
decrementing the counter. These two user created threads are spawned by the main thread, which is created by the JVM and is always there by default. The
ConsumerPr oducer is the shared object with synchronized methods that communicate with each other via wait( ) and notifyAll methods. Only one of the two
synchronized methods can be executed at a time. The wait( ) call in consume( ) relinquishes the lock to the produce( ) method, and once the produce method has
incremented the count, it notifies all threads and one of the waiting threads will resume. In this example, there is only one waiting consumer (i.e. Thread-1)
thread. So, both threads will be communicating with each other via the wait( ) and notifyAll( ) calls in the shared object ConsumerPr oducer. This is an
example of the producer -consumer design pattern.
Firstly, look at the code and then the diagram. The diagram is simplified to get an understanding and should not be construed as exactly what happens in the
JVM.
public class ConsumerProducer {
private int count ;
public synchronized void consume () {
 while (count == 0) { // keep waiting if nothing is produced to consume
 try {
 wait(); // give up lock and wait
 } catch (InterruptedException e) {
 // keep trying
 }

The main thread spawns a consumer and a producer thread. The ConsumerProducer is shared between two threads. The boolean flag is used to signal if it is a
consumer thread or a producer thread to invoke the relevant methods. }
 count --; // consume
 System.out.println (Thread .currentThread ().getName () + " after consuming " + count );
}
public synchronized void produce () {
 count ++; //produce
 System.out.println (Thread .currentThread ().getName () + " after producing " + count );
 notifyAll (); // notify waiting threads to resume
}
public class ConsumerProducerT est implements Runnable {
boolean isConsumer ;
ConsumerProducer cp;
public ConsumerProducerT est(boolean isConsumer, ConsumerProducer cp) {
 this.isConsumer = isConsumer ;
 this.cp = cp;
}
public static void main (String [] args) {
 ConsumerProducer cp = new ConsumerProducer (); //shared by both threads to communicate
 
 Thread producer = new Thread (new ConsumerProducerT est(false, cp));
 Thread consumer = new Thread (new ConsumerProducerT est(true, cp));
 producer .start();
 consumer .start();
}
@Override

The output will vary, but the last thing consumed will be 0.public void run() {
 for (int i = 1; i <= 10; i++) {
 if (!isConsumer ) {
 cp.produce ();
 } else {
 cp.consume ();
 }
 }
 //try with introducing a sleep for 100ms.
}
Thread -0 after producing 1
Thread -0 after producing 2
Thread -0 after producing 3
Thread -0 after producing 4
Thread -0 after producing 5
Thread -0 after producing 6
Thread -0 after producing 7
Thread -0 after producing 8
Thread -0 after producing 9
Thread -0 after producing 10
Thread -1 after consuming 9
Thread -1 after consuming 8
Thread -1 after consuming 7
Thread -1 after consuming 6
Thread -1 after consuming 5
Thread -1 after consuming 4
Thread -1 after consuming 3
Thread -1 after consuming 2
Thread -1 after consuming 1
Thread -1 after consuming 0

---

## 🔹 Q2: What must you know about using wait () and notifyAll( ) properly?

**Answer:**

Here are 5 things you must know to use wait( ) / notify( ) for inter thread communication
1) Use the same object for calling wait() and notify() method as every object in Java has its own lock. so calling wait() on objA and notify() on obj B will not
make any sense, and will not give you inter -thread communication.
2) In order to wait or notify, you need to “own” the object’ s lock first. So, the method or block must be synchronized.
3) You need to wait( ) or notify() on the same object you have acquired the lock for .
4) use notifyAll() instead of notify() if you expect more than one thread is waiting for lock.
5) Always call wait() method in a loop because if multiple threads are waiting for lock and one of them got lock and reset the condition and the other thread
needs to check the condition after they got woken up to see whether they need to wait again or can start processing.

---

## 🔹 Q3: Why wait/notify methods are in the java.lang.Object class and not in the java.lang.Thread class?

**Answer:**

— In Java, an object itself shared between threads, and each object intrinsically has a lock, which allows thread to share a object, and communicate with each
other .
— If wait( ) and notify( ) were on the Thread instead then each thread would have to know the status of every other thread.
— Since wait/notify are in the java.lang.Object class, the threads don’ t need to have specific knowledge of each other and they can run asynchronously .
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