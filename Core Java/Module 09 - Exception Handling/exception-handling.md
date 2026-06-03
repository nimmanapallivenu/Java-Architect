# Exception Handling Best Practices

> **Module**: Exception Handling  
> **Topic**: Exception Handling Best Practices

---

## 📋 Table of Contents



- [Q1: What is the difference between a runtime exception and a checked exception?](#q1)
- [Q2: What is the problem or benefit of catching or throwing type “java.lang.Exception](#q2)
- [Q3: Does Java’ s exception handling use any design pattern?](#q3)
- [Q4: What are the different ways to look at the trace of your program execution?](#q4)
- [Q5: How would you go about analyzing stack traces correctly?](#q5)

---

## 🔹 Q1: What is the difference between a runtime exception and a checked exception?

**Answer:**

You must either catch or throw a checked exception. The unchecked exception (aka Runtime exception) does not have to be explicitly handled.
Checked Vs Unchecked
Q. So, when to use checked exception, and when to use unchecked exception ?
A. There is no clear cut answer. Document a consistent exception handling strategy. In general favor unchecked (i.e. Runtime) exceptions, which you don’ t have
to handle with catch and throw clauses. Use checked exceptions in a rare scenarios where you can recover from the exception like deadlock or service retries. In
this scenario, you will catch a checked exception like java.io.IOException and wait a few seconds as configured with the retry interval, and retry the service
configured by the retry counts.

---

## 🔹 Q2: What is the problem or benefit of catching or throwing type “java.lang.Exception”?

**Answer:**

Exceptions are polymorphic in nature., which means you need to catch a more specific ones before the generic ones. For example, IOException must be
caught before Exception as the IOException extends Exception. If you catch the Exception before IOException, then IOException catch block will never be
reached as Exception catches everything.
So, it is wrong to catch Exception before IOException.
Fix this by catching the more specific IOException first
In Java 7 on wards, you can catch multiple exceptions likery{
 //....
} catch (Exception ex){
 log.error ("Error:" + ex)
} catch (IOException ex){
 log.error ("Connectivity issue:" + ex); //never reached as Exception catch block catches everything
}
ry{
 //....
} catch (IOException ex){
 log.error ("Connectivity issue:" + ex);
} catch (Exception ex){
 log.error ("Error:" + ex)
}

---

## 🔹 Q3: Does Java’ s exception handling use any design pattern?

**Answer:**

Yes. Java’ s exception handling mechanism uses the “Chain of Responsibility” design pattern.
1. Sender of an exception will not know which object in the chain will serve its request.
2. Each processor in the chain may decide to serve the exception by catching and logging or
3. wrapping it with an application specific exception and then rethrowing it to the caller or
4. don’ t handle it and leave it to the caller

---

## 🔹 Q4: What are the different ways to look at the trace of your program execution?

**Answer:**

Java is a stack based language, and the program execution is pushed and popped out of a stack. When a method is entered into, it is pushed into a stack, and
when that method invokes many other methods, they are pushed into the stack in the order in which they are executed. As each method completes its execution,
they are popped out of the stack in the LIFO order. Say methodA( ) invoked methodB( ), and methodB( ) invoked methodC ( ), when execution of methodC( ) is
completed, it is popped out first, and then followed by methodB( ) and then methodA( ). When an exception is thrown at any point, a stack trace is printed for
you to be able to find where the issue is.
A Java developer can access a stack trace at any time. One way to do this is to call
You could get a stack trace of all the threads using the Java utilities such as jstack, JConsole or by sending a kill -quit signal (on a Posix operating system) or on
WIN32 platform to get a thread dump. The thread dumps are very useful in identifying concurrency issues like dead locks, contention issues, thread starvation,
etc.ry{
 //....
}
catch (ParseException | IOException exception ) {
 // handle I/O problems.
} catch (Exception ex) {
 //handle all other exceptions
}
Thread .currentThread ().getStackT race(); //handy for tracing

---

## 🔹 Q5: How would you go about analyzing stack traces correctly?

**Answer:**

1. One of the most important concepts of correctly understanding a stack trace is to recognize that it lists the execution path in reverse chronological order from
most recent operation to earliest operation. That is, it is LIFO.
2. The stack trace below is simple and it tells you that the root cause is a NullPointerException on ClassC line 16. So you look at the top most class.
3. The stack trace can get more complex with multiple “caused by” clauses, and in this case you usually look at the bottom most “caused by”. For example,
The root cause is the last “caused by”, which is a NullPointerException on ClassC line 16.Exception in thread "main" java.lang.NullPointerException
 at com.myapp .ClassC .methodC (ClassC .java:16)
 at com.myapp .ClassB .methodB (ClassB .java:25)
 at com.myapp .ClassA .main (ClassA .java:14)
Exception in thread "main" java.lang.IllegalStateException : ClassC has a null property
 at com.myapp .ClassC .methodC (ClassC .java:16)
 at com.myapp .ClassB .methodB (ClassB .java:25)
 at com.myapp .ClassA .main (ClassA .java:14)
Caused by: com.myapp .MyAppV alidationException
 at com.myapp .ClassB .methodB (ClassB .java:25)
 at com.myapp .ClassC .methodC (ClassC .java:16)
 ... 1 more
Caused by: java.lang.NullPointerException
 at com.myapp .ClassC .methodC (ClassC .java:16)
 ... 1 more

4. When you use plethora of third-party libraries like Spring, Hibernate, etc, your stack trace’ s “caused by” can really grow and you need to look at the bottom
most “caused by” that has the package relevant to you application like com.myapp.ClassC and skip library specific ones like or g.hibernate.exception.*.
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