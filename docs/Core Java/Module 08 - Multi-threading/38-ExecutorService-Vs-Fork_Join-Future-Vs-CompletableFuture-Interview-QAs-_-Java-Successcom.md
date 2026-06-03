# 38. ExecutorService Vs Fork Join Future Vs CompletableFuture Interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: What is the difference between “ ExecutorService ” and “ Fork/Join Framework “?](#q1)
- [Q2: What do you understand by the term “ asynchr onous ” processing?](#q2)
- [Q3: Why was CompletableFuture class introduced in Java 8 to work with executor servi](#q3)
- [Q4: What is a Future interface, and why was it introduced in Java?](#q4)
- [Q5: Why was CompletableFutur e introduced in Java 8 when you already had the Future ](#q5)
- [Q6: What is the difference between the calls thenAccept Async (…) and thenAccept(….](#q6)
- [Q7: What is a task from the above examples that is passed to a CompletableFuture obj](#q7)
- [Q8: The CompletableFuture takes a task and optionally an executor service as ar gume](#q8)
- [Q9: What do you understand by the term reactive pr ogramming (RP)?](#q9)

---

## 🔹 Q1: What is the difference between “ ExecutorService ” and “ Fork/Join Framework “?

**Answer:**

The Fork/Join framework uses a special kind of thread pool known as the ForkJoinPool , which is a specialized implementation of ExecutorService
implementing the 1) work-stealing algorithm, in which the idle workers steal the work from those workers who are busy . This can give better performance.
ForkJoinPool W ork-Stealing
The Fork-Join breaks the task at hand into mini-tasks until the mini-task is simple enough that it can be solved without further breakups. The pseudo-code is
like:

The actual working example can be found at Java multi-threading scenarios interview Q&A .
Another difference in ForkJoinPool compared to an ExecutorService is that, even though you specify an initial capacity , the
2) ForkJoinPool adjusts its pool size dynamically in an attempt to maintain enough active threads at any given point in time.
3) ForkJoinPool threads are all daemon threads, hence its pool does not have to be explicitly shutdown as we do for executor service
“executorService.shutdown()”;

---

## 🔹 Q2: What do you understand by the term “ asynchr onous ” processing?

**Answer:**

Asynchr onous task means its processing returns before the task is finished, and generally causes some work to happen in the background before triggering
some futur e action in the application to get the results or handle the exception via one of the following mechanisms:
1) Callback argument
2) Return place holder . e.g. Futur e, Promise, etc.
3) Deliver to a queue.

---

## 🔹 Q3: Why was CompletableFuture class introduced in Java 8 to work with executor service when you already have “ForkJoinPool” that gives good
performance?

**Answer:**

The use cases for the fork/join are pretty narrow . It can only be used when the following conditions are met.
1) The data-set is large & efficiently splittable.
2) The operations performed on individual data items should be reasonably independent of each other .Result solve (Problem problem ) {
 if (problem is small ) //e.g. smaller than batch size
 compute the result
 else {
 split problem into chunks
 fork new subtasks to solve each chunk
 join all subtasks
 compose result from subresults
 }

3) The operations should be expensive & CPU intensive.
When the above conditions do not hold, and your use case is more I/O or network intensive then use ExecutorService with CompletableFutur e.

---

## 🔹 Q4: What is a Future interface, and why was it introduced in Java?

**Answer:**

The Future<V> represents the result of an asynchr onous computation . The result is known as a future because the results will not be available until some
moment in the future. You can invoke methods on the Future interface
1) Get the results by blocking indefinitely or for a timeout to elapse when the task hasn’ t finished.
2) Cancel a task.
3) Determine if a task has been cancelled or completed.
The 3 variants of submitting a task to the ExeccutorService returns a Future object.
In Java 8, the Runnable & Callable interfaces are annotated with “ @FunctionalInterface “. The major benefit of functional interface is that we can use lambda
expr essions to instantiate them and avoid using bulky anonymous class implementation.Future submit (Callable task)
Future submit (Runnable task)
Future submit (Runnable task, T result )
mport java.util.concurrent .ExecutionException ;
mport java.util.concurrent .ExecutorService ;
mport java.util.concurrent .Executors ;
mport java.util.concurrent .Future ;
mport java.util.function .Function ;

Outputs:mport java.util.function .IntToDoubleFunction ;
public class FutureExample {
 static class Recursive <I> {
 public I func;
 }
 static Function <Integer , Double > factorial = x -> {
 Recursive <IntToDoubleFunction > recursive = new Recursive <IntToDoubleFunction >();
 recursive .func = n -> (n == 0) ? 1 : n * recursive .func.applyAsDouble (n - 1);
 
 return recursive .func.applyAsDouble (x);
 };
 public static void main (String [] args) throws InterruptedException , ExecutionException {
 System .out.println (Thread .currentThread ().getName () + " thread enters main method" ); // 1
 ExecutorService newFixedThreadPool = Executors .newFixedThreadPool (1);
 final int nthFactorial = 25;
 Future <Double > result = newFixedThreadPool .submit (() -> {
 System .out.println (Thread .currentThread ().getName () + " factorial task is called" ); // 2
 Double factorialResult = factorial .apply (nthFactorial );
 return factorialResult ;
 });
 System .out.println ("isDone = " + result .isDone ());
 System .out.println ("isCancelled = " + result .isCancelled ());
 // result.cancel(true); //You may cancel it conditionally
 Double res = result .get(); // 3 blocked until result is returned
 System .out.println (Thread .currentThread ().getName () + " result is " + res); // 4
 newFixedThreadPool .shutdown ();
 }

Note : The “factorial” static function is explained at Top 6 tips to transforming your thinking from OOP to FP with examples
As you can see, there is a main thr ead, which spawns a new thr ead to asynchr onously find out the 25th factorial. The factorial function is a time consuming
function, and then main thread continues without being blocked whilst the factorial task is being executed in the background. The main thread gets blocked
when it reaches the “ get() ” method call on the Future object “result” at line “//3”. Finally , the main thread prints the result of 25th factorial.

---

## 🔹 Q5: Why was CompletableFutur e introduced in Java 8 when you already had the Future interface?

**Answer:**

The CompletableFuture implement both CompletionStage <<T> and Futur e<T> interfaces. Hence it provides the functionality of the Future interface in
terms of getting the result, canceling a task, and determining if a task is completed or cancelled, etc. Since it also implements the “CompletionStage” interface it
provides 1) functionality to join & combine CompletableFuture objects and recover from exceptional scenarios.
2) The “ Futur e.get() ” is a blocking call. Whereas, given a CompletableFuture “f” executing a task, you can both synchronously and asynchronously run another
task as a callback upon completion of “f”.
If you need a r esult:
If you don’t need a r esult:main thread enters main method
pool-1-thread -1 factorial task is called
sDone = false
sCancelled = false
main result is 1.551 1210043330986E25
f.thenApply (result -> isDone (result )); // sync callback
f.thenApplyAsync (result -> isDone (result )); // async callback

The same logic as above for “FutureExample” using a CompletableFutur e object:f.thenRun (() -> isDone ()); // sync callback
f.thenRunAsync (() -> isDone ()); // async callback
mport java.util.concurrent .CompletableFuture ;
mport java.util.concurrent .ExecutionException ;
mport java.util.concurrent .ExecutorService ;
mport java.util.concurrent .Executors ;
mport java.util.function .Function ;
mport java.util.function .IntToDoubleFunction ;
public class FutureExample {
 static class Recursive <I> {
 public I func;
 }
 static Function <Integer , Double > factorial = x -> {
 Recursive <IntToDoubleFunction > recursive = new Recursive <IntToDoubleFunction >();
 recursive .func = n -> (n == 0) ? 1 : n * recursive .func.applyAsDouble (n - 1);
 return recursive .func.applyAsDouble (x);
 };
 public static void main (String [] args) throws InterruptedException , ExecutionException {
 System .out.println (Thread .currentThread ().getName () + " thread enters main method" ); // 1
 ExecutorService newFixedThreadPool = Executors .newFixedThreadPool (1);
 final int nthFactorial = 25;

Chaining CompletionFutur e Example
As shown below , new CompletionFuture objects can be created from another and also chained as demonstrated below at //1, //2, //3, and //4. CompletableFuture <Double > result = CompletableFuture .supplyAsync (() -> {
 System .out.println (Thread .currentThread ().getName () + " factorial task is called" ); // 2
 Double factorialResult = factorial .apply (nthFactorial );
 return factorialResult ;
 }, newFixedThreadPool );
 System .out.println ("isDone = " + result .isDone ());
 System .out.println ("isCancelled = " + result .isCancelled ());
 // result.cancel(true); //You may cancel it conditionally
 Double res = result .get(); // blocked until result is returned
 System .out.println (Thread .currentThread ().getName () + " result is " + res); // 3
 
 newFixedThreadPool .shutdown ();
 }
mport java.util.concurrent .CompletableFuture ;
mport java.util.concurrent .ExecutionException ;
mport java.util.concurrent .ExecutorService ;
mport java.util.concurrent .Executors ;
mport java.util.function .Function ;
mport java.util.function .IntToDoubleFunction ;
public class FutureExample {
 // recursion declaration

 static class Recursive <I> {
 public I func;
 }
 static Function <Integer , Double > factorial = x -> {
 Recursive <IntToDoubleFunction > recursive = new Recursive <IntToDoubleFunction >();
 recursive .func = n -> (n == 0) ? 1 : n * recursive .func.applyAsDouble (n - 1);
 return recursive .func.applyAsDouble (x);
 };
 static Function <Double , Double > square = x -> {
 return x * x;
 };
 public static void main (String [] args) throws InterruptedException , ExecutionException {
 System .out.println (Thread .currentThread ().getName () + " thread enters main method" );
 ExecutorService newFixedThreadPool = Executors .newFixedThreadPool (5);
 final int nthFactorial = 5;
 // 1 CompletableFuture stage for calculating factorial
 CompletableFuture <Double > factorialCalcStage = CompletableFuture .supplyAsync (() -> {
 System .out.println (Thread .currentThread ().getName () + " factorial task is called" );
 Double factorialResult = factorial .apply (nthFactorial );
 return factorialResult ;
 }, newFixedThreadPool );
 // 2 creates another CompletableFuture.
 factorialCalcStage .thenAcceptAsync (t -> System .out.println (Thread .currentThread ().getName () + " The result after factorial: " + t));
 // 3 creates another CompletableFuture for squaring .
 CompletableFuture <Double > squareCalcStage = factorialCalcStage .thenApplyAsync (r -> {
 System .out.println (Thread .currentThread ().getName () + " square task is called" );
 Double squareResult = square .apply (r);
 return squareResult ;
 }, newFixedThreadPool );
 // 4 creates another CompletableFuture.
 CompletableFuture <Void> lastStage = squareCalcStage .thenAcceptAsync (t -> System .out
 .println (Thread .currentThread ().getName () + " The result after square: " + t));

Outputs:

---

## 🔹 Q6: What is the difference between the calls thenAccept Async (…) and thenAccept(….)?

**Answer:**

In the above example, the thenAccept Async (…) are executed in a separate default thread pool known as the “ForkJoinPool.commonPool” and the order in
which the lines “The result after square:” and “result after factorial: 120.0” vary between the runs. If you want to execute these two calls immediately after
“pool-1-thread-1 factorial task is called” and “pool-1-thread-2 square task is called” in the same thread then use thenAccept (….), in the lines \\2 & \\4 which
will give an output as shown below:
The output will be: lastStage .join(); //waits for the lastStage to complete
 newFixedThreadPool .shutdown ();
 }
main thread enters main method
pool-1-thread -1 factorial task is called
pool-1-thread -2 square task is called
ForkJoinPool .commonPool -worker -2 The result after square : 14400.0
ForkJoinPool .commonPool -worker -1 The result after factorial : 120.0
main thread enters main method
pool-1-thread -1 factorial task is called
pool-1-thread -2 square task is called
pool-1-thread -2 The result after square : 14400.0

Now , if you want to run the “square task” synchronously in the same thread as “factorial task” then you need to make the following 2 changes as as shown to
line //3.
1) thenApplyAsync(.., ..) to thenApply(..);
2) Remove the second argument “newFixedThreadPool” as thenApply(..) only takes a task.
The output will be:pool-1-thread -1 The result after factorial : 120.0
 // 3 creates another CompletableFuture for squaring .
 CompletableFuture <Double > squareCalcStage = factorialCalcStage .thenApply (r -> {
 System .out.println (Thread .currentThread ().getName () + " square task is called" );
 Double squareResult = square .apply (r);
 return squareResult ;
 });
main thread enters main method
pool-1-thread -1 factorial task is called
pool-1-thread -1 square task is called
pool-1-thread -1 The result after square : 14400.0
pool-1-thread -1 The result after factorial : 120.0

---

## 🔹 Q7: What is a task from the above examples that is passed to a CompletableFuture object?

**Answer:**

A task can be
1) A Runnable : that takes no arguments and returns no arguments. “CompletableFuture. runAsync (runnable)”
2) A Consumer : that takes an object as an argument, but returns nothing. “CompletableFuture. thenAcceptAsync (Consumer )”.
3) A Function : That takes an object argument and returns an object result. “CompletableFuture. supplyAsync (Supplier )”. “Supplier” is a function that returns a
value.CompletableFuture <Void> cfRunnableExample = CompletableFuture .runAsync (new Runnable () {
 
 @Override
 public void run() {
 //do something 
 }
});
factorialCalcStage .thenAcceptAsync (t -> System .out.println ("The result after factorial: " + t));
CompletableFuture <Double > factorialCalcStage = CompletableFuture

---

## 🔹 Q8: The CompletableFuture takes a task and optionally an executor service as arguments. What happens only if a task is supplied without an executor service?

**Answer:**

By default supplyAsync() uses ForkJoinPool.commonPool() thread pool shared between all CompletableFutures. This implicitly is a hard-coded thread
pool that is completely outside of your control. So, you should always specify your own ExecutorService as shown in the above examples as in
“newFixedThr eadPool “.

---

## 🔹 Q9: What do you understand by the term reactive pr ogramming (RP)?

**Answer:**

The text book definition is that:
“Reactive programming is programming with asynchronous data streams.”
As we saw earlier in the CompletableFutur e methods like supplyAsync(…), thenApplyAsync(….), thenCombineAsync(….), thenApply(), etc where you can
join (i.e. chain) and combine CompletableFuture stages to build a pipeline of later running tasks. These tasks can run asynchronously and provide
synchronization methods to join or combine split parallel tasks. This helps you to parallelize your code to make an application more reactive and responsive.
Reactive programming example
1) Lines //1 & //2 are long running asynchronous tasks that run in parallel. Long run is simulated with 5 seconds sleep.
2) Line //3 is executed when //1 & //2 are completed. .supplyAsync (() -> {
 System .out.println (Thread .currentThread ().getName ()
 + " factorial task is called" );
 Double factorialResult = factorial .apply (nthFactorial );
 return factorialResult ;
 }, newFixedThreadPool );
mport java.util.ArrayList ;
mport java.util.Arrays ;
mport java.util.concurrent .CompletableFuture ;
mport java.util.concurrent .TimeUnit ;

Outputs:public class ReactiveProgramming {
 public static void main (String [] args) {
 // 1. long running asynchronous task
 final CompletableFuture <ArrayList <String >> users = CompletableFuture .supplyAsync (() -> {
 // lookup all students ... this can run for a long time
 sleep (5);
 return new ArrayList <String >(Arrays .asList ("John" , "Peter" , "Sam" ));
 });
 // 2. long running asynchronous task
 final CompletableFuture <ArrayList <String >> subjects = CompletableFuture .supplyAsync (() -> {
 // lookup all subjects ... this can run for a long time
 sleep (5);
 return new ArrayList <String >(Arrays .asList ("English, Maths, Science" ));
 });
 //3 combine //1 & //2 to produce a report synchronously
 final CompletableFuture <String > report = users .thenCombine (subjects , (u, s) -> {
 System .out.println ("Thread " + Thread .currentThread ().getName () + " is producing a report'" + "");
 return u.toString () + s.toString ();
 });
 System .out.println (report .join()); // returns a result value when complete
 }
 private static void sleep (long seconds ) {
 try {
 System .out.println ("Thread " + Thread .currentThread ().getName () + " is processing" );
 TimeUnit .SECONDS .sleep (seconds );
 } catch (InterruptedException e) {
 e.printStackT race();
 }
 }

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »Thread ForkJoinPool .commonPool -worker -1 is processing
Thread ForkJoinPool .commonPool -worker -2 is processing
Thread ForkJoinPool .commonPool -worker -2 is producing a report '
John , Peter , Sam][English , Maths , Science ]

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

