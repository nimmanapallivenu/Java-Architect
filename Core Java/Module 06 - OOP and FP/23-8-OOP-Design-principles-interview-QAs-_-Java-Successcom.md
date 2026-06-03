# 23. 8 OOP Design principles interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q25: What are the SOLID design principles?](#q25)
- [Q26: Can you explain why design principles are only a guide?](#q26)
- [Q27: Is there anything wrong with the following class design? If yes, can the design ](#q27)
- [Q28: Is DIP related to Dependency Injection (DI)?](#q28)
- [Q29: Is there anything wrong with the following class design?
mport java.sql.Connecti](#q29)
- [Q30: What is your understanding of some of the following most talked terms like Depen](#q30)
- [Q31: Can you list some of the key principles to remember while designing your classes](#q31)
- [Q32: What is design by contract?](#q32)

---

## 🔹 Q25: What are the SOLID design principles?

**Answer:**

SOLID is an abbreviation for 5 design principles.
SOLID design principles
SRP (Single Responsibility Principle)
If you have a class with calculation logic, validation logic, data access logic and display logic all mixed up then it violates the SRP principle. This makes it
difficult to change one part without breaking others. Mixing responsibilities also makes the class harder to understand, harder to test, and increases the risk of

duplicating logic.
OCP (Open Closed Principle)
means you extend the behavior of a class without modifying the class but by adding new classes. A typical sign of violating the OCP is having large if/else or
switch statements.
LSP (Liskov Substitution Principle)
Mathematically , a square is a rectangle. But in OOD, but when modelled in code, it violates the LSP . A square has only setW idth(double width) method, whereas
a rectangle has both setW idth(double width) and setLength(double length) methods. What should setW idth(double width) do when called on a Square? Should it
set the height as well? What if you have a reference to it via its parent class, Rectangle?
The best way to reduce LSP violations is to keep very aware of the LSP whenever using inheritance , and consider avoiding the problem by favoring
composition (i.e has-a) where appropriate over inheritance (i.e. is-a).
ISP (Interface Segregation Principle)
simply means keep your interfaces small and cohesive. If you have a fat interface with lots of abstract methods, then you are imposing a huge implementation
burden on any class that wants to adhere to that contract. W orse still is that there is a tendency for class to only provide valid implementations for a subset of a
fat interface, which defeats the benefits of having an interface. What is the point in having a bulky contract that implementers don’ t adhere to?
DIP (Dependency Inversion Principle)
The DIP says that if a class has dependencies on other classes, it should rely on the dependencies’ interfaces rather than their concrete types. Code to interface,
not implementation.

---

## 🔹 Q26: Can you explain why design principles are only a guide?

**Answer:**

It is bad to over design. Design is all about trade of fs. You will find some principles contradict with each other . For example, OCP requires interface
inheritance, but LSP discourages inheritance and favors composition. Over use of SRP can lead to too many fine grained classes. So, design principles help you
have a balance by asking the right questions. Do you really need a huge interface explosion due while adhering to the OCP and DIP? Do you really need fine
granularity of objects or better to have coarse granularity?

---

## 🔹 Q27: Is there anything wrong with the following class design? If yes, can the design be improved?

**Answer:**

It’s not a good idea to try to anticipate changes in requirements ahead of time, but you should focus on writing code that is well written enough so that it’ s
easy to change. This means, you should strive to write code that doesn’ t have to be changed every time the requirements change. This is what the Open/Closed
principle is. According to GoF design pattern authors “software entities (classes, modules, functions, etc.) should be open for extension, but closed for
modification”. Spring framework promotes this principle.
In the above example, you can anticipate more operators like “-” (subtraction) and division (/) to be supported in the future and the class “MathOperation” is not
closed for modification. When you need to support operators “-” and “%” you need to add 2 more “else if” statements. Whenever you see large if/else or switch
statements, you need to think if “Open/Closed” design principle is more suited.
Let’s open for extension and close for modifications
In the rewritten example below , the classes AddOperation and MultiplyOperation are closed for modification, but open for extension by allowing you to add
new classes like SubtractOperation and DivisionOperation by implementing the Operation interface.
Define the interface Operation.mport javax .management .RuntimeErrorException ;
mport org.apache .commons .lang.StringUtils ;
public class MathOperation {
public int operate (int input1 , int input2 , String operator ){
 if(StringUtils .isEmpty (operator )){
 throw new IllegalAr gumentException ("Invalid operator: " + operator );
 }
 if(operator .equalsIgnoreCase ("+")){
 return input1 + input2 ;
 }
 else if(operator .equalsIgnoreCase ("*")){
 return input1 * input2 ;
 } else {
 throw new RuntimeException ("unsupported operator: " + operator );
 }

Define the implementationspublic interface Operation {
 abstract int operate (int input1 , int input2 );
}
public class AddOperation implements Operation {
@Override
public int operate (int input1 , int input2 ) {
 return input1 + input2 ;
}
}
public class MultiplyOperation implements Operation {
@Override
public int operate (int input1 , int input2 ) {
 return input1 * input2 ;
}
}

---

## 🔹 Q28: Is DIP related to Dependency Injection (DI)?

**Answer:**

Dependency Inversion Principle ( DIP) is a design principle which is in some ways related to the Dependency Injection (DI) pattern. The idea of DIP is
that higher layers of your application should not directly depend on lower layers. Dependency Inversion Principle does not imply Dependency Injection. This
principle doesn’ t say anything about how higher la yers know what lower layer to use. This could be done as shown below by coding to interface using a factory
pattern or through Dependency Injection by using an IoC container like Spring framework, JEE CDI, Guice, etc.
Loosely coupled by coding to interface

---

## 🔹 Q29: Is there anything wrong with the following class design?
mport java.sql.Connection ;
mport java.sql.SQLException ;

**Answer:**

Violates the SRP . The above class has multiple responsibilities like being a domain object, data access object and a validator .
This principle is based on cohesion . Cohesion is a measure of how strongly a class focuses on its responsibilities. It is of the following two types:
High cohesion: This means that a class is designed to carry on a specific and precise task. Using high cohesion, methods are easier to understand, as they
perform a single task.
Low cohesion: This means that a class is designed to carry on various tasks. Using low cohesion, methods are dif ficult to understand and maintain.
Hence the above code suf fers from low cohesion . The above code can be improved as shown below . The Employee class is re-factored to have only a single
responsibility of uniquely identifying an employee.mport org.apache .commons .lang.StringUtils ;
public class Employee {
 private Integer id;
 private String name ;
 private Connection con = null;
 //getters and setters for above attributes go here..
 public boolean validate ( ) {
 return id != null && id > 0 && StringUtils.isNotBlank(name);
 }
 public void saveEmployee () throws SQLException {
 //save Employee to database using SQL
 //goes here ...
 }
 public boolean validateEmployee ( ) {
 //logic to validate an employee
 }

The responsibility of interacting with the database is shifted to a data access object (i.e. DAO) class. The data access object class takes an employee object or
any of its attributes as input.
Provide an implementation for the above interface. Finally , the responsibility of validating an employee is re-factored to a separate class.

---

## 🔹 Q30: What is your understanding of some of the following most talked terms like Dependency Inversion Principle ( DIP), Dependency Injection ( DI) and
Inversion of Control ( IoC) ?

**Answer:**

Dependency Inversion Principle (DIP) is a design principle which is in some ways related to the Dependency Injection (DI) pattern. The idea of DIP is
that higher layers of your application should not directly depend on lower layers. Dependency Inversion Principle does not imply Dependency Injection. This
principle doesn’ t say anything about how higher layers know what lower layer to use. This could be done as shown in the above example, or through
Dependency Injection by using an IoC container like Spring framework, Pico container , Guice, or Apache HiveMind.public class Employee {
 private Integer id;
 private String name ;
 //getters, setters, equals(..), hashCode(), and toString()
}
public interface EmployeeDao {
 public void saveEmployee (Employee emp);
}
public interface Validator {
 public boolean validate (Employee emp);
}

Dependency Injection (DI) is a pattern of injecting a class’ s dependencies into it at runtime. This is achieved by defining the dependencies as interfaces, and
then injecting in a concrete class implementing that interface to the constructor . This allows you to swap in different implementations without having to modify
the main class. The Dependency Injection pattern also promotes high cohesion by promoting the Single Responsibility Principle (SRP), since your dependencies
are individual objects which perform discrete specialized tasks like data access (via DAOs) and business services (via Service and Delegate classes) .
The Inversion of Contr ol Container (IoC) is a container that supports Dependency Injection. In this you use a central container like Spring framework, Guice,
or HiveMind, which defines what concrete classes should be used for what dependencies through out your application. This brings in an added flexibility
through looser coupling, and it makes it much easier to change what dependencies are used on the fly .
The real power of DI and IoC is realized in its ability to replace the compile time binding of the relationships between classes with binding those r elationships
at runtime . For example, in Seam framework, you can have a real and mock implementation of an interface, and at runtime decide which one to use based on a
property , presence of another file, or some precedence values. This is incredibly useful if you think you may need to modify the way your application behaves in
different scenarios. Another real benefit of DI and IoC is that it makes your code easier to unit test. There are other benefits like promoting looser coupling
without any proliferation of factory and singleton design patterns, follows a consistent approach for lesser experienced developers to follow , etc. These benefits
can come in at the cost of the added complexity to your application and has to be carefully manged by using them only at the right places where the real benefits
are realized, and not just using them because many others are using them.
Note: The CDI (Contexts and Dependency Injection) is an attempt at describing a true standard on Dependency Injection. CDI is a part of the Java EE 6 stack,
meaning an application running in a Java EE 6 compatible container can leverage CDI out-of-the-box. Weld is the reference implementation of CDI.

---

## 🔹 Q31: Can you list some of the key principles to remember while designing your classes?

**Answer:**

1) Favor composition over inheritance.
2) Don’ t Repeat Yourself (DR Y principle). No code or logic duplication.
3) Find what varies and encapsulate it.
4) Code to an interface, and not to an implementation.
5) Strive for loose coupling and high cohesion.
6) Keep It Simple and Stupid (KISS).
7) You Aren’ t Gonna Need It (Y AGNI): Don’ t add functionality until you need it.
8) Design by contract.
Use design principles and patterns, but use them judiciously . Design for current requirements without anticipating future requirements, and over complicating
things. A well designed (i.e. loosely coupled and more cohesive) system can easily lend itself to future changes, whilst keeping the system less complex.

---

## 🔹 Q32: What is design by contract?

**Answer:**

. The design by contract ensures code quality by enforcing that the services of fered by classes and interfaces adhere to unambiguous contracts. The design
by contract is very useful for designing classes and interfaces. These contracts are like checking for an empty or null string, checking for negative numbers,
checking for a particular range, etc.

In general the contracts checked are:
Pre-conditions : These are checked when execution enters a method.
Post-conditions: These are checked when execution exits a method.
Class invariants: These must hold at every observable point.
Contracts are inherited, just like methods. When a method is overridden by a subclass, the subclass may specify its own contracts. This means a subclass may
“weaken the pre-condition and strengthen the post-condition”.
mport java.math .BigDecimal ;
public class Funds {
 private BigDecimal result = null;
 //...
 public BigDecimal calcRate (double rate) {
 //pre-condition check
 if (rate <= BigDecimal .ZERO .doubleV alue( )
 || rate > BigDecimal .TEN .doubleV alue( )) {
 throw new IllegalAr gumentException ( "Outside 0.01% - 10.00%" );
 
 }
 // ... logic to calculate the result
 //post-condition check 
 if (this.result == null
 || this.result .doubleV alue( ) <= BigDecimal .ZERO
 .doubleV alue( )) {
 throw new RuntimeException ("Invalid result" );
 }
 return result ;
 }
 //...

Firstly , in the above code snippet the parameter “ rate” is an invariant that must hold true. The Funds object’ s calculation of the result could be incorrect if an
invalid “rate” is passed as an argument by its caller . A precondition validation is performed on the “rate” to ensure that the caller fulfills the requirements.
Secondly , the implementation of the logic to calculate the result could be defective. So a post-condition validation is performed to verify the correctness of
internal logic.
There are a number of frameworks that support “ design by contract ” like OV al, Jass, etc. The OV al is a validation framework that lets you place the conditions
in different ways like annotations, XML, POJO, or scripting languages like Groovy , JavaScript, OGNL (Object Graph Navigation Language – an expression
language for getting and setting properties of Java objects), etc and also partly implements programming by contract.
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

