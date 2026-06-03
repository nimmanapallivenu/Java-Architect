# 11. 12 Java classes and interfaces interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: Which class declaration is correct if A and B are classes and C and D are interf](#q1)
- [Q2: What happens when a parent and a child class has the same variable name?](#q2)
- [Q3: What happens when the parent and the child class has the same non-static method ](#q3)
- [Q4: What happens when the parent and the child class has the same static method with](#q4)
- [Q5: What is the difference between an abstract class and an interface and when shou](#q5)
- [Q7: When will you use an interface?](#q7)
- [Q8: What is a marker or a tag interface? Why there are some interfaces with no defin](#q8)
- [Q9: What is a functional interface?](#q9)
- [Q10: What does the @FunctionalInterface do, and how is it different from the @Overri](#q10)
- [Q10: Does Java 8 solve the “diamond problem” discussed earlier? If yes how?](#q10)

---

## 🔹 Q1: Which class declaration is correct if A and B are classes and C and D are interfaces?
a) class Z extends A implements C, D{}
b) class Z extends A,B implements D {}
c) class Z extends C implements A,B {}
d) class Z extends C,D implements B {}

**Answer:**

a). class Z extends A implements C, D{}
A class is a template. A class can extend only a single class (i.e. single inheritance. Java does not support multiple implementation inheritance), but can
implement multiple interfaces to achieve multiple interface inheritance. An interface can also extend more than one other interfaces.

---

## 🔹 Q2: What happens when a parent and a child class has the same variable name?

**Answer:**

When both a parent class and its subclass have a field with the same name, this technique is called variable shadowing or variable hiding . The variable
shadowing depends on the static type of the variable in which the object’ s reference is stored at compile time and NOT based on the dynamic type of the actual
object stored at runtime as demonstrated in polymorphism via method overriding.

---

## 🔹 Q3: What happens when the parent and the child class has the same non-static method with the same signature?

**Answer:**

Unlike variables, when a parent class and a child class each has a non-static method (aka an instance method) with the same signature, the method of the
child class overrides the method of the parent class. The method overriding depends on the dynamic type of the actual object being stored and NOT the static
type of the variable in which the object reference is stored. The dynamic type can only be evaluated at runtime. As you can see, the rules for variable shadowing
and method overriding are directly opposed. The method overriding enables polymorphic behavior .

---

## 🔹 Q4: What happens when the parent and the child class has the same static method with the same signature?

**Answer:**

The behavior of static methods will be similar to the variable shadowing or variable hiding , and not recommended. It will be invoking the static method
of the referencing static object type determined at compile time, and NOT the dynamic object type being stored at runtime.

---

## 🔹 Q5: What is the difference between an abstract class and an interface and when should you use them?

**Answer:**

In design, you want the base class to present only an interface for its derived classes. This means, you don’ t want anyone to actually instantiate an object ofnterface E extends C,D { //.... }

the base class. You only want to upcast to it (implicit upcasting , which gives you polymorphic behavior), so that its interface can be used. This is accomplished
by making that class abstract using the abstract keyword. If anyone tries to make an object of an abstract class, the compiler prevents it.
The interface keyword takes this concept of an abstract class a step further by preventing any method or function implementation at all. You can only declare a
method or function but not provide the implementation till Java 7. The class, which is implementing the interface, should provide the actual implementation.
The interface is a very useful and commonly used aspect in OO design, as it provides the separation of interface and implementation and enables you to
— Capture similarities among unrelated classes without artificially forcing a class relationship.
Declare methods that one or more classes are expected to implement.
— Reveal an object’ s programming interface without revealing its actual implementation.
— Model multiple interface inheritance in Java, which provides some of the benefits of full on multiple inheritances, a feature that some object-oriented
languages support that allow a class to have more than one super class.
Q6 When will you use an abstract class?
A6 In case where you want to use implementation inheritance then it is usually provided by an abstract base class. Abstract classes are excellent candidates
inside of application frameworks. Abstract classes let you define some default behavior and force subclasses to provide any specific behavior . Care should be
taken not to overuse implementation inheritance as discussed before. The template method design pattern is a good example to use an abstract class where the
abstract class provides a default implementation.

---

## 🔹 Q7: When will you use an interface?

**Answer:**

For polymorphic interface inheritance, where the client wants to only deal with a type and does not care about the actual implementation, then use
interfaces. If you need to change your design frequently , you should prefer using interface to abstract class. Coding to an interface reduces coupling. Another
justification for using interfaces is that they solve the ‘ diamond pr oblem ’ of traditional multiple inheritance as shown in the diagram. Java does not support
multiple inheritance. Java only supports multiple interface inheritance. Interface will solve all the ambiguities caused by this ‘diamond problem’. Java 8 has
introduced functional interfaces, and partially solves the diamond problem by allowing default and static method implementations in interfaces to inherit
multiple behaviors. Strategy design pattern lets you swap new algorithms and processes into your program without altering the objects that use them.

Diamond problem and multiple inheritance in Java

---

## 🔹 Q8: What is a marker or a tag interface? Why there are some interfaces with no defined methods (i.e. marker interfaces) in Java?

**Answer:**

The interfaces with no defined methods act like markers. They just tell the compiler that the objects of the classes implementing the interfaces with no
defined methods need to be treated differently . For example, java.io.Serializable, java.lang.Cloneable, java.util.EventListener , etc. Marker interfaces are also
known as “tag” interfaces since they tag all the derived classes into a category based on their purpose.
Now with the introduction of annotations in Java 5, the marker interfaces make less sense from a design standpoint. Everything that can be done with marker or
tag interfaces in earlier versions of Java can now be done with annotations at runtime using r eflection . One of the common problems with the marker or tag
interfaces like Serializable, Cloneable, etc is that when a class implements them, all of its subclasses inherit them as well whether you want them to or not. You
cannot force your subclasses to un-implement an interface. Annotations can have parameters of various kinds, and they’re much more flexible than the marker
interfaces. This makes tag or marker interfaces obsolete, except for situations in which empty or tag interfaces can be checked at compile-time using the type-
system in the compiler

---

## 🔹 Q9: What is a functional interface?

**Answer:**

Functional interfaces are introduced in Java 8 to allow default and static method implementations to enable functional programming (aka closures) with
lambda expressions.
@FunctionalInterface

---

## 🔹 Q10: What does the @FunctionalInterface do, and how is it different from the @Override annotation?

**Answer:**

The @Override annotation takes advantage of the compiler checking to make sure you actually are overriding a method when you think you are. This
way, if you make a common mistake of misspelling a method name or not correctly matching the parameters, you will be warned that you method does not
actually override as you think it does. Secondly , it makes your code easier to understand because it is more obvious when methods are overwritten.
The annotation @FunctionalInterface acts similarly to @Override by signaling to the compiler that the interface is intended to be a functional interface. The
compiler will then throw an error if the interface has multiple abstract methods.

---

## 🔹 Q10: Does Java 8 solve the “diamond problem” discussed earlier? If yes how?

**Answer:**

Partially yes.
We know that Java does not support multiple implementation inheritance to solve the diamond problem (till Java 8). Java did only support multiple interface
inheritance. That is, a class can implement multiple interfaces. By having default method implementations in interfaces, you can now have multiple behavioral
inheritance in Java 8. Partially solving the diamond problem.

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

