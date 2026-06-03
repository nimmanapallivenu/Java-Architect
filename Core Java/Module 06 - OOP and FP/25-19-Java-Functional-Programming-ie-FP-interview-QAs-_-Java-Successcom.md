# 25. 19 Java Functional Programming ie FP interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: Can you explain your understanding of Functional Programming (FP)?](#q1)
- [Q2: Can you list the differences between OOP and FP?](#q2)
- [Q3: What are the 6 characteristics of FP you need to be familiar with?](#q3)
- [Q4: What do you understand by the terms streams, lambdas, intermediate vs terminal o](#q4)
- [Q5: Why is it essential to understand lazy loading with regards to FP?](#q5)
- [Q6: What is a functional interface?](#q6)
- [Q7: What does the @FunctionalInterface do, and how does it differ from the @Overrid](#q7)
- [Q8: Can you have method implantations in Java interfaces?](#q8)
- [Q9: Does this mean that abstract classes are not required any more?](#q9)
- [Q10: Can a functional interface have any number of default and static methods?](#q10)
- [Q11: Why were default methods introduced in Java 8?](#q11)
- [Q12: What are the benefits of default and static methods?](#q12)
- [Q13: What happens to a class that implements two or more interfaces with the same def](#q13)
- [Q14: Can a default method override a method from java.lang.Object class?](#q14)
- [Q15: Can an interface static method override a method from java.lang.Object class?](#q15)
- [Q16: What is currying? Does Java 8 support currying?](#q16)
- [Q17: What do you understand by the term referential transparency with respect to func](#q17)
- [Q18: What is a pure function?](#q18)
- [Q19: What is a closure?](#q19)

---

## 🔹 Q1: Can you explain your understanding of Functional Programming (FP)?

**Answer:**

In a very simplistic approach FP means:
#1 Programming without assignments: For example, in imperative style programming like OOP , you can say “x = x + 5”, which is an assignment , but in
mathematical or functional programming you need to say f(x) -> x + 5. If, x were to be 2, it is mathematically incorrect to say “2 = 2 + 5”. In imperative style
you are assigning a new value of 7 to x by saying “x = x + 5”. So, imperative programming has state and you can assign a new state. FP treats computation as
the evaluation of mathematical functions and avoids storing state and mutating data.
#2 FP focuses on WHA T to be done, and NOT on HOW to be done: For example, in the ‘functional style loop’ you’ll focus only on the action of what to do on
each element and you don’ t have to concentrate on how to go through each element. The “forEach” function will take care of it.
Here is a simple example that shows imperative (e.g. OOP) and functional styles to loop through 1 to 10, square each number , and print squared numbers.
package com.mytutorial ;
mport java.util.stream .IntStream ;
public class WorkW ithNumbers {
 
 public static void main (String [] args) {
 WorkW ithNumbers wwn = new WorkW ithNumbers ();
 wwn .printSquaresUptoT enImperativeStyle ();
 System .out.println (); // newline
 wwn .printSquaresUptoT enFunctionalStyle ();
 }
 
 //Imperative style
 public void printSquaresUptoT enImperativeStyle () {
 for (int i = 1; i <= 10 ; i++) {
 System .out.print (i*i + " ");
 }

printSquar esUptoT enImperativeStyle
The variable “i” takes new value in each iteration. This is assignment as described in #1. The “for (int i = 1; i <= 10 ; i++)" defines "HOW to" go through each
element as discussed in #2.
printSquar esUptoT enFunctionalStyle
1) Consists of 3 functions: “rangeClosed”, “map”, and “forEach”. The mutable state is avoided, and the output of a function is exclusively dependent on the
values of its inputs. This means that if we call a function x amount of times with the same parameters we’ll get exactly the same result every time.
2) The “rangeClosed” function takes two arguments “startIclusive” and “endInclusive” to produce a range of integers (i.e 1 to 10).
3) The “map” function transforms a Collection, List, Set or Map. By using a “map” function you can apply a predefined function or a user defined function. You
can not only use a lambda expression (e.g. i -> i * i), but also you can use a method reference (e.g System.out::println).
4) The “forEach” function loops through each element and performs the “action” of printing each squared integers. Focuses on what to do as per #2. Does not
have to worry about “how to” iterate as “forEach” function will take care of the iteration logic.

---

## 🔹 Q2: Can you list the differences between OOP and FP?

**Answer:**

Top 6 tips to transforming your thinking from OOP to FP with examples

---

## 🔹 Q3: What are the 6 characteristics of FP you need to be familiar with?

**Answer:**

}
 
 //FP style
 public void printSquaresUptoT enFunctionalStyle () {
 IntStream .rangeClosed (1, 10) // range function takes 2 parameters "startIclusive" and "endInclusive"
 .map(i -> i * i) // map function to transform each element
 .forEach (i -> { // forEch function focuses only on the "action" of what to do on each element
 System .out.print (i + " ");
 });
 }

1) A focus on what is to be computed rather then how to compute it.
2) Function Closure Support
3) Higher -order functions
4) Use of recursion as a mechanism for flow control
5) Referential transparency
6) No side-ef fects
Each of the above is explained in detail with examples at Top 6 tips to transforming your thinking from OOP to FP with examples

---

## 🔹 Q4: What do you understand by the terms streams, lambdas, intermediate vs terminal ops, and lazy loading with regards to FP?

**Answer:**

A stream is an infinite sequence of consumable elements (i.e a data structure) for the consumption of an operation or iteration.
An Intermediate operation like “map” produces another stream, whereas a “T erminal” operation like “forEach” produces an object or an output that is not a
stream.
Intermediate operations are lazy operations , which will be executed only after a terminal operation is called. For example, when you call the intermediate
operation “.map(i -> i * i)” the lambda body isn’ t executed until after the terminal operation “.forEach(i -> { System.out.print(i + ” “);});” is called.
Learn more in detail with examples & diagrams: Java 8 Streams, lambdas, intermediate vs terminal ops, and lazy loading with simple examples

---

## 🔹 Q5: Why is it essential to understand lazy loading with regards to FP?

**Answer:**

This is essential to understand from the viewpoint of adding break points in your IDE for debugging purpose.

---

## 🔹 Q6: What is a functional interface?

**Answer:**

java.lang.Runnable, java.util.Comparator , and java.util.concurrent.Callable are Single Abstract Method interfaces (SAM Interfaces). For example,
java.lang.Runnable has a single abstract method
package java.lang;
public interface Runnable {
 public abstract void run();
}

and these methods were called by anonymous classes like
Functional interfaces introduced in Java 8 are recreation of SAM(Single Abstract Method) interface to allow functional programming with lambda expressions
by adding an annotation @FunctionalInterface which can be used for compiler level errors when the interface you have annotated is not a valid Functional
Interface.
Note: The method “sum” is an abstract method as the keyword “abstract” is optional in an interface.
Now , the following “ lambda expr ession ” is assignable to the functional interface type Summable. new Thread (new Runnable () {
 @Override
 public void run() {
 System .out.println ("Running in a new thread" );
 }
 }).start();
@FunctionalInterface 
public interface Summable { 
 public int sum(int input1 , int input2 ); //abstract method. keyword abstract is optional in an interface
} 

So, the lambda expr ession has two parts:
Part 1: The body of the expression denoted by “(a, b) -> a + b;”. This is equivalent to an anonymous method. aka no name method.
Part 2: The signatur e of the lambda expression via a functional interface. A functional interface is a single method interface. The functional interface takes two
input arguments of types integer and returns a result of type integer .
Outputs:
Now , the same example with Generics included.Summable sumT ype = (a, b) -> a + b;
public class LambdaAssignedT oFunctionalInterfaceT ype {
 public static void main (String [] args) {
 Summable sumT ype = (a, b) -> a + b;
 int result = sumT ype.sum(5, 6);
 System .out.println ("result=" + result );
 }
}
result =11

Where, T, and U are input arguments type and R is a result type.
A new package “ java.util.function ” is added with a number of functional interfaces to provide tar get types for lambda expressions and method references. E.g.
Function , Consumer , IntConsumer , Predicate , Supplier , ToIntFunction , LongFunction , etc to name a few .
This means, in most cases you don’ t have to define your own Functional Interface like “Summabale”. You can reuse the “java.util.function. BiFunction ”
functional interface that takes 2 input arguments and returns a result as shown below:@FunctionalInterface
public interface Summable <T, U, R> {
 public R sum(T input1 , U input2 ); // abstract method. keyword abstract
 // is optional in an interface
}
public class LambdaAssignedT oFunctionalInterfaceT ype {
 public static void main (String [] args) {
 Summable <Integer , Integer , Integer > sumT ype = (a, b) -> a + b;
 int result = sumT ype.sum(5, 6);
 System .out.println ("result=" + result );
 }
}
mport java.util.function .BiFunction ;

Outputs:
Refer to:
1) Java 8 using the Predicate functional interface .
2) Java 8 API examples using lambda expressions and functional interfaces

---

## 🔹 Q7: What does the @FunctionalInterface do, and how does it differ from the @Override annotation?

**Answer:**

The @Override annotation takes advantage of the compiler checking to make sure you actually are overriding a method when you think you are. This way ,
if you make a common mistake of misspelling a method name or not correctly matching the parameters, you will be warned that you method does not actually
override as you think it does. Secondly , it makes your code easier to understand because it is more obvious when methods are overwritten.
The annotation @FunctionalInterface acts similarly to @Override by signalling to the compiler that the interface is intended to be a functional interface. The
compiler will then throw a compile-time error
1) If the interface has no abstract methods. T ry removing the “sum” method shown above, you will get the following compile-time error: “Invalid
‘@FunctionalInterface’ annotation; Summable is not a functional interface”.
2) If the interface has multiple abstract methods. Following gives a compile-time error .public class LambdaAssignedT oFunctionalInterfaceT ype {
 public static void main (String [] args) {
 BiFunction <Integer , Integer , Integer > sumT ype = (a, b) -> a + b;
 int result = sumT ype.apply (5, 6);
 System .out.println ("result=" + result );
 }
result =11

The “@FunctionalInterface” annotation is an optional facility to avoid accidental addition of abstract methods in the functional interfaces. It is a good
practice to use this annotation where applicable.

---

## 🔹 Q8: Can you have method implantations in Java interfaces?

**Answer:**

Yes. Prior to Java 8, you could have only “method declarations” in the interfaces. One of the key design changes in Java 8 is that you can have default
methods and static methods in the interfaces. This means, from Java 8 onwards you can define behaviors (i.e. implementation) in interfaces.

---

## 🔹 Q9: Does this mean that abstract classes are not required any more?

**Answer:**

No. You can only define behaviors via default and static methods, but you can’ t maintain states by defining non-final variables. Absract classes are required
to define default behavior with state.

---

## 🔹 Q10: Can a functional interface have any number of default and static methods?

**Answer:**

Yes. only 1 abstract method (e.g sum), and any number of default and static methods as shown below .@FunctionalInterface
public interface Summable {
 public int sum(int input1 , int input2 );
 public int subtract (int input1 , int input2 ); 
}
@FunctionalInterface
public interface Summable {
 
 static final int var = 0;
 
 int sum(int input1 , int input2 );
 // static method

---

## 🔹 Q11: Why were default methods introduced in Java 8?

**Answer:**

In order to support functional programming, new intermediate and terminal operations like map, forEach, etc had to be added so that the Java collection
API can work with lambda expressions like “i -> i * i” and “i -> {System.out.print(i + ” “);}”. One way to enable this is by adding these new methods to
existing interfaces like java.util.Collection, java.util.List, etc and then providing the implementations where required. But this approach has a problem. Once the
JDK is published, it would not be possible to add new methods to those interfaces by extending those interfaces without breaking the existing implementation.
Hence this new concept of “default methods” was introduced to provide default implementation of the declared behavior .
An example where a collection if integers is converted to a stream and then printed
The stream() method returns a “ Stream” interface, which has all the methods that take functional interfaces like Supplier , BiConsumer , Function, etc as
arguments. static int subtract (int input1 , int input2 ) {
 return input1 - input2 ;
 }
 // default method
 default int multiply (int input1 , int input2 ) {
 return input1 * input2 ;
 }
 // default method
 default int divide (int input1 , int input2 ) {
 return input1 / input2 ;
 }
List<Integer > numbers = Arrays .asList (1,2,3,4,5);
numbers .stream ().forEach (i -> System .out.print (i + " "));

---

## 🔹 Q12: What are the benefits of default and static methods?

**Answer:**

1) Default methods enhance the Collection API to support lambda expressions.
2) Default methods will help you in extending existing interfaces without breaking the implementation classes as explained in “A1 1”.
3) Default methods eliminate the need for the base classes to provide default behaviors. The interfaces can provide default behaviors, and the implementing
classes can choose to override the default behaviors.
4) Default and Static methods can be added to the interface itself without requiring separate utility classes like java.util.Collections, java.util.Arrays, etc.
5) The interface static methods are handy for providing utility methods like null checks and collection sorting/shuf fling.
6) Unlike the default methods, the interface static methods prevent the implementing classes from accidentally overriding them.

---

## 🔹 Q13: What happens to a class that implements two or more interfaces with the same default method name and signature?

**Answer:**

You will get a compile-time error in the implementing class to define its own method with the same default method name and signature to resolve the
conflict.package java.util.stream ;
public interface Stream <T> extends BaseStream <T, Stream <T>> {
 
 <R> Stream <R> map(Function <? super T, ? extends R> mapper );
 void forEach (Consumer <? super T> action );
 <R> R collect (Supplier <R> supplier ,
 BiConsumer <R, ? super T> accumulator ,
 BiConsumer <R, R> combiner );
 //more abstract and static methods

---

## 🔹 Q14: Can a default method override a method from java.lang.Object class?

**Answer:**

No. It’s not possible because the “java.lang.Object” is the implicit base class for all Java classes. This means even if you have Object class methods
defined as default methods in interfaces, the Object class method will always be used. So, no point in allowing interfaces to override a method from
java.lang.Object class.

---

## 🔹 Q15: Can an interface static method override a method from java.lang.Object class?

**Answer:**

No. You will get a compile-time error: “This static method cannot hide the instance method from Object”. As mentioned earlier , the Object is implicitly
the base class for all the other Java objects and you can’ t have one class level static method and another instance level method with the same signature.

---

## 🔹 Q16: What is currying? Does Java 8 support currying?

**Answer:**

Currying (named after Haskell Curry) is the fact of evaluating function arguments one by one, producing a new function with one argument less on each
step. Java 8 still does not have first class functions, but currying is “practically” possible with verbose type signatures.
Learn more with examples: What is currying? Does Java 8 support currying?

---

## 🔹 Q17: What do you understand by the term referential transparency with respect to functional programming?

**Answer:**

In FP , a function will return the same result for invocations with the same parameters, and this is known as “referential transparency”. This allows the
result to be easily cached and returned any number of times to improve performance. This enables lazy evaluation that I mentioned earlier to defer the
computation of values until the point when they are needed.

---

## 🔹 Q18: What is a pure function?

**Answer:**

Functions with absolutely no side effects or functions that operate on immutable data are known as pure functions. This has several benefits. Firstly , since
the data can not be changed accidentally or on purpose, it can be freely shared improving memory requirements and enabling parallelism.

---

## 🔹 Q19: What is a closure?

**Answer:**

A closure is function that can be stored as a variable, and passed around to other functions with a special ability to access other variables local to the scope
it was created in. Since functions are treated like first class citizens, it’ s really useful to be able to pass them around together with their referencing environment
(i.e. a reference to each non-local variable of that function).
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

