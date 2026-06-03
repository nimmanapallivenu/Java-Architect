# Executor Framework

> **Module**: Multi-threading  
> **Topic**: Executor Framework

---

## 📋 Table of Contents



- [Q01: What is an Executor Framework?](#q01)
- [Q02: What does the following code do? What will be the output?
mport java.util.concur](#q02)
- [Q03: How will you change the above code so that the result from each task is returned](#q03)
- [Q04: What are Future and FutureT ask?](#q04)
- [Q05: In Q3, the main thread is blocked on f1.get() and f2.get(). How will you fix the](#q05)
- [Q06: What if you are allowed to display a default result if the computation was takin](#q06)
- [Q07: What are the differences between a Callable and a Supplier ?](#q07)

---

## 🔹 Q01: What is an Executor Framework?

**Answer:**

In Java 5, Executor framework was introduced with the java.util.concurrent. Executor interface. This is a framework for
1) facilitating thread-pools .
2) standardizing invocation, scheduling, execution, and control of asynchronous tasks according to a set of execution policies.
It has ExecutorService & Executors .
ExecutorService provides different methods to start a thread.
1) execute(..) is for threads which are Runnable
2) submit(..) is for threads that are Callable.
3) invokeAny(…) takes a collection of Callable objects. Invoking this method does not return a Future, but returns the result of Callable objects that have
finished executing.
4) invokeAll(…) method takes a collection of Callable objects, and returns a list of Future objects via which you can obtain the results of the executions of each
Callable.
Executors is a factory that provides the methods to return ExecutorService, ScheduledExecutorService, etc.
1) newFixedThr eadPool() returns the pool with fixed number of threads
2) newScheduledThr eadPool() returns a fixed pool with a scheduling capabilitity
3) newCachedThr eadPool() returns a pool that creates threads at runtime and if there is no task threads will die after 60 seconds.

---

## 🔹 Q02: What does the following code do? What will be the output?
mport java.util.concurrent .ExecutorService ;
mport java.util.concurrent .Executors ;

**Answer:**

Two tasks will be executed by 2 threads and then outputsmport java.util.concurrent .TimeUnit ;
public class SimpleT ask implements Runnable {
 public static void main (String [] args) throws InterruptedException {
 ExecutorService es = Executors .newFixedThreadPool (2);
 SimpleT ask task1 = new SimpleT ask(); // runnable task
 SimpleT ask task2 = new SimpleT ask(); // runnable task
 es.execute (task1 );
 es.execute (task2 );
 System.out.println (Thread .currentThread ().getName () + " I am not blocked" );
 
 //terminate the pool after 5 seconds
 es.awaitT ermination (5L, TimeUnit .SECONDS );
 es.shutdown ();
 }
 @Override
 public void run() {
 try {
 TimeUnit .SECONDS .sleep (2);
 } catch (InterruptedException e) {
 e.printStackT race();
 }
 System.out.println (Thread .currentThread ().getName () + " result=" + 123); // fake result 
 }

---

## 🔹 Q03: How will you change the above code so that the result from each task is returned to the main thread and then the result is the sum of both task results?

**Answer:**

The Callable interface returns a value like Integer, Customer, etc. Also notice that the main thr ead is blocked until the r esults are computed. Q5
covers non-blocking code.main I am not blocked
pool-1-thread -1 result =123
pool-1-thread -2 result =123
mport java.util.concurrent .Callable ;
mport java.util.concurrent .ExecutionException ;
mport java.util.concurrent .ExecutorService ;
mport java.util.concurrent .Executors ;
mport java.util.concurrent .Future ;
mport java.util.concurrent .TimeUnit ;
public class SimpleT ask implements Callable <Integer > {
 public static void main (String [] args) throws InterruptedException, ExecutionException {
 ExecutorService es = Executors .newFixedThreadPool (2);
 SimpleT ask task1 = new SimpleT ask();
 SimpleT ask task2 = new SimpleT ask();
 Future <Integer > f1 = es.submit (task1 );
 Future <Integer > f2 = es.submit (task2 );
 Integer result = f1.get() + f2.get(); //blocking call as wait for the results
 System.out.println (Thread .currentThread ().getName () + " I was blocked until the results are computed" );
 System.out.println (Thread .currentThread ().getName () + " result=" + result );
 es.awaitT ermination (5L, TimeUnit .SECONDS );
 es.shutdown ();
 }
 @Override

Output:

---

## 🔹 Q04: What are Future and FutureT ask?

**Answer:**

As shown in the above code, a Futur e is a result of an asynchr onous computation. Future checks if a task is complete and if completed it gets the
output. Future is a general concurrency abstraction, also known as a promise, which promises to return a result in future. public Integer call() throws InterruptedException {
 System.out.println (Thread .currentThread ().getName () + " Started" );
 Thread .sleep (2000 ); // 2000ms processing time
 System.out.println (Thread .currentThread ().getName () + " Done" );
 return 123; // fake result
 }
pool-1-thread -1 Started
pool-1-thread -2 Started
pool-1-thread -2 Done
pool-1-thread -1 Done
main I was blocked until the results are computed
main result =246
 while (!f1.isDone () && !f2.isDone()){
 System.out.println(Thread.currentThread().getName() + " waiting for the results");
 } 
 
 Integer result = f1.get() + f2.get();

you can also check if a future is cancelled with f1. isCancelled ().
Futur eTask is the underlying implementation of the Futur e interface, which is also a cancellable asynchronous computation. It can cancel the task which is
running. Once the FutureT ask will be cancelled, it cannot be restarted. So, it is also possible to wrap Callable or Runnable in a FutureT ask. You would only need
to use a FutureT ask if you want to change its behaviour or access its Callable later. For 99% of use cases, just use Callable and Future as shown above without
wrapping it in FutureT ask as shown below .

---

## 🔹 Q05: In Q3, the main thread is blocked on f1.get() and f2.get(). How will you fix the code to unblock?

**Answer:**

In Java 8, CompletableFutur e was added, which allows execution of code upon completion. Since future.get() blocks, we have to wait for the result to
become available.
What if ther e was a way we could tell the futur e object to execute some code whenever its r eady?
This is what the CompletableFutur e then….() and when….() methods like thenAccept(), thenComposeAsync(), thenComposeAsync(), etc do as shown
below. ExecutorService es = Executors .newFixedThreadPool (2);
 SimpleT ask task1 = new SimpleT ask(); //callable task
 SimpleT ask task2 = new SimpleT ask(); //callable task
 //wrap in FutureT ask
 FutureT ask<Integer > fututeT ask1 = new FutureT ask<Integer >(task1 );
 FutureT ask<Integer > fututeT ask2 = new FutureT ask<Integer >(task2 );
 es.submit (fututeT ask1);
 es.submit (fututeT ask2);
 Integer result = fututeT ask1.get() + fututeT ask2.get();

mport java.util.concurrent .CompletableFuture ;
mport java.util.concurrent .ExecutionException ;
mport java.util.concurrent .ExecutorService ;
mport java.util.concurrent .Executors ;
mport java.util.concurrent .TimeUnit ;
mport java.util.function .Supplier ;
public class SimpleT ask implements Supplier <Integer > {
 public static void main (String [] args) throws InterruptedException, ExecutionException {
 ExecutorService es = Executors .newFixedThreadPool (2);
 SimpleT ask task1 = new SimpleT ask(); //Supplier
 SimpleT ask task2 = new SimpleT ask(); //Supplier
 
 CompletableFuture <Integer > cf1 = CompletableFuture .supplyAsync (task1, es); // execution started
 CompletableFuture <Integer > cf2 = CompletableFuture .supplyAsync (task2, es); // execution started
 CompletableFuture <Integer > resultCf = cf1.thenCombineAsync (cf2, (val1, val2) -> (val1 + val2));
 
 resultCf .whenCompleteAsync ((result, throwable ) -> {
 System.out.println (Thread .currentThread ().getName () + " result=" + result ); //non-blocking as invoked when the result is ready
 }, es);
 
 System.out.println (Thread .currentThread ().getName () + " I am not blocked" );
 System.out.println (Thread .currentThread ().getName () + " other tasks can be performed here..." );
 //wait 5 seconds before terminating the thread-pool
 es.awaitT ermination (5L, TimeUnit .SECONDS );
 es.shutdown ();
 }
 @Override
 public Integer get() {
 try {
 System.out.println (Thread .currentThread ().getName () + " Started" );
 Thread .sleep (2000 ); // 2000ms processing time
 } catch (InterruptedException e) {
 e.printStackT race();
 }
 System.out.println (Thread .currentThread ().getName () + " Done" );

Output:
As you can see:
1. CompletableFutures can be chained with methods like “ thenCombineAsync “.
Source: http://kennethjor gensen.com/blog/2016/introduction-to-completablefutures
2. The method “whenComplete ” gets called when the task is completed without blocking the main thread. 
 return 123; // fake result
 }
pool-1-thread -1 Started
pool-1-thread -2 Started
main I am not blocked
main other tasks can be performed here...
pool-1-thread -2 Done
pool-1-thread -1 Done
pool-1-thread -2 result =246

3) we can manually “ complete ” a CompletableFuture, and this feature is not found with the classical Future interface.

---

## 🔹 Q06: What if you are allowed to display a default result if the computation was taking too long?

**Answer:**

You can invoke the “complete(default)” method as demonstrated below .
mport java.util.concurrent .CompletableFuture ;
mport java.util.concurrent .ExecutionException ;
mport java.util.concurrent .ExecutorService ;
mport java.util.concurrent .Executors ;
mport java.util.concurrent .TimeUnit ;
mport java.util.function .Supplier ;
public class SimpleT ask implements Supplier <Integer > {
 public static void main (String [] args) throws InterruptedException, ExecutionException {
 ExecutorService es = Executors .newFixedThreadPool (2);
 SimpleT ask task1 = new SimpleT ask(); // Supplier
 SimpleT ask task2 = new SimpleT ask(); // Supplier
 CompletableFuture <Integer > cf1 = CompletableFuture .supplyAsync (task1, es); // execution started
 CompletableFuture <Integer > cf2 = CompletableFuture .supplyAsync (task2, es); // execution started
 CompletableFuture <Integer > resultCf = cf1.thenCombineAsync (cf2, (val1, val2) -> (val1 + val2));
 if (resultCf .isDone ()) { // not blocked
 System.out.println (Thread .currentThread ().getName () + " result computed = " + resultCf .get());
 }
 System.out.println (Thread .currentThread ().getName () + " I am not blocked" );
c
 System.out.println (Thread .currentThread ().getName () + " other tasks can be performed here..." );
 //Thread.sleep(3000); //try uncomenting this
 
 boolean isDefault = resultCf .complete (5); // default is 5 if the result is still not ready
 if (isDefault ) {
 System.out.println (Thread .currentThread ().getName () + " result default = " + resultCf .get()); // not blocked

Output:
If you uncomment the “Thread.sleep(3000);” line } else {
 System.out.println (Thread .currentThread ().getName () + " result computed = " + resultCf .get()); // not blocked
 }
 // wait 5 seconds before terminating the thread-pool
 es.awaitT ermination (5L, TimeUnit .SECONDS );
 es.shutdown ();
 }
 @Override
 public Integer get() {
 try {
 System.out.println (Thread .currentThread ().getName () + " Started" );
 Thread .sleep (2000 ); // 2000ms processing time
 } catch (InterruptedException e) {
 e.printStackT race();
 }
 System.out.println (Thread .currentThread ().getName () + " Done" );
 return 123; // fake result
 }
pool-1-thread -1 Started
pool-1-thread -2 Started
main I am not blocked
main other tasks can be performed here...
main result default = 5
pool-1-thread -2 Done
pool-1-thread -1 Done

Above code demonstrated that we can manually “ complete ” a CompletableFuture without having to block.

---

## 🔹 Q07: What are the differences between a Callable and a Supplier ?

**Answer:**

Callable was created as part of the java.util. concurr ent package since Java 5. Supplier was created as part of the java.util. function package.
A Callable is an action that can be executed in another thread, and that allows you to inspect its side ef fects as a result of its execution. A Supplier on the other
hand is a function on which you rely for supplying objects of some type.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »pool-1-thread -1 Started
pool-1-thread -2 Started
main I am not blocked
main other tasks can be performed here...
pool-1-thread -2 Done
pool-1-thread -1 Done
main result computed = 246

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