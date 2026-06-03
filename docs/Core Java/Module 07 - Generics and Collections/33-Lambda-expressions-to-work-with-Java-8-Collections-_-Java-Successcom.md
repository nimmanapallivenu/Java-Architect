# 33. Lambda expressions to work with Java 8 Collections   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

A stream is an infinite sequence of consumable elements (i.e a data structure) for the consumption of an operation or iteration. Any Collection<T> can be
exposed as a stream. It looks complex, but once you get it, it is very simple. The operations you perform on a stream can either be
1. Intermediate operations like map, filter , sorted, limit, skip, concat, substream, distinct, peek, etc producing another java.util.stream.Stream<T> or a
2. Terminal operations like forEach, reduce, collect, sum, max, count, matchAny , findFirst, findAny , etc producing an object that is not a stream .
Streams: intermediate(blue) & terminal (red)
Basically , you are building a pipeline as in Unix. In Unix we “pipe” operations, and in Java 8, we stream them.
The stream( ) is a default method added to the Collection<T> interface in Java 8. The stream( ) returns a java.util.starem. Stream<T> interface with multiple
abstract methods like filter , map, sorted, collect, etc. The DelegatingStr eam<T> is the implementing class.
Intermediate operations are lazy operations , which will be executed only after a terminal operation was executed. So when you call .filter(i -> i % 3 == 0) the
lambda body isn’ t being executed at the moment. It will only be executed after a terminal operation was called ( collect , in the example shown below). This is
essential to understand from the viewpoint of adding break points in your IDE for debugging purpose.
Go through these examples to get a good handle on the stream concepts.
11 numbers 1 to 10 and an extra 6 are a) filtered first for multiples of 3 b) filtered for values less than 7 c) remove duplicates by adding to a Set<T> d) print the
result.s -l | grep “Dec” | Sort +4n | more

i -> i % 3 == 0 is a lambda expr ession used as a predicate to filter only multiples of 3. So,
Q. what is this “lambda expression”?
A. In OOP or imperative programming, x = x+ 5 makes sense, but in mathematics or functional pr ogramming , you can’ t say x = x + 5 because if x were to be
2, you can’ t say that 2 = 2 + 5. In functional programming you need to say f(x) -> x + 5.
Java 8 Stream
Example 1:
mport java.util.Arrays ;
mport java.util.List;
mport java.util.stream .Collectors ;

In the above example, filter and peek are intermediate operations that return a “Stream<T>” object. The “peek” is used for debugging . The “collect(…)” is a
terminal operation that returns a “Collection<T>” object, which extends “Iterable<T>”interface which has the “forEach(…)” method. Don’ t confuse this with
the “forEach()” method in the “java.util.stream.Stream<T>”.
Output:
Example 2:
Same as above, let’ s introduce another terminal operation sum() .public class Java8LambdaDebug {
 
 public static void main (String [] args) {
 List<Integer > list = Arrays .asList (1,2,3,4,5,6,7,8,9,10, 6);// 6 is repeated
 list.stream ()
 .filter (i -> i % 3 == 0) //multiples of 3
 .peek (i -> System .out.println ("Debug pt1: " + i))
 .filter (i -> i < 7)
 .peek (i -> System .out.println ("Debug pt2: " + i))
 .collect (Collectors .toSet ()) // remove duplicates i.e 6
 .forEach (i -> System .out.println ("result: " + i));
 }
Debug pt1: 3
Debug pt2: 3
Debug pt1: 6
Debug pt2: 6
Debug pt1: 9
Debug pt1: 6
Debug pt2: 6
esult : 3
esult : 6

In the above example, filter , peek, and mapT oInt are intermediate operations that return a “Stream” object. “sum” is terminal operation that returns a result.
Output:mport java.util.Arrays ;
mport java.util.List;
public class Java8LambdaDebug {
 
 public static void main (String [] args) {
 List<Integer > list = Arrays .asList (1,2,3,4,5,6,7,8,9,10, 6);//6 is repeated
 
 final int sum = list.stream ()
 .filter (i -> i % 3 == 0) //multiples of 3
 .peek (i -> System .out.println ("Debug pt1: " + i))
 .filter (i -> i < 7)
 .peek (i -> System .out.println ("Debug pt2: " + i))
 .mapT oInt(Integer ::intValue)
 .sum(); //duplicate 6 is included 3+6+6=15
 
 System .out.println ("sum=" + sum);
 }
Debug pt1: 3
Debug pt2: 3
Debug pt1: 6
Debug pt2: 6
Debug pt1: 9
Debug pt1: 6
Debug pt2: 6

Example 3:
Let’s mix “intermediate” and “terminal” operations up.
In the above example, filter(..), peek(..), stream(..), and mapT oInt(..) are intermediate operations that return a “Stream<T>” object. “collect(…)” and “sum()” are
terminal operations. Since, “collect” returns a “Collection&lt’;T>” terminal object after removing the duplicate value of 6 with the help of toSet() , we need to
call the stream() again to get the “Stream<T>” object back. Finally , “sum()” is a terminal operation.sum=15
mport java.util.Arrays ;
mport java.util.List;
mport java.util.stream .Collectors ;
public class Java8LambdaDebug {
 
 public static void main (String [] args) {
 List<Integer > list = Arrays .asList (1,2,3,4,5,6,7,8,9,10, 6);//6 is repeated
 
 final int sum = list.stream ()
 .filter (i -> i % 3 == 0) //multiples of 3
 .peek (i -> System .out.println ("Debug pt1: " + i))
 .filter (i -> i < 7)
 .peek (i -> System .out.println ("Debug pt2: " + i))
 .collect (Collectors .toSet ()) //remove duplicate i.e. 6
 .stream ()
 .mapT oInt(Integer ::intValue)
 .sum(); //duplicate is removed 3+6=12
 
 System .out.println ("sum=" + sum); 
 }

Output:
So, if still having trouble grasping this, have a look at the Java 8 API docos for Interfaces Stream<T>, Iterable<T> and Interface Collection<E>. Pay attention
to default methods and return objects.
So, now with a little bit of help from the Java 8 API docs, you can perform different combination of operations on a collection of data. You can also debug by
placing break points in your IDE like eclipse by keeping in mind that intermediate ops are lazily evaluated after a terminal operation. The peek() intermediate
operation is very handy for debugging as well.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »Debug pt1: 3
Debug pt2: 3
Debug pt1: 6
Debug pt2: 6
Debug pt1: 9
Debug pt1: 6
Debug pt2: 6
sum=9



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

