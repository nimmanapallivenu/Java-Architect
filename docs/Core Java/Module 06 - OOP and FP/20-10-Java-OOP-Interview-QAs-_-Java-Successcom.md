# 20. 10 Java OOP Interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: Is Java a 100% Object Oriented (OO) language? if yes why? and if no, why not?](#q1)
- [Q2: How to create a well designed Java application?](#q2)
- [Q3: What do you achieve through good class and interface design with OOP?](#q3)
- [Q4: What are the 4 main concepts of OOP?](#q4)
- [Q5: What problem(s) does abstraction and encapsulation solve?](#q5)
- [Q6: What problem(s) does inheritance & composition solve?](#q6)
- [Q7: Can you explain the Java concepts like overloading, overriding, hiding & obscuri](#q7)
- [Q8: Given the following code snippets, can you explain the OOP concepts – abstractio](#q8)
- [Q9: What is the relationship among “ OOP “, “Design Principles “, and “ Design Patte](#q9)
- [Q10: What is the difference between OOP & Functional Programming (i.e. FP)?](#q10)

---

## 🔹 Q1: Is Java a 100% Object Oriented (OO) language? if yes why? and if no, why not?

**Answer:**

I would say Java is not 100% object oriented, but it embodies practical OO concepts. Strictly speaking not 100% object oriented due to:
1) its existence of 8 primitive variables like int, long, char , float, etc. These data types have been excused from being objects for simplicity and to improve
performance.
2) its existence of static methods and variables. Static methods are be invoked without instantiating an object.
3) not supporting multiple class inheritance to solve the diamond problem because different classes may have different variables with same name that may be
contradicted and can cause confusions and result in errors.
4) not supporting operator overloading except for string concatenation and addition operations.

---

## 🔹 Q2: How to create a well designed Java application?

**Answer:**

A software application is built by coupling various classes & interfaces , modules (i.e. made of classes & interfaces), and components (i.e. made up of
modules). W ithout coupling, you can’t build a software system. But, the software applications are always subject to changes and enhancements . So, you need to
build your applications such a way that they can not only adapt to growing requirements, but also are easy to maintain and understand . This is what OOP
achieves.
[Detailed example : How to create a well designed Java application? ]

---

## 🔹 Q3: What do you achieve through good class and interface design with OOP?

**Answer:**

OOP is a programming paradigm based on the concept of “objects”, which may contain data, in the form of fields aka attributes, and functionality , in the
form of methods. Objects couple with each other by invoking methods on each other . Classes & interfaces are the building blocks to create objects. If the classes
& interfaces are not structured properly , the resulting code becomes harder to maintain or extend due to increased complexity & coupling.
Each object has its own responsibility and couple or collaborate with the other objects to get the task done.
CircuService –> handleLion() –> LionHandler –> help() –> LionHelper
OrderApp –> placeOrder(…) –> OrderService –> saveOrder(…) –> OrderDao
OOP concepts, OO design principles, and OO design patterns help you create
1. Loosely coupled classes, objects, and components enable your application to easily grow and adapt to changes without being rigid or fragile.

2. Less complex and reusable code that increases maintainability , extendability and testability .
OOP Vs Design Principles Vs Design Patterns

---

## 🔹 Q4: What are the 4 main concepts of OOP?

**Answer:**

Encapsulation, polymorphism, and inheritance are the 3 main concepts or pillars of an object oriented programming. Abstraction is another important

concept that can be applied to both object oriented and non object oriented programming. [Remember: “ a pie ” abstraction, polymorphism, inheritance, and
encapsulation.]

---

## 🔹 Q5: What problem(s) does abstraction and encapsulation solve?

**Answer:**

Both abstraction and encapsulation solve same problem of complexity in different dimensions. Encapsulation exposes only the r equir ed details of an
object to the caller by forbidding access to certain members,
Bad: No encapsulation

Good: Encapsulated
Abstraction allows us to represent complex real world in simplest manner by
1) Hiding non-essential implementation details by letting you focus on what the object does instead of how it does it. For example, expose only a handful of
methods to be accessed via an “Interface” or “Abstract” class. When you drive a car , you know what a steering wheel does but you may not know the
underlying implementation details as to how it does.
2) providing a basis for your application to grow and change over a period of time with the help of generalization . For example, if you generalize out the
“make” and “model” of a vehicle as class attributes as opposed to as individual classes like “T oyota.java”, “T oyotaCamry .java”, “T oyotaCorolla.java”, etc, you
can easily incorporate new types of cars at runtime by creating a new car object with the relevant “make” and “model” as arguments as opposed to having to
declare a new set of classes.

Java Abstraction
A good OO design should hide non-essential implementation details through abstraction and information details through encapsulation.
Q. What is the difference between Abstraction & Generalization?
A. Abstraction and generalization are often used together . Abstracts are generalized through variables, parameterization, generics and polymorphism.
Generalization places the emphasis on the similarities among objects. It helps to manage complexity by collecting individuals into groups.
For example, say you want to capture employment type like part-time, full-time, casual, semi-casual, and so on, it is a bad practice to define them as classes as
shown below .
Bad non abstracted example:
package com.oo;
public class PartT imeEmployee extends Employee {

Why is it bad? You will end up creating a new class for each employee type. This will make your code more rigid and tightly coupled . The better approach is
to abstract out the employmentT ype as a class attribute. This way , instead of creating new classes for every employment type, you will be just creating new
objects at runtime with different employmentT ypes.
The above class is both properly abstracted and encapsulated.

---

## 🔹 Q6: What problem(s) does inheritance & composition solve?

**Answer:**

Reusability . How can logic be easily used in two places? In object-oriented language, there are four primary ways to accomplish this:}
package com.oo;
public class FullT imeEmployee extends Employee {
}
public class Employee {
enum EmploymentT ype {PART_TIME , FULL_TIME , CASUAL , SEMI_CASUAL }
private String name ;
private int age;
private BigDecimal salary ;
private EmploymentT ype employmentT ype;
 // ... getters and setters for external access

Copy and Paste – Bad as it is hard to maintain. Any changes original logic need to be applied across all the pasted locations.
Inheritance – Ok. T akes place at compile-time. So, can be fragile.
Composition – Good, and favored over inheritance. Happens at runtime.
Mixins – Good, but not supported in Java and can be abused and consequently increase complexity . The mixins are kind of composable abstract classes.
They are used in a multi-inheritance context to add services to a class. Java 8 has a naive emulation of mixins with virtual extension or default methods.
There is a post dedicated to why favor composition over inheritance? : Why favor composition over inheritance? Java OOP Interview Q&As

---

## 🔹 Q7: Can you explain the Java concepts like overloading, overriding, hiding & obscuring? Which one of these concepts lead to polymorphism?

**Answer:**

Overriding is the means by which you achieve polymorphism. Overloading, overriding, hiding & obscuring are related concepts explained in detail at Java
Polymorphism vs Overriding vs Overloading .

---

## 🔹 Q8: Given the following code snippets, can you explain the OOP concepts – abstraction, encapsulation, Inheritance, and polymorphism ?
You can elaborate with additional code examples using java.util.List methods.

**Answer:**

This is explained in detail at Explain abstraction, encapsulation, Inheritance, and polymorphism with the given Java code?

---

## 🔹 Q9: What is the relationship among “ OOP “, “Design Principles “, and “ Design Patterns ” ?

**Answer:**

All these 3 lead to “ best practices ” for building a quality application with Java classes & interfaces, which are the building blocks. You can’ t use these
building blocks the any way you like. There are overarching principles & tried and tested patterns as depicted above.
7 Design principles interview questions & answers for Java developers | Java design patterns interview questions 7 answers

---

## 🔹 Q10: What is the difference between OOP & Functional Programming (i.e. FP)?

**Answer:**

FP is another programming paradigm like OOP . In FP “Functions” are the first class citizens as “Objects” are in OOP . Both OOP & FP compliment each
other and you can use both in your next Java application if using Java 8. You like it or not, you will be using functional programming in Java, and interviewers
are going to quiz you on functional programming.
Top 6 tips to transforming your thinking from OOP to FP with examplesList<String > list = new ArrayList <>();
ist.add("Java" );
ist.add("JEE" );

Q. If you are mentoring a junior developer , what tips would you give him/her on OOP?
A. An open-ended question to judge your experience & know how .Top 5 OOPs tips for Java developers
Q. How would you go about writing loosely coupled Java application?
A.Top 6 tips to go about writing loosely coupled Java applications
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

