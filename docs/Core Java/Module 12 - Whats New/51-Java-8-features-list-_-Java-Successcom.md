# 51. Java 8 features list   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

#1 One needs to get used to the transformation from imperative programming to functional programming . You like it or not, you will be using functional
programming in Java 8, and interviewers are going to quiz you on functional programming. Fortunately , Java is not a fully functional programming language,
and hence one does not require the full leap to functional programming. Java 8 supports both imperative and functional programming approaches.
#2 Here are top 6 Java 8 features you can start using now by downloading Java 8.
1. Interface can have static and default methods.
2. Lambda expression for supporting closures.
3. Java 8 adopts Joda. 
4. Java 8 corrects its lack of good support for JavaScript with the engine Nashorn . 
5. Parallel processing piggy backing the Java 7 Fork/Join .
6. The java.util.concurrent.atomic package has been expanded to include four new classes — LongAccumulator , LongAdder , DoubleAccumulator and
DoubleAdder
#3 Writing functional programs with Java 8 – simple examples .
The @FunctionalInterface annotation ensures that you can only have a single abstract method (aka SAM ). You can have additional default and static method
implementations. The default methods provide default behaviors, and the static methods are used as helper methods.
java.utilCollection is an interface and java.util.Collections is utility class with helper methods
java.nio.file.Path is an interface and java.nio.file.Paths is a helper class.
#4 7 useful Java 8 miscellaneous additions worth knowing and talking about in the job interviews if asked.
1. String.join( ) method, which is an opposite of String.split(…). 
2. Comparator interfaces have a number of useful methods to sort objects with options to nest multiple fields with thenComparing(…) , and other handy
methods like nullsFirst( ) , nullsLast( ) , naturalOrder( ) , reverseOrder( ) , etc.
3. In Java 8, java.util.Optional class has been added to deal with optional object references.
4. In cases where you want to detect result overflow errors in int and long, the methods addExact , subtractExact , multiplyExact , and toIntExact in Java 8,
the java.lang.Math class throws an ArithmeticException when the results overflow . 
5. The ability to lazily read lines from a file with Files.lines(…) .
6. In Java 8, you can repeat the same annotation with the @Repeatable annotation. 
7. Java 8 introduces the “ Type Annotations “, which are annotations that can be placed anywhere you use a type.
#5 Different ways to sort a collection of objects in pre and post Java 8 .
1. Java library Comparator pre Java 8
2. Apache commons library BeanComparator pre Java 8
3. Google Gauva library functional programming style Pre Java 8

4. Java 8 functional programming approach. Concise and powerful.
#6 Java 8 functional programming is not a silver bullet . Where does it really shine? When will you use it?
1. Where the functional code has increased readability and maintainability
2. Where performance improvement is possible with Fork/Join parallel processing.
3. Where code becomes easier to refactor in the future
4. Where the code is easier to test and debug.
So, OO programming and functional programming can co-exist.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »



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

