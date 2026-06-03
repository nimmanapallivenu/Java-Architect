# 22. 2 Polymorphism OOP Interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q16: Can you describe “ method overloading ” versus “ method overriding ”? Does it ha](#q16)
- [Q17: What is the difference among Overriding, Hiding, and Overloading in Java? How d](#q17)

---

## 🔹 Q16: Can you describe “ method overloading ” versus “ method overriding ”? Does it happen at compile time or runtime?

**Answer:**

Method overloading : Overloading deals with multiple methods in the same class with the same name but different method signatures. Both the below
methods have the same method names but different method signatures, which mean the methods are overloaded.
This happens at compile-time. This is also called compile-time polymorphism because the compiler must decide how to select which method to run based on
the data types of the arguments. If the compiler were to compile the statement:
it could see that the argument was a string literal, and generate byte code that called method #1.
Overloading lets you define the same operation in different ways for different data within the same class.
Method overriding : Overriding deals with two methods, one in the parent class and the other one in the child class and has the same name and signatures. Both
the below methods have the same method names and the signatures but the method in the subclass “B” overrides the method in the superclass “A”.public class { 
 public static void evaluate (String param1 ); //method #1 
 public static void evaluate (int param1 ); //method #2 
} 
evaluate (“My Test Argument passed to param1 ”); 

This happens at runtime. This is also called runtime polymorphism because the compiler does not and cannot know which method to call. Instead, the JVM
must make the determination while the code is running.
The method compute(..) in subclass “B” overrides the method compute(..) in super class “A”. If the compiler has to compile the following method,public class A { 
 public int compute (int input ) { 
 return 3 * input ;//method #3 
 } 
} 
public class B extends A { 
 @Override 
 public int compute (int input ) { 
 return 4 * input ; //method #4 
 } 
} 
public int evaluate (A reference , int arg2) { 
 int result = reference .compute (arg2); 
} 

The compiler would not know whether the input argument ‘reference’ is of type “A” or type “B”. This must be determined during runtime whether to call
method #3 or method #4 depending on what type of object (i.e. instance of Class A or instance of Class B) is assigned to input variable “reference”.
Overriding lets you define the same operation in different ways for different object types . This is known as polymorphism. Which method is invoked depends
on the type of object stored “A” or “B”, and NOT on the reference type which is “A” for both.

---

## 🔹 Q17: What is the difference among Overriding, Hiding, and Overloading in Java? How does overriding give polymorphism?

**Answer:**

Overriding , Hiding , Overloading and Obscuring are important core Java concepts and you will be quizzed on job interviews or written tests.
Q. What is overriding?
An instance method overrides all accessible instance methods with the same signature in super classes. If overriding were not possible, you can’ t have the OO
concept known as polymorphism. Let’ s explain this with a simple example.
Q. Can you tell what will be the output of the following code snippet?A obj1 = new B( ); 
A obj2 = new A( ); 
evaluate (obj1); // method #4 is invoked as stored object is of type B 
evaluate (obj2); // method #3 is invoked as stored object is of type A 
public class Base { 
public void f() { 
System .out.println ("Base" ); 
} 
} 

Test polymorphism :public class DerivedOne extends Base { 
@Override //ensures signature is the same at compile-time 
public void f() { 
System .out.println ("Derived 1" ); // overrides Base.f() 
} 
} 
public class DerivedT wo extends Base { 
@Override //ensures signature is the same at compile-time 
public void f() { 
System .out.println ("Derived 2" ); //overrides Base.f() 
} 
} 
public class PolymorphismT est { 
public static void main (String [] args) { 

A. The output will be
Derived 1
Derived 2
Base
What is happening here?
1. Method overriding (i.e. polymorphic behavior) is occurring at run time to determine stored object type.
2. Even though the variable “b” is of type Base, which f( ) method gets called depends on what type of object is stored in that variable.
//1 the stored object is of type DerivedOne, hence prints “Derived 1”.
//2 the stored object is of type DerivedT wo, hence prints “Derived 2”.
//3 the stored object is of type Base, hence prints “Base”
This is polymorphism. Poly in greek means many and Morph means change. So polymorphism is the ability (in programming) to present the same interface for
differing underlying forms (data types).
Q. What is hiding or shadowing?
The polymorphism and overriding are applicable only to non-static methods (i.e. instance methods). If you try to override a static method, it is known as hiding.
For example,Base b = new DerivedOne (); 
b.f();//1 
 
b = new DerivedT wo(); 
b.f();//2 
 
b = new Base (); 
b.f();//3 

Test hiding :public class Base { 
nt a = 5; 
public static void f() { 
System .out.println ("Base" ); 

public class DerivedOne extends Base { 
nt a = 7; //hides new Base().a 
public static void f() { 
System .out.println ("Derived 1" ); // hides Base.f() 

public class PolymorphismT est { 
public static void main (String [] args) { 
 
Base b = new DerivedOne (); 
b.f(); // prints "Base" 
b = new Base (); 

Why prints “Base” in both cases? Because, it only looks at what type the variable “b” is? in both cases of type Base. It does not care what type of object is
stored. Same behavior for variables, and this is known as “Shadowing”. So, the overriding static methods and instance variables are “ shadowed “, whereas the
overriding instance methods gives polymorphic behavior .
prints 5. So, hiding and shadowing are Bad Practices, and should be avoided.
Q. What is method overloading? Unlike, overriding and hiding that happen when you have an inheritance hierarchy like Parent, Child classes, Overloading
happens within the same class.
Methods in a class overload one another if they have the same name and different signatures. Unlike overriding, overloading happens at compile time. It knows
at compile time based on the method arguments you pass.b.f(); //prints "Base" 

public class PolymorphismT est { 
public static void main (String [] args) { 
 
Base b = new DerivedOne (); 
System .out.println (b.a); //prints 5 
 
b = new DerivedT wo(); 
System .out.println (b.a); //prints 5 

Finally ,
Q. What is obscuring? A variable obscures a type with the same name if both are in scope. Say , your instance and local variables have the same name.
Test obscuring:public class Base { 
public void f() { 
System .out.println ("Base" ); 
 
public void f(String a) { 
System .out.println ("Base" ); //overloaded 
 
public void f(int b) { 
System .out.println ("Base" ); // overloaded 

public class Base { 
nt a = 5; 
public void f() { 
int a = 7; //obscuring instance variable a 
System .out.println (a); 

So, overriding and overloading are good, but avoid method hiding (i.e. having same name static methods in parent/child classes), variable shadowing (i.e.
having similar variable names in parent/child classes), and variable obscuring (i.e. having same variable name for instance, static, and local variables).
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »public class PolymorphismT est { 
public static void main (String [] args) { 
 
Base b = new DerivedOne (); 
b.f(); //prints 7 -- local varible obscures instance variable

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

