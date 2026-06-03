# Design Patterns Overview

> **Module**: Design Patterns  
> **Topic**: Design Patterns Overview

---

## 📋 Table of Contents



- [Q1: Why use design patterns?](#q1)
- [Q2: Why use a factory pattern?](#q2)
- [Q3: How does a builder pattern differ from a factory pattern?](#q3)
- [Q4: How does a builder pattern differ from a flyweight pattern?](#q4)
- [Q5: What is a decorator design pattern?](#q5)
- [Q6: How does a decorator design pattern differ from a proxy design pattern?](#q6)
- [Q7: What is a strategy design pattern?](#q7)
- [Q8: How will you go about counting the occurrences of characters greater than 15 or ](#q8)
- [Q9: What is the difference between a strategy and a command pattern?](#q9)
- [Q10: What is the purpose of an iterator pattern?](#q10)
- [Q11: When you have a sequence of steps to be processed within a method and you want t](#q11)
- [Q12: What is a composite design pattern? Can you explain it with a class diagram and ](#q12)

---

## 🔹 Q1: Why use design patterns?

**Answer:**

1) Captur e design experience fr om the past : E.g. Facade and value object patterns evolved from performance problems experienced due to multiple remote
calls.
Facade
2) Promote r euse without having to r einvent the wheel : E.g. The flyweight pattern improves application performance through object reuse, which minimizes
the overhead such as memory allocation and garbage collection.
3) Define the system structur e better : E.g. The composite design pattern treats nodes and leafs uniformly. The Leaf contains the data, and the Node contains
Leaves. Both the Leaf and Node are treated as a T ree. This is the power of composite design pattern.

Composite design pattern

The builder pattern makes your code more elegant. For example
Instead of
4. Provide a common design vocabulary : Should we use a Data Access Object (DAO)? How about using a Business Delegate? How about a bridge to
minimize the explosion of class hierarchies? How about an adapter to work with the third party libraries? How about a singleton to provide a global point of
access? E.g. A DataSource should have only a single instance where it will supply multiple connections from its single DataSource pool.

---

## 🔹 Q2: Why use a factory pattern?

**Answer:**

Factory pattern returns an instance of several (product hierarchy) subclasses (like Cir cle, Squar e etc), but the calling code is unaware of the actual
implementation class. The calling code invokes the method on the interface for example Shape and using polymorphism the correct draw() method gets
invoked.
So, as you can see, the factory pattern reduces the coupling or the dependencies between the calling code and called objects like Circle, Square etc. This is a
very powerful and common feature in many frameworks. You do not have to create a new Circle or a new Square on each invocation.NetAsset .Builder builder = new NetAsset .Builder ();
NetAsset na = builder .setGrossAssetV alueBeforeT ax(BigDecimal .valueOf ("3500.00" ))
 .setGrossAssetV alueAfterT ax(BigDecimal .valueOf ("500.00" ))
 .setTotalAssets (BigDecimal .valueOf ("3500.00" ))
 .setTotalReceivables (BigDecimal .valueOf ("2500.00" )).build ();
NetAsset na = new NetAsset ();
na.setGrossAssetV alueBeforeT ax(BigDecimal .valueOf ("3500.00" ));
na.setGrossAssetV alueAfterT ax(BigDecimal .valueOf ("500.00" ));na
na.setTotalAssets (BigDecimal .valueOf ("3500.00" ));
na.setTotalReceivables (BigDecimal .valueOf ("2500.00" ));

In future, to conserve memory you can decide to cache objects or reuse objects in your factory with no changes required to your calling code. In other words,
you are applying the flyweight design pattern within your factory to conserve memory .
You can also load objects in your factory based on attribute(s) read from an external properties file or some other condition. Another benefit going for the
factory is that unlike calling constructors directly, factory patterns have more meaningful names like getShape(…), getInstance(…) etc, which may make calling
code more clear .

---

## 🔹 Q3: How does a builder pattern differ from a factory pattern?

**Answer:**

The subtle difference between the builder pattern and the factory pattern is that in builder pattern, the user is given the choice to cr eate the type of
object he/she wants but the construction process is the same. But with the factory method pattern the factory decides how to cr eate one of several possible
classes based on data provided to it.
The builder design pattern builds an object over several steps. It holds the needed state for the tar get item at each intermediate step. The StringBuilder is a good
example that goes through to produce a final string. Here is a custom class example.
mport java.math .BigDecimal ;
public class NetAsset
{
 private final BigDecimal grossAssetV alueAfterT ax;
 private final BigDecimal grossAssetV alueBeforeT ax;
 private NetAsset (Builder builder ) //private constructor
 {
 this.grossAssetV alueAfterT ax = builder .grossAssetV alueAfterT ax;
 this.grossAssetV alueBeforeT ax = builder .grossAssetV alueBeforeT ax;
 }
 
 public BigDecimal getGrossAssetV alueAfterT ax()
 {
 return grossAssetV alueAfterT ax;
 }
 
 public BigDecimal getGrossAssetV alueBeforeT ax()
 {
 return grossAssetV alueBeforeT ax;
 }
 
 //inner builder class

The builder can be used as shown below:

---

## 🔹 Q4: How does a builder pattern differ from a flyweight pattern?

**Answer:**

The Builder pattern is used to create many objects, whereby the Flyweight pattern is about sharing such a collection of objects. The flyweight design
pattern is a structural pattern used to improve memory usage and performance (i.e. due to shorter and less frequent garbage collections). Here, instead of
creating a large number of objects, we reuse the objects that are already created. With fewer objects, your application could fly. public static class Builder
 {
 private BigDecimal grossAssetV alueAfterT ax;
 private BigDecimal grossAssetV alueBeforeT ax;
 public Builder setGrossAssetV alueAfterT ax(BigDecimal grossAssetV alueAfterT ax)
 {
 this.grossAssetV alueAfterT ax = grossAssetV alueAfterT ax;
 return this;
 }
 
 public Builder setGrossAssetV alueBeforeT ax(BigDecimal grossAssetV alueBeforeT ax)
 {
 this.grossAssetV alueBeforeT ax = grossAssetV alueBeforeT ax;
 return this;
 }
 
 //return the built NetAsset
 public NetAsset build ()
 {
 return new NetAsset (this);
 }
 } 
NetAsset .Builder builder = new NetAsset .Builder ();
NetAsset na = builder .setGrossAssetV alueBeforeT ax(BigDecimal .valueOf ("3500.00" ))
 .setGrossAssetV alueAfterT ax(BigDecimal .valueOf ("500.00" )).build ();

Example 1 : In Java, String objects are managed as flyweight. Java puts all fixed String literals into a literal pool. For redundant literals, Java keeps only one
copy in the pool.
Example 2 : The W rapper classes like Integer, Float, Decimal, Boolean, and many other classes having the valueOf static factory method applies the flyweight
design pattern to conserve memory by reusing the objects.

---

## 🔹 Q5: What is a decorator design pattern?

**Answer:**

By implementing the decorator pattern you construct a wrapper around an object by extending its behavior. The wrapper will do its job before or after and
delegate the call to the wrapped instance. The decoration happens at run-time via object composition. A good example is the Java I/O classes as shown
below. Each reader or writer will decorate the other to extend or modify the behavior .String author = "Little brown fox" ;
String authorCopy = "Little brown fox" ;
f(author == authorCopy ) {
 System.out.println ("referencing the same object" );
}
public static void main (String [] args) {
 Integer value1 = Integer .valueOf (5);
 Integer value2 = Integer .valueOf (5);
 if (value1 == value2 ) {
 System.out.println ("referencing the same object" );
 }
}

---

## 🔹 Q6: How does a decorator design pattern differ from a proxy design pattern?

**Answer:**

In Proxy pattern, you have a proxy and a real subject. The relationship between a proxy and the real subject is typically set at compile time, whereas
decorators can be recursively constructed at run time. The Decorator Pattern is also known as the Wrapper pattern. The Proxy Pattern is also known as the
Surr ogate pattern. The purpose of decorator pattern is to add additional responsibilities to an object. These responsibilities can of course be added through
inheritance, but composition provides better flexibility as explained above via the Java I/O classes. The purpose of the proxy pattern is to add an intermediate
between the client and the tar get object. This intermediate shares the same interface as the tar get object. Here are some scenarios in which a proxy pattern can be
applied.
— A remote pr oxy provides a local representative for an object in a different address space.Providing interface for remote resources such as web service
resources, EJBs or RMI (Stub and Skeleton).
— A virtual pr oxy creates expensive object on demand. E.g. Hibernate lazy loaded proxy objects.
— A protection proxy controls access to the original object. Protection proxies are useful when objects should have different access rights. For example, adding
a thread-safe feature to an existing class without changing the existing class’ s code. This is useful when you do not have the freedom to fix thread-safety issues
in a third-party library .

---

## 🔹 Q7: What is a strategy design pattern?

**Answer:**

The Strategy pattern lets you build software as a loosely coupled collection of interchangeable parts, in contrast to a monolithic, tightly coupled system.
Loose coupling makes your software much more extensible, maintainable, and reusable. The main attribute of this pattern is that each strategy encapsulates
algorithms .
Example 1 : You can draw borders around almost all Swing components, including panels, buttons, lists, and so on. Swing provides numerous border types for
its components: bevel, etched, line, titled, and even compound. The various borders are drawn using the strategy pattern.
Example 2 : Strategies to check if a given description is longer than 15 characters, starts with “CD”, etc.String inputT ext = "Some text to read" ;
ByteArrayInputStream bais = new ByteArrayInputStream (inputT ext.getBytes ());
Reader isr = new InputStreamReader (bais);
BufferedReader br = new BufferedReader (isr);
br.readLine ();

Strategy pattern
public interface CheckStrategy {
 public boolean check (String word );
}
public class LongerThan15 implements CheckStrategy {
 public static final int LENGTH = 15; //constant
 public boolean check (String description ) {
 if (description == null)
 return false ;
 else
 return description .length () > LENGTH ;
 }
public class StartsWithCD implements CheckStrategy {
public static final String STARTS_WITH = "cd";

---

## 🔹 Q8: How will you go about counting the occurrences of characters greater than 15 or description starting with “cd”?

**Answer:**

Have a decorator nor wrapper to perform the count first, and then forward the request to the strategy class.
Decoratorpublic boolean check (String description ) {
 String s = description .toLowerCase ();
 if (description == null || description .length () == 0)
 return false ;
 else
 return s.startsWith(STARTS_WITH );
}
public class CountDecorator implements CheckStrategy {
private CheckStrategy cs = null;
private int count = 0;
public CountDecorator (CheckStrategy cs) {
 this.cs = cs;
}
public boolean check (String description ) {
 boolean isFound = cs.check (description );

---

## 🔹 Q9: What is the difference between a strategy and a command pattern?

**Answer:**

Firstly, some examples
Strategy – quicksort or mer gesort, simple vs compound interest calculations, etc
Command – Open or Close actions, redo or undo actions, etc. You need to know the states undo. if (isFound ){
 this.count ++;
 }
 return isFound ;
}
public int count () {
 return this.count ;
}
public void reset () {
 this.count = 0;
}
public interface AbstractCommand {
 abstract void execute ();
}
public class ConcreteCommand implements AbstractCommand {
 private Object arg; //state
 public ConcreteCommand (Object arg) {
 this.arg = arg;

Strategy handles how something should be done by taking the supplied arguments in the execute(….) method. Command creates an object out of what needs to
be done (i.e. hold state ) so that these command objects can be passed around between other classes. The actions the command represent can be undone or
redone by maintaining the state.

---

## 🔹 Q10: What is the purpose of an iterator pattern?

**Answer:**

Iterator provides a way to access the elements of an aggregate object without exposing its underlying implementation.
 }
 @Override
 public void execute () {
 // Work with own state.
 }

Iterator pattern
public interface Iterator {
 public Item nextItem ();
 public Item previousItem ();
 public Item currentItem ();
 public Item firstItem ();
 public Item lastItem ();
 public boolean isDone ();
 public void setStep (int step);
}
public class ShoppingBasketBuilder implements ItemBuilder {
private List listItems = null;
public Iterator getIterator () {
 return listItems .iterator ();
}
public com.item.Iterator getItemIterator () {
 return new ItemsIterator ();
}
/**
 * inner class which iterates over basket of items
 */
class ItemsIterator implements Iterator {
 private int current = 0;
 private int step = 1;
 public Item nextItem () {

 Item item = null;
 current += step;
 if (!isDone ()) {
 item = (Item) listItems .get(current );
 }
 return item;
 }
 
 public Item previousItem () {
 Item item = null;
 current -= step;
 if (!isDone ()) {
 item = (Item) listItems .get(current );
 }
 return item;
 }
 public Item firstItem () {
 current = 0;
 return (Item) listItems .get(current );
 }
 public Item lastItem () {
 current = listItems .size() - 1;
 return (Item) listItems .get(current );
 }
 public boolean isDone () {
 return current >= listItems .size() ? true : false ;
 }
 public Item currentItem () {
 if (!isDone ()) {
 return (Item) listItems .get(current );
 } else {
 return null;
 }
 }
 public void setStep (int step) {
 this.step = step;
 }

---

## 🔹 Q11: When you have a sequence of steps to be processed within a method and you want to defer some of the steps to its subclass, what design pattern will you
use?

**Answer:**

Template method design pattern. Good example of this is the process() method in the Struts RequestProcessor class, which executes a sequence of
pr ocessXXXX(…) methods allowing the subclass to override some of the methods when required.
Template method
prepar eItemForRetail() has 3 steps like 1) add the item to stock 2) apply the bar code and 3) mark a retail price. The abstract base class will implement all the
default behavior. The specific item classes like Boo, CD, etc can either use the default behvior, or override one or more of the step behavior(s).
public abstract class Goods implements Item {
/**
 * The template method
 */
public void prepareItemForRetail () {
 addT oStock ();
 applyBarcode ();
 markRetailPrice ();
}

---

## 🔹 Q12: What is a composite design pattern? Can you explain it with a class diagram and an example?

**Answer:**

The composite design pattern composes objects into tree structures where individual objects like sales staf f and composite objects like managers are
handled uniformly .public void addT oStock (){
 //..default impl
};
public void applyBarcode (){
 //..default impl
};
public void markRetailPrice (){
 //..default impl
};
public class Book extends Goods {
@Override
public void addT oStock () {
 //override default behavior
 //... special logic
}
}

Composite design pattern
public abstract class Employee {
 private String name ;
 private double salary ;
 public Employee (String name, double salary ) {
 this.name = name ;
 this.salary = salary ;
 }
public String getName () {
 return name ;
}

public double getSalaries () {
 return salary ;
}
public abstract boolean addEmployee (Employee emp);
public abstract boolean removeEmployee (Employee emp);
protected abstract boolean hasSubordinates ();
public class Manager extends Employee {
List<Employee > subordinates = null;
public Manager (String name, double salary ) {
 super (name, salary );
}
public boolean addEmployee (Employee emp) {
 if (subordinates == null) {
 subordinates = new ArrayList (10);
 }
 return subordinates .add(emp);
}
public boolean removeEmployee (Employee emp) {
 if (subordinates == null) {
 subordinates = new ArrayList (10);
 }
 return subordinates .remove (emp);
}
/**
 * Recursive method call to calculate the sum of salary of a manager and his subordinates, which
 * means sum of salary of a manager on whom this method was invoked and any employees who
 * themselves will have any subordinates and so on.
 */
public double getSalaries () {

 double sum = super .getSalaries (); //this one's salary
 if (this.hasSubordinates ()) {
 for (int i = 0; i < subordinates .size(); i++) {
 sum += ((Employee ) subordinates .get(i)).getSalaries (); // recursive method call
 }
 }
 return sum;
}
public boolean hasSubordinates () {
 boolean hasSubOrdinates = false ;
 if (subordinates != null && subordinates.size() > 0) {
 hasSubOrdinates = true;
 }
 return hasSubOrdinates ;
}
public class Staff extends Employee {
public Staff(String name, double salary ) {
 super (name, salary );
}
public boolean addEmployee (Employee emp) {
 throw new RuntimeException ("Improper use of Staf f class" );
}
public boolean removeEmployee (Employee emp) {
 throw new RuntimeException ("Improper use of Staf f class" );
}
protected boolean hasSubordinates () {
 return false ;
}

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