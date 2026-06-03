# 13. 3 Abstract classes Vs interfaces interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

Q1. When to use an abstract class over an interface?
Q2. What is a diamond problem?
Q3. Does Java support multiple inheritance?
In design, you want the base class to present only an interface (or a contract) for its derived classes. This means, you don’ t want anyone to actually instantiate an
object of the base class. You only want to up cast to it (implicit up casting, which gives you polymorphic behavior), so that its interface can be used. This is
accomplished by making that class abstract using the abstract keyword. If anyone tries to make an object of an abstract class, the compiler prevents it with a
compile-time error .
Q. What is the purpose of an abstract class? 
A. The purpose of abstract classes is to function as base classes which can be extended by sub classes to create a full implementation. The base class is used for
code reuse and gives polymorphism by up casting to it.
Q. Should it have at least one abstract member?
A. It is subjective, but it is preferred. If your intention is to prevent a class from being instantiated, then the best way to handle this is with a private or protected
constructor , and not by marking it abstract.
Q.Can you give an example of an abstract class where it is used prevalently? 
A. Template Method design pattern is a good example of using an abstract class and this pattern is used very prevalently in application frameworks. The
Template Method design pattern is about providing partial implementations in the abstract base classes, and the subclasses can complete when extending the
Template Method base class(es). Here is an example
/cannot be instantiated
public abstract class BaseT emplate {
 
public void process () {
 fillHead ();
 //some default logic
 fillBody ();
 //some default logic
 fillFooter ();
}
//to be overridden by sub class

public abstract void fillBody ();
//template method. Sub classes can override or reuse this default implementation
public void fillHead () {
 System .out.println ("Simple header" );
}
 //template method. Sub classes can override or reuse this default implementation
 public void fillFooter () {
 System .out.println ("Simple footer" );
 }
 //more template methods can be defined here
public class InvoiceLetterProcessor extends BaseT emplate {
@Override
public void fillBody () {
System .out.println ("Invoice body" );
/ template method
public void fillHead () {
System .out.println ("Invoice header" );
public class InvoiceT estMain {

Another design pattern that makes use of abstract classes is the composite design pattern . A node or a component is the parent or base class and
derivatives can either be leaves (singular), or collections of other nodes, which in turn can contain leaves or collection-nodes. When an operation is performed
on the parent, that operation is recursively passed down the hierarchy . An interface can be used instead of an abstract class, but an abstract class can provide
some default behavior for the add() , remove() and getChild() methods.
Composite design pattern public static void main (String [] args) {
 //subclass is up cast to base class -- polymorphism
 BaseT emplate template = new InvoiceLetterProcessor ();
 template .process ();
 }

Java 8: If you are using Java 8 or later versions of Java, you can have default methods and static helper methods in Java 8 interface definition.
Q. What is the purpose of an interface ?
A. The interface keyword takes this concept of an abstract class a step further where till Java 7, you can’ t have implementations in an interface, but from Java 8
onwards, the concept of functional interfaces was introduced where you can implement default methods and static helper methods .
Q. What are the differences between abstract classes and interfaces?
A.
Before Java 8 :
Abstract class Interface
Can maintain state in instance and static variables. No state.
Have executable methods and abstract methods. Have no implementation code. All methods are abstract.
Can only extend one abstract class. A class can implement any number of interfaces.
Java 8 onwards :
Abstract class Interface
Can maintain state in instance and static variables. No state.
Have executable methods and abstract methods. Have executable default methods and static helper methods.
Can only subclass one abstract class. A class can implement any number of interfaces.public ClassB extends ClassA {} public ClassB implements InterfaceA , InterfaceB {}

So, from Java 8 onwards, the key difference is only one class can extend an abstract class, but a class can implement more than one interfaces.
Q. What is the diamond problem?
A. The diamond problem is that in the multiple-inheritance diagram on the left hand side below where CircleOnSquar e inherits state and behavior from both
Circle and Squar e. So, when we instantiate an object of class CircleOnSquar e, any calls to method definitions in class Shape will be ambiguous – because it’ s
not sure whether to call the version of the method derived from class Circle or class Squar e.
Multiple inheritance and diamond issue
Java does not have multiple inheritance , as you can only extend one class. This means that Java is not at risk of suf fering the consequences of the diamond
problem. But, Java does support multiple interface inheritance as shown above on the right hand side of the diagram as a Java class can implement
multiple interfaces. Hang on!!!!! From Java 8 onwards, you can have functional interfaces where you can implement default and static methods. This means,
Java supports multiple behavior inheritance , but not full multiple inheritance as state cannot be inherited because you can’ t define instance variables in an
interface.
Before Java 8 interface example :

After Java 8 interface example with default and static helper methods :
Now , if we have a class Calculator , which implements two functional interfaces Operation and Sum, and both has a default method add(Integer o) . How does itpublic interface Summable { 
abstract int sum(int input1 , int input2 ); 
}
@FunctionalInterface 
public interface Operation <Integer > { 
 Integer operate (Integer operand ); 
 default Operation <Integer > add(Integer o){ 
 return (o1) -> operate (o1) + o; 
 } 
 default Operation <Integer > multiply (Integer o){ 
 return (o1) -> operate (o1) * o; 
 } 
 //ads 5 to a given number 
 static Integer plus5 (Integer input ) { 
 return input + 5 ; 
} 

know which default method to use? The one from Operation or the one from Sum. The Calculator class must solve the multiple behavior inheritance
ambiguity by throwing a compile-time error
java: class Impl inherits unrelated defaults for add(Integer o) from types Operation and Sum.
In order to fix this class, the Calculator class needs to implement the add(Integer o) method to resolve the ambiguity .
Q. When to use an abstract class over an interface?
A.
Abstract classes
With the advent of Default Methods in Java 8, it seems that Interfaces and abstract Classes are same as you can implement behavior in both. However , they are
still different concepts as
An Abstract Class can define constructor (s).
Abstract classes are more structur ed and can have a state associated with them . While in contrast, default method can be implemented only in the
terms of invoking other Interface methods, with no reference to a particular implementation’ s state.
Hence, both are used for different purposes and choosing between two really depends on the scenario. Abstract methods are good for implementing template
method and composite design patterns with state and behavior .
Interfaces
If you need to change your design frequently , you should prefer using interfaces to abstract classes. Coding to an interface reduces coupling and interface
inheritance can achieve code reuse with the help of object composition. For example: The Spring frameworks’ dependency injection promotes code to an
interface principle. Another justification for using interfaces is that they solve the ‘diamond problem’ of traditional multiple inheritance. Java does not support
multiple inheritance, but supports multiple behavior inheritance.
Strategy design pattern lets you swap new algorithms and processes into your program without altering the objects that use them by making use of
interfaces.
Q. What are the different ways to get code r euse?
A. There are 3 approaches
1. Implementation inheritance with abstract classes.
2. Composition .
3. Delegation to a helper class.

Implementation inheritance gives you polymorphism in addition to code reuse. You can up cast your child class to your abstract base class. If you are using
composition for code reuse, then you need to use behavior inheritance with interfaces to get polymorphism ( i.e. code to interface ).
The GoF design patterns favor composition for code r euse with polymorphism with interfaces over implementation inheritance with abstract classes for
code r euse and poylmorphism .
Why?
1. Code reuse via composition happens at run time whereas code reuse via inheritance happens at compile time . Hence, composition is more flexible and less
fragile.
2. It is easy to misuse inheritance when there is really no “is a” relationship, hence breaking the Lithkov’ s Substitution Principle ( LSP). For example, a square is
not a rectangle. Overuse of implementation inheritance (uses the “extends” key word) can break all the sub classes, if the super class is modified. Do not use
inheritance just to get polymorphism. If there is no ‘is a’ relationship and all you want is polymorphism then use interface inheritance with composition, which
gives you code reuse.
3. Composition of fers better testability than Inheritance. Composition is easier to test because inheritance tends to create very coupled classes that are more
fragile (i.e. fragile parent class) and harder to test in isolation. The IoC containers like Spring, make testing even easier through injecting the composed objects
via constructor or setter injection.
Why favor composition over inheritance? detailed discussion.
Q. What is the difference between the default methods introduced in Java 8 and regular methods?
A. In summary , default methods are like regular methods, but
the default methods come with the default modifier .
the default methods can only access its arguments as Interfaces do not have any state.
Regular methods in Classes can use and modify method arguments as well as the variables (i.e state) of their Class .
The default (aka defender) methods allow you to add new methods to interfaces without breaking the existing implementations of your interfaces. This provides
an added flexibility by allowing interfaces to provide default implementations in situations where concrete classes fail to provide implementations for a
methods.
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

