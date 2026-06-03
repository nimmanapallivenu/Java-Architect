# Thread Control Methods

> **Module**: Multi-threading  
> **Topic**: Thread Control Methods

---

## 📋 Table of Contents



- [Q1: Why are Thread.stop, Thread.suspend and Thread.resume depr ecated ?](#q1)
- [Q2: What causes a deadlock in the following code snippet?
public class ThreadDepreca](#q2)
- [Q3: Can wait/notifyAll be used in the above code instead of the deprecated resume/su](#q3)
- [Q4: How do you stop a thread that waits for long on say I/O?](#q4)
- [Q5: What java.lang.Thread.interrupt() does when invoked?](#q5)

---

## 🔹 Q1: Why are Thread.stop, Thread.suspend and Thread.resume depr ecated ?

**Answer:**

We already learned that
“In Java programming, each object has a lock. A thread can acquire the lock for an object by using the synchronized keyword.”
“The JVM uses locks in conjunction with monitors. A monitor is basically a guardian who watches over a sequence of synchronized code and making sure
only one thread at a time executes a synchronized piece of code. Each monitor is associated with an object reference.”
1) Thread.stop is deprecated because it’ s unsafe as it causes the thread to release all the acquired monitors and throw a ThreadDeath error, which ultimately
causes the thread to die. When the thread suddenly releases all the acquired monitors, it may leave a few objects whose monitors were acquired by the thread in
an inconsistent state. Such objects are called damaged objects, resulting in arbitrary behavior .
2) Thread.suspend & Thread.resume are deprecated as they may cause a deadlock. The reason for this is that the suspend() method doesn’ t release the
acquired monitors and if a suspended thread has already acquired the monitor of a critical resource, and then an attempt to acquire the “same resource” in a
separate thread which would resume the suspended tar get thread will cause a deadlock.

---

## 🔹 Q2: What causes a deadlock in the following code snippet?
public class ThreadDeprecatedMethods {
 static MyObject myObj = new MyObject ();
 volatile static boolean done = false ;
 public static void main (String args[]) throws InterruptedException {
 Thread t = new Thread () {
 public void run() {
 while (!done ) {
 myObj .println ("looping" ); //thread "t" acquires the monitor on myObj
 }
 }
 };
 t.start();

**Answer:**

Firstly, “suspend” and “resume” methods are deprecated and should not be used.
Secondly, suspend does not release the acquired monitor on “myObj”. So, the main thread gets blocked at “myObj.println(“resuming”);”.
For the above program to work, you need to either comment out “myObj.println(“resuming”);” OR ” myObj.println(“looping”); .

---

## 🔹 Q3: Can wait/notifyAll be used in the above code instead of the deprecated resume/suspend?

**Answer:**

Here is the revised code. Note that wait/notify calls need to be within a synchr onized block. //main thread execution
 Thread .sleep (1000 );
 
 System.out.println ("Suspending thread t" );
 t.suspend (); // suspend does not release the acquired monitor on "myObj"
 
 myObj .println ("resuming" ); // main thread deadlocks here
 // as Thread "t" holds the monitor to "myObj"
 
 System.out.println ("resuming thread t" );
 t.resume ();
 done = true;
 }
 static class MyObject {
 public void println (String msg) {
 synchronized (this) {
 System.out.println (msg);
 }
 }
 }
public class ThreadDeprecatedMethods {

 static MyObject myObj = new MyObject ();
 volatile static boolean done = false ;
 public static void main (String args[]) throws InterruptedException {
 Thread t = new Thread () {
 public void run() {
 while (!done ) {
 myObj .println ("looping" ); //thread "t" acquires the monitor on myObj
 try {
 Thread .sleep (100);
 } catch (InterruptedException e) {
 e.printStackT race();
 }
 }
 }
 };
 t.start();
 
 Thread .sleep (1000 );
 
 synchronized (myObj ) {
 myObj .wait(); //wait to be notified
 myObj .println ("resuming" ); 
 }
 
 Thread .sleep (1000 );
 
 done = true;
 }
 static class MyObject {
 public void println (String msg) {
 synchronized (this) {
 System.out.println (msg);
 this.notifyAll (); // notify threads that are waiting for a lock on "MyObject"
 }
 }
 }

Learn more about wait/notify at Producer and Consumer Java Multi-threading code .

---

## 🔹 Q4: How do you stop a thread that waits for long on say I/O?

**Answer:**

stop() method is deprecated and should not be used.
Appr oach 1: interrupt()
Cancels the task after 5 seconds.
mport java.util.concurrent .TimeUnit ;
public class StopLongRunningThread {
 
 public static void main (String args[]) throws InterruptedException {
 
 long start = System .currentT imeMillis ();
 
 Thread t = new Thread () {
 
 public void run() {
 boolean moreBytesT oRead = true;
 
 while (!isInterrupted () && moreBytesT oRead) {
 //just to simulate long running scenario
 System.out.println("A long running task");
 moreBytesT oRead = true; 
 }
 }
 };
 t.start();
 
 //main thread execution
 long end = System .currentT imeMillis ();
 while (end - start < TimeUnit .SECONDS .toMillis (5)) {

Appr oach 2: volatile boolean flag TimeUnit .MILLISECONDS .sleep (500);
 end = System .currentT imeMillis ();
 }

 System.out.println ("More than 5 seconds elapsed" );
 t.interrupt ();
 }
mport java.util.concurrent .TimeUnit ;
public class StopLongRunningThread {
 
 volatile static boolean cancelled = false ;
 
 public static void main (String args[]) throws InterruptedException {
 
 long start = System .currentT imeMillis ();
 
 Thread t = new Thread () {
 
 public void run() {
 boolean moreBytesT oRead = true;
 
 while (!cancelled && moreBytesT oRead) {
 //just to simulate long running scenario
 System.out.println("A long running task");
 moreBytesT oRead = true; 
 }
 }
 };

---

## 🔹 Q5: What java.lang.Thread.interrupt() does when invoked?

**Answer:**

Thread.interrupt() is a gentle way to tell a thread to exit cleanly, as opposed to Thread.stop(), which kills a thread leading to unpredictable behaviours.
Thread.interrupt() sets the interrupted status/flag of the tar get thread. Then code running in that tar get thread may poll the interrupted status with
“isInterrupted() ” as shown above and handle it appropriately. Some methods that block such as Object.wait() or sleep(10) may consume the interrupted status
immediately and throw an InterruptedException .
Interruption in Java is not pre-emptive, which means both threads have to cooperate in order to process the interrupt properly. If the tar get thread does not poll
the interrupted status the interrupt is ef fectively ignored. In the above example both the thread “t” and thread “main” had to cooperate. java.nio throws
“ClosedByInterruptException”.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » t.start();
 //main thread execution
 long end = System .currentT imeMillis ();
 while (end - start < TimeUnit .SECONDS .toMillis (5)) {
 TimeUnit .MILLISECONDS .sleep (500);
 end = System .currentT imeMillis ();
 }

 System.out.println ("More than 5 seconds elapsed" );
 cancelled = true;
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