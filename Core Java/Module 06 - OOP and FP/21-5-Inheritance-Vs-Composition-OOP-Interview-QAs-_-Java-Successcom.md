# 21. 5 Inheritance Vs Composition OOP Interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q11: How do you express an ‘is a’ relationship and a ‘has a’ relationship or explain ](#q11)
- [Q12: Which one to favor , composition or inheritance?](#q12)
- [Q13: Can you give an example of the Java API that favors composition?](#q13)
- [Q14: Can you a give an example where GoF design patterns use inheritance?](#q14)
- [Q15: What questions do you ask yourself to choose composition (i.e. has-a relationshi](#q15)

---

## 🔹 Q11: How do you express an ‘is a’ relationship and a ‘has a’ relationship or explain inheritance and composition?

**Answer:**

The ‘ is a’ relationship is expressed with inheritance and ‘ has a ’ relationship is expressed with composition. Both inheritance and composition allow
you to place sub-objects inside your new class. T wo of the main techniques for code reuse are class inheritance and object composition.
inheritance vs composition
Inheritance is uni-directional. For example House is a type of Building . But Building is not a House . Inheritance uses extends key word.
Composition : is used when a House has a Bathr oom. It is incorrect to say House is a Bathr oom. Composition simply means using instance variables that refer to
other objects. The class House will have an instance variable, which refers to a Bathr oom object.

---

## 🔹 Q12: Which one to favor , composition or inheritance?

**Answer:**

The guide is that inheritance should be only used when subclass ‘is a’ super class. Don’ t use inheritance just to get code reuse. If there is no ‘ is a’
relationship then use composition for code reuse.
Reason #1 : Overuse of implementation inheritance (uses the “extends” key word) can break all the subclasses, if the super class is modified. Do not use
inheritance just to get polymorphism. If there is no ‘is a’ relationship and all you want is polymorphism then use interface inheritance with composition ,
which gives you code reuse. Interface inheritance is accomplished by implementing interfaces.

Reason #2 : Composition is more flexible as it is easily achieved at runtime while inheritance provides its features at compile time. Don’ t confuse inheritance
with polymorphism. Polymorphism happens at runtime as it states that Java chooses which overridden method to run only at runtime.
Reason #3 : Composition of fers better testability than Inheritance. Composition is easier to test because inheritance tends to create very coupled classes that are
more fragile (i.e. fragile parent class) and harder to test in isolation. The IoC containers like Spring, make testing even easier through injecting the composed
objects via constructor or setter injection.

---

## 🔹 Q13: Can you give an example of the Java API that favors composition?

**Answer:**

The Java IO classes that use composition to construct different combinations of I/O outcomes like reading from a file or System.in, buf fering the streams,
tracking the line numbers, piping the streams for ef ficiency , etc using the decorator design pattern at run time.
The GoF design patterns like strategy , decorator , and proxy favor composition for code reuse over inheritance. Interesting read ti further your knowledge in
OOP & design patterns: Why do Proxy , Decorator , Adapter , Bridge, and Facade design patterns look very similar? What are the differences?

---

## 🔹 Q14: Can you a give an example where GoF design patterns use inheritance?

**Answer:**

A typical example of using inheritance for code reuse is in frameworks where the template method design pattern is used.
Template Method design pattern is a good example of using an abstract class and this pattern is used very prevalently in application frameworks.
1. Java HTTP Servlet’ s doGet and doPost methods.
2. Message Driven EJB’ s and Spring message listener ’s onMessage(….) method.
3. Spring framework’ s JdbcT emplate, JmsT emplate, etc.
4. All non-abstract methods of java.io.InputStr eam, java.io.OutputStr eam, java.util.AbstractList , java.util.AbstractMap , java.io.Reader , etc
The T emplate Method design pattern is about providing partial implementations in the abstract base classes, and the subclasses can complete when extending
the T emplate Method base class(es). Here is an example/construct a reader 
StringReader sr = new StringReader (“Some Text....”); 
/decorate the reader for performance 
BufferedReader br = new BufferedReader (sr); 
/decorate again to obtain line numbers 
LineNumberReader lnr = new LineNumberReader (br); 

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
//template method
public void fillHead () {
 System .out.println ("Simple header" );
}
 //template method
 public void fillFooter () {
 System .out.println ("Simple footer" );
 }
 //more template methods can be defined here
public class InvoiceLetterProcessor extends BaseT emplate {

Another common pattern that would use inheritance is the Composite design pattern .
A node or a component is the parent or base class and derivatives can either be leaves (singular), or collections of other nodes, which in turn can contain leaves
or collection-nodes. When an operation is performed on the parent, that operation is recursively passed down the hierarchy . An interface can be used instead of
an abstract class, but an abstract class can provide some default behavior for the add(), remove() and getChild() methods.@Override
public void fillBody () {
System .out.println ("Invoice body" );
/ template method
public void fillHead () {
System .out.println ("Invoice header" );
public class InvoiceT estMain {
 public static void main (String [] args) {
 //subclass is up cast to base class -- polymorphism
 BaseT emplate template = new InvoiceLetterProcessor ();
 template .process ();
 }

---

## 🔹 Q15: What questions do you ask yourself to choose composition (i.e. has-a relationship) for code reuse over implementation inheritance (i.e. is-a relationship)?

**Answer:**

Do my subclasses only change the implementation and not the meaning or internal intent of the base class? Is every object of type House really “is-an”
object of type Building ? Have I checked this for “Liskov Substitution Principle”
According to Liskov substitution principle (LSP) , a Square is not a Rectangle provided they are mutable. Mathematically a square is a rectangle, but
behaviorally a rectangle needs to have both length and width, whereas a square only needs a width.
Another typical example would be an Account class having a method called calculateInter est(..). You can derive two subclasses named SavingsAccount and
ChequeAccount that reuse the super class method. But you cannot have another class called a MortgageAccount to subclass the above Account class. This will
break the Liskov substitution principle because the intent is different. The savings and cheque accounts calculate the interest due to the customer , but the
mortgage or home loan accounts calculate the interest due to the bank.
Violation of LSP results in all kinds of mess like failing unit tests, unexpected or strange behavior , and violation of open closed principle (OCP) as you end
up having if-else or switch statements to resolve the correct subclass. For example,
f(shape instanceof Square ){ 
 //.... 

If you cannot truthfully answer yes to the above questions, then favor using “has-a” relationship (i.e. composition). Don’ t use “is-a” relationship for just
convenience. If you try to force an “is-a” relationship, your code may become inflexible, post-conditions and invariants may become weaker or violated, your
code may behave unexpectedly , and the API may become very confusing. LSP is the reason it is hard to create deep class hierarchies.
Learn more about SOLID OOP design principles .
Always ask yourself, can this be modeled with a “ has-a ” relationship to make it more flexible?
For example, If you want to model a circus dog, will it be better to model it with “is a” relationship as in a Cir cusDog “is a” Dog or model it as a role that a dog
plays? If you implement it with implementation inheritance, you will end up with sub classes like Cir cusDog , DomesticDog , GuideDog , SnifferDog , and
StrayDog . In future, if the dogs are differentiated by locality like local, national, international, etc, you may have another level of hierarchy like
LocalCir cusDog , NationalCicusDog , InternationalCir cusDog , etc extending the class Cir cusDog . So you may end up having 1 animal x 1 dog x 5 roles x 3
localities = 15 dog related classes. If you were to have similar differentiation for cats, you will end up having similar cat hierarchy like W ildCat , DomesticCat ,
LocalW ildCat , NationalW ildCat , etc. This will make your classes str ongly coupled.} 
else if (shape instanceof Rectangle ){ 
 //... 
} 

Explosion of classes due to inheritance
If you implement it with interface inheritance, and composition for code reuse , you can think of circus dog as a role that a dog plays. These roles provide an
abstraction to be used with any other animals like cat, horse, donkey , etc, and not just dogs. The role becomes a “has a” relationship. There will be an attribute
of interface type Role defined in the Dog class as a composition that can take on different subtypes (using interface inheritance) such as Cir cusRole ,

DomesticRole , GuideRole , SnifferRole , and StrayRole at runtime . The locality can also be modeled similar to the role as a composition. This will enable
different combinations of roles and localities to be constructed at runtime with 1 dog + 5 roles + 3 localities = 9 classes and 3 interfaces (i.e. Animal , Role and
Locality ). As the number of roles, localities, and types of animals increases, the gap widens between the two approaches. You will get a better abstraction with
looser coupling with this approach as composition is dynamic and takes place at run time compar ed to implementation inheritance, which is static .

Composition to the rescue

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

