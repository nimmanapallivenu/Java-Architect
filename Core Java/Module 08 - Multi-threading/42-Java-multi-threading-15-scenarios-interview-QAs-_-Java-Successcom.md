# 42. Java multi threading 15 scenarios interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: Can you give some scenarios where you had used multi-threading in Java applicati](#q1)

---

## 🔹 Q1: Can you give some scenarios where you had used multi-threading in Java applications?

**Answer:**

Scenario 1: Servlets are inherently multi-threaded
and each user will be using a thread from the thread pool. The number of threads are configured via the web container of the application server . Servlet 3.1
supports non-blocking I/O for better throughput.
What is wr ong with the thr ead-per -request model?
Pre Servlet 3.1 uses thread-per -request model, which limits the number of concurrent connections to the number of concurrently running JVM threads. Every
thread introduces significant increase of memory footprint and CPU utilization via context switches. Servlet 3.1 rectifies this via non-blocking I/O. Fewer
threads can be used in a pool to execute the request. NIO allows you to manage multiple channels (network connections or files) using only a single (or fewer)
threads.

Scenario 2: A MINA (i.e. a non-blocking I/O based) server with low level TCP protocol
to service 250+ petrol sites. The server supports the pay at the pump solution. The clients are C++ based and send fuel and credit card details to the server . A
pool of reusable threads say 30 can be used to handle concurrent transactions.

Scenario 3: A Swing programmer deals with the following kinds of threads:
a) Initial threads that execute the initial application code.
b) The event dispatch thread, where all event-handling code is executed. Most code that interacts with the Swing framework must also execute on this thread.
c) Worker threads, also known as background threads, where time-consuming background tasks are executed. For example, loading an image, retrieving and
caching the data, processing any time consuming logic, etc.
Scenario 4: Asynchronous processing by spawning a worker thread
An online application with a requirement to produce time consuming reports or a business process (e.g. rebalancing accounts, aggregating hierachical
information, etc) could benefit from making these long running operations asynchronous. These tasks are performed on a separate worker thread. Once the
reports or the long running business process is completed, the outcome can be communicated to the user via emails or asynchronously refreshing the web page
via techniques known as “server push” or “client pull”. A typical example would be
a) A user makes a request for an aggregate report or a business process like rebalancing his/her portfolios.

b) The user input can be saved to a database table for a separate process to periodically pick it up and process it asynchronously .
c) The user could now continue to perform other functionality of the website without being blocked.
d) A separate process running on the same machine or different machine can periodically scan the table for any entries and produce the necessary reports or
execute the relevant business process. This could be a scheduled job that runs once during of f-peak or every 10 minutes. This depends on the business
requirement.
e) Once the report or the process is completed, notify the user via emails or making the report available online to be downloaded.
A CountDownLatch can be used to wait on multiple threads performig different tasks. Once CountDownLatch reaches zero, the waiting threads can be
released. For example 3 separate threads populating the header , body , and footer sections. The CountDownLatch starts from 3.
Asynchronous (i.e. non-blocking) processing in Java examples
1. Asynchronous processing in Java real life examples – part-1
2. Asynchronous processing in Java real life examples – part-2
Scenario 5: Writing your own editor in Java where syntax highlighting is performed on a separate thread
To maximize the performance of the application, the CPU intensive syntax highlighting can be carried out on a separate worker thread whilst the user can use
the editor .
Scenario 6: Java 7 fork and join to process computation intensive algorithms on a multi-core machine
Example . numbers = {1,2,3,4,5,6,7,8,9,10}, sum = 55; process them using the fork and join feature introduced in Java 7.

Pseudo Code for Fork, Compute and Join
Result solve (Problem problem ) {
 if (problem is small ) //e.g. smaller than batch size
 compute the problem
 else {
 split problem into chunks
 fork new subtasks to solve each chunk
 join all subtasks
 compose result from subresults
 }

Here is the Java code
package com.fork.join;
mport java.util.ArrayList ;
mport java.util.Arrays ;
mport java.util.List;
mport java.util.concurrent .RecursiveT ask;
class SumT ask extends RecursiveT ask<Integer > {
tatic final int CHUNK_SIZE = 3; // execution batch size;
nteger [] numbers ;
nt begin ;
nt end;
SumT ask(Integer [] numbers , int begin , int end) {
this.numbers = numbers ;
this.begin = begin ;
this.end = end;
@Override
protected Integer compute () {
//sums the given number
if (end - begin <= CHUNK_SIZE ) {
 int sum = 0;
 List<Integer > processedNumbers = new ArrayList <>();
 for(int i=begin ; i < end; ++i) {
 processedNumbers .add(numbers [i]);//just to track
 sum += numbers [i];
 }
 //tracking thread, numbers processed, and sum
 System .out.println (Thread .currentThread ().getName () + " proceesing " +

Here is the test class with the main method Arrays .asList (processedNumbers ) + ", sum = " + sum);
 return sum;
}
//create chunks, fork and join
else {
 int mid = begin + (end - begin ) / 2; //mid point to partition
 SumT ask left = new SumT ask(numbers , begin , mid); //left partition
 SumT ask right = new SumT ask(numbers , mid, end); //right partition
 left.fork(); //asynchronously execute on a separate thread
 int leftAns = right .compute (); //recurse and compute
 int rightAns = left.join(); //returns the asynchronously executed result
 System .out.println ("leftAns=" + leftAns + " + " + "rightAns=" + rightAns );
 return leftAns + rightAns ; 
}
package com.fork.join;
mport java.util.concurrent .ForkJoinPool ;
public class SumT askTest {
public static void main (String [] args) {
 int numberOfCpuCores = Runtime .getRuntime ().availableProcessors ();
 ForkJoinPool forkJoinPool = new ForkJoinPool (numberOfCpuCores );
 Integer [] numbers = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
 int sum = forkJoinPool .invoke (new SumT ask(numbers , 0, numbers .length ));
 System .out.println (sum);

The output
Q. Where to use fork/join as opposed to using the ExecutorService framework?
A. The Fork/Join Framework in Java 7 is designed for work that can be broken down into smaller tasks and the results of those tasks combined to produce the
final result. Multicore processors are now widespread across server , desktop, and laptop hardware. They are also making their way into smaller devices, such as
smartphones and tablets. Fork/Join of fers serious gains for solving problems that involve recursion .
The fork/join tasks should operate as “pure” in-memory algorithms in which no I/O operations come into play . Also, communication between tasks through
shared state should be avoided as much as possible, because that implies that locking might have to be performed. Ideally , tasks communicate only when one
task forks another or when one task joins another .
ExecutorService continues to be a fine solution for many concurrent programming tasks, and in programming scenarios in which recursion is vital to processing
power , it makes sense to use Fork/join. This fork and join feature is used in Java 8 parallel stream processing with lambda expressions.
Scenario 7: RESTful service processing the request asynchronously by spawning a new threadForkJoinPool -1-worker -1 proceesing [[6, 7]], sum = 13
ForkJoinPool -1-worker -3 proceesing [[8, 9, 10]], sum = 27
eftAns =27 + rightAns =13
ForkJoinPool -1-worker -2 proceesing [[3, 4, 5]], sum = 12
ForkJoinPool -1-worker -0 proceesing [[1, 2]], sum = 3
eftAns =12 + rightAns =3
eftAns =40 + rightAns =15
55

Configure the thread executor service via Spring JavaConfig.@Controller
@RequestMapping (value = "/v1/myapp/processor" , produces = { MediaT ype.APPLICA TION_JSON , MediaT ype.APPLICA TION_XML })
public class MyAppEndpointControllerImpl implements MyAppEndpointController {
 @Inject
 @Named ("replayExecutor" )
 private Executor replayExecutor ;
 
 @Inject
 private MyAppReplayService replayService ;
 @Override
 @ResponseStatus (HttpStatus .ACCEPTED )
 @RequestMapping (value = "/replay" , method = RequestMethod .PUT )
 public void replayPendingRequests (
 @RequestParam (required = false ) final Integer maxReplayEntries ) {
 replayExecutor .execute (new Runnable () {
 @Override
 public void run() {
 LOG .info("replayPendingRequests() invoked using the following parameters: {}" , maxReplayEntries );
 replayService .replayPendingRequests (maxReplayEntries );
 }
 });
 }
 
@Configuration
@EnableWebMvc
@Import ({
 MyAppServiceConfiguration .class
)
@ComponentScan ("com.mgl.wrap.ecm.enabler .endpoint" )

4 Mor e scenario based
Questions and Answers on Java multi-threading

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

