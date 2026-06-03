# 39. 7 Java ThreadLocal interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: What is a ThreadLocal class? When to use it? How to not abuse it?](#q1)
- [Q2: Are there any alternatives to using an object or resource pool to minimize their](#q2)
- [Q3: Are SimpleDateFormat and DecimalFormat classes thread-safe in Java?](#q3)
- [Q4: How will you you use them in a thread-safe manner?](#q4)
- [Q5: Can a ThreadLocal cause memory leaks?](#q5)

---

## 🔹 Q1: What is a ThreadLocal class? When to use it? How to not abuse it?

**Answer:**

ThreadLocal is a handy class for simplifying development of thread-safe concurrent programs by making the object stored in this class not sharable
between threads.
When to use?
1) ThreadLocal class encapsulates non-thread-safe classes (e.g. SimpleDateFormat, DecimalFormat, etc) to be safely used in a multi-threaded environment.
2) Allows you to create per -thread-singleton. Many frameworks use ThreadLocal to maintain some context related to the current thread. For example, when the
current transaction is stored in a ThreadLocal, you don’ t need to pass it as a parameter through every method call.
3) Handy in scenarios where you have no control over when the threads are not created by you. For example,
a) The Java main thread, which is created by the JVM
b) Swing dispatcher , Quartz scheduler , Servlet container , and other third party library , framework or container threads not created by you.
How to not abuse it?
Having said this, you must be very careful when using ThreadLocal as they are per thread static/global variable, and can cause memory leaks. ThreadLocal can
cause classloading leaks in applications when used with thread pools in Servlet/Web containers. It can also cause issues in standalone Java applications where
the worker threads stay alive until the program exits.
Things to consider
So, it is imperative to
1) use it judiciously , only where it makes more sense.
2) clean up any ThreadLocals that you get() or set() by using the ThreadLocal’ s remove() method.

---

## 🔹 Q2: Are there any alternatives to using an object or resource pool to minimize their initialization cost?

**Answer:**

Yes, you can use a ThreadLocal object to create an object per thread. This approach is useful when creation of a particular object is not trivial and the
objects cannot be shared between threads. For example, java.util.Calendar , java.text. SimpleDateFormat , application framework contexts like hibernate sessions,
etc are initialization heavy objects, and they need to be initialized and reused. Keep destroying and initializing them can be very expensive.
It is very tempting to create these objects or resources with a static initializer and stick the instance in a static field. These objects/resources are often mutable,
and if they are called from multiple threads at the same time, the internal mutable state will most likely do unexpected things and give you wrong answers. In
simple terms, these will cause thread-safety issues that can be very hard to debug.

ThreadLocal is to the rescue where you can maintain a per thread singleton or per thread static/global variable.

---

## 🔹 Q3: Are SimpleDateFormat and DecimalFormat classes thread-safe in Java?

**Answer:**

No.

---

## 🔹 Q4: How will you you use them in a thread-safe manner?

**Answer:**

Declare it either as a local variable or use the anonymous ThreadLocal inner class if you want to use it across a number of different methods within the
class as shown below .
package test.example ;
mport java.math .BigDecimal ;
mport java.text.DateFormat ;
mport java.text.DecimalFormat ;
mport java.text.NumberFormat ;
mport java.text.SimpleDateFormat ;
mport java.util.Date ;
public class InvestmentBalance {
private static final ThreadLocal <NumberFormat > PERCENT_FORMA T = new ThreadLocal <NumberFormat >() {
@Override
protected NumberFormat initialV alue() {
 return new DecimalFormat ("###.##" );
}
;
private static final ThreadLocal <NumberFormat > DOLLAR_FORMA T = new ThreadLocal <NumberFormat >() {
@Override
protected NumberFormat initialV alue() {
 return new DecimalFormat ("$#,###.####" );
}
;
private static final ThreadLocal <DateFormat > DATE_FORMA T = new ThreadLocal <DateFormat >() {
@Override
protected DateFormat initialV alue() {
 return new SimpleDateFormat ("dd/MMM/yyyy" );

The class with the main method}
;
public String getPercentageOfInvAmount (BigDecimal amount ) {
 return PERCENT_FORMA T.get().format (amount .doubleV alue()) + "%";
public String getDollarInvAmount (BigDecimal amount ) {
 return DOLLAR_FORMA T.get().format (amount .doubleV alue());
public String getFormattedExpiryDate (Date date) {
return DATE_FORMA T.get().format (date);
package test.example ;
mport java.math .BigDecimal ;
mport org.joda.time.DateT ime;
mport static java.lang.System .out;
public class InvestmentBalanceT est {
private static final DateT ime dt;
tatic {
 dt = new DateT ime(2005 , 3, 26, 12, 0, 0, 0);
public static void main (String [] args) {
InvestmentBalance ib = new InvestmentBalance ();
BigDecimal percent = new BigDecimal (35.479 );

Output :

---

## 🔹 Q5: Can a ThreadLocal cause memory leaks?

**Answer:**

If ThreadLocal is not used properly , it causes Memory Leaks because a reference to a ThreadLocal value is kept until the “owning” thread dies or if the
ThreadLocal itself is no longer reachable.
1) As threads can outlive classloaders, you can potentially get classloader leaks resulting in “java.lang.OutOfMemoryError: PermGen ” space. This occurs if
ThreadLocal variables are used inside of Java EE applications running in an application server with thread pools.
java.lang.Class instances and other meta data are stored in the area of JVM memory known as the PermGen space. If you do not clean up with
ThreadLocal. remove() when you are done, any references held to loaded classes when a web application is deployed will remain in the PermGen heap without
being garbage collected. Each successive deployment will create a new instance of the class which will never be garbage collected, and eventually throws
“java.lang.OutOfMemoryError: PermGen ” as too many classes are repeatedly loaded.percent .setScale (2, BigDecimal .ROUND_HALF_EVEN );
BigDecimal amount = new BigDecimal (3525.49423 );
amount .setScale (4, BigDecimal .ROUND_HALF_EVEN );
out.println (ib.getPercentageOfInvAmount (percent ));
out.println (ib.getDollarInvAmount (amount ));
out.println (ib.getFormattedExpiryDate (dt.toDate ()));
35.48 %
$3,525.4942
26/Mar/2005

2) Depending on the number of threads in the pool (> 150 threads are normal in productive environments) and the size of the object in the ThreadLocal variable,
critical memory problems can occur . If for example 200 threads are configured for the thread pool and the ThreadLocal variables are 5MB big, this could result
in 1 GB of heaps space occupied by just these ThreadLocal variables.
3) ThreadLocal can also cause issues in standalone Java applications where the worker threads stay alive until the program exits. You need to remove objects
from the ThreadLocal once done with them.
So, use
1) Thread Local judiciously .
2) When you have to use them, it is imperative to clean up any ThreadLocals that you get() or set() by using the ThreadLocal’ s remove() method.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » try {
 myThreadLocal .set(personA );
 //...
 }
 finally {
 myThreadLocal .remove ();
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

