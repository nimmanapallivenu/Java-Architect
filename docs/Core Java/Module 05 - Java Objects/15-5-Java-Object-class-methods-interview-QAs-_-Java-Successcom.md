# 15. 5 Java Object class methods interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: What are the non-final methods in Java Object class, which are meant primarily f](#q1)
- [Q2: Are these methods easy to implement correctly?](#q2)
- [Q3: What are the implications of implementing them incorrectly?](#q3)
- [Q4: If a class overrides the equals( ) method, what other method it should override?](#q4)
- [Q5: Why or when should you override a toString( ) method?](#q5)
- [Q6: Can you override clone( ) and finalize( ) methods in the Object class? How do yo](#q6)

---

## 🔹 Q1: What are the non-final methods in Java Object class, which are meant primarily for extension?

**Answer:**

The non-final methods are
equals( ), hashCode( ), toString( ), clone( ), and finalize( ).
These methods are meant to be overridden . The equals( ) and hashCode( ) methods prove to be very important, when objects implementing these two methods
are added to collections. If implemented incorrectly or not implemented at all, then your objects stored in a Map may behave strangely and also it is hard to
debug.
The other methods like wait( ), notify( ), notifyAll( ), getClass( ), etc are final methods and therefore cannot be overridden. The methods clone( ) and finalize( )
have protected access .

---

## 🔹 Q2: Are these methods easy to implement correctly?

**Answer:**

No. It is not easy to implement these methods correctly because,
1) You must pay attention to whether your implementation of these methods will continue to work correctly if sub classed. If your class is not meant for
extension, then declare your class as final.
2) These methods have to adhere to contracts, and when implementing or overriding a method, the contracts must be satisfied.
Equals() Vs hashCode()

---

## 🔹 Q3: What are the implications of implementing them incorrectly?

**Answer:**

In short, excess debug and rework time. Incorrect implementations of equals( ) and hashCode( ) methods can result in data being lost in HashMaps and
Sets. You can also get intermittent data related problems that are harder to consistently reproduce over time.
Here is an example of equals() and hashCode() methods being invoked implicitly in adding and retrieving objects from a Map:

Java equals vs hashCode

The above example uses a custom key class MyKey that takes “ name ” and “ date” as attributes. So, this key class needs to implement equals() and hashCode()
methods using these 2 attributes.
a & b ) When you put an object into a map with a key and a value, hashCode() method is implicitly invoked, and hash code value say 123 is returned. T wo
different keys can return the same hash value. A good hashing algorithm spreads out the numbers. In the above example, let’ s assume that (“John”,01/01/1956)
key and (“Peter”, 01/01/1995) key return the same hash value of 123.
Q. So, when we retrieve the object with a specific key , how does it know which one of two objects to return?
A. This is where the equals() method is implicitly invoked to get the object value with the right key . So, it needs to loop through each key/value pair in the
bucket “123” to find the right key by comparing “name” & “date” via equals() method.
Q. What if you change the hashCode() method of the “MyKey” class to always return a constant value of? W ill you be able to add objects to the map?
A. Yes, you will be able to add objects, but all the key/value pairs will be added to the single bucket, and the equals() method needs to loop through all the
objects in the same bucket.

hashCode() returns 1
1 & 2 ). The hashCode() method picks the right bucket 123. Then loop through all the values and invoke the equals() method to pick the right key , which is
(“John”,01/01/1956).

---

## 🔹 Q4: If a class overrides the equals( ) method, what other method it should override? What are some of the key considerations while implementing these
methods?

**Answer:**

It should override the hashCode( ) method . The contract between the hashCode( ) and equals( ) methods is clearly defined on the Java API for the Object
class under respective methods. Here are some key points to keep in mind while implementing these methods,
1) The equals( ) and hashCode( ) methods should be implemented together . You should not have one without the other .
2) If two objects return the same hashCode( ) integer value does not mean that those two objects are equal.
3) If two objects are equal, then they must return the same hashCode( ) integer value.
4) The implementation of the equals( ) method must be consistent with the hashCode( ) method to meet the previous bullet points.
5) Use @override annotation to ensure that the methods are correctly overridden.
6) Favor instanceof instead of getClass(..) in the equals( ) method which takes care of super types and null comparison as recommended by Joshua Bloch, but
ensure that the equals implementation is final to preserve the symmetry contract of the method: x.equals(y) == y .equals(x). If final seems restrictive, carefully
examine to see if overriding implementations can fully maintain the contract established by the Object class.
7) Check for self-comparison and null values where required.
public final class Pet {
 int id;
 String name ;
 /**
 * use @override annotation to prevent the danger of misspelling
 * method name or incorrect method signature.
 */
 
 @Override
 public boolean equals (Object that){
 //check for self-comparison
 if ( this == that ) return true;
 /**
 * use instanceof instead of getClass here for two reasons

Use Apache’ s HashCodeBuilder & EqualsBuilder classes to simplify your implementation, especially when you have a large number of member variables.
Commonclipse is an eclipse plugin for jakarta commons-lang users. It is very handy for automatic generation of toString( ), hashCode( ), equals(..), and
compareT o( ) methods. * 1. it can match any super type, and not just one class;
 * 2. explicit check for "that == null" is not required as
 * "null instanceof Pet" always returns false.
 **/
 if ( ! (that instanceof Pet) )
 return false ;
 
 Pet pet = (Pet)that;
 return this.id == pet.id && this != null && this.name.equals(pet.name);
 
 }
 /**
 * fields id & name are used in both equals( ) and
 * hashCode( ) methods. 
 */
 
 @Override
 public int hashCode ( ) {
 int hash = 9;
 hash = (31 * hash) + id;
 hash = (31 * hash) + (null == name ? 0 : name .hashCode ( ));
 return hash;
 }
public final class Pet2 {
 int id;
 String name ;
 @Override
 public boolean equals (Object that) {
 if (this == that)

toString() method

---

## 🔹 Q5: Why or when should you override a toString( ) method?

**Answer:**

You can use System.out.println( ) or logger .info(…) to print any object. The toString( ) method of an object gets invoked automatically , when an object
reference is passed in the System.out.println(refPet) or logger .info(refPet) method. However for good results, your class should have a toString( ) method that
overrides Object class’ s default implementation by formatting the object’ s data in a sensible way and returning a String. Otherwise all that’ s printed is the class
name followed by an “@” sign and then unsigned hexadecimal representation of the hashCode. For example, If the Pet class doesn’ t override the toString( )
method as shown below , by default Pet@162b91 will be printed via toString( ) default implementation in the Object class.

---

## 🔹 Q6: Can you override clone( ) and finalize( ) methods in the Object class? How do you disable a clone( ) method?

**Answer:**

es, but you need to do it very judiciously . Implementing a properly functioning clone( ) method is complex and it is rarely necessary . You are better of f
providing some alternative means of object copying through a copy constructor or a static factory method.
Unlike C++ destructors, the finalize( ) method in Java is unpredictable, often dangerous and generally unnecessary . Use finally{} blocks to close any resources
or free memory . The finalize( ) method should only be used in rare instances as a safety net or to terminate noncritical native resources. If you do happen to call
the finalize( ) method in some rare instances, then remember to follow the following guidelines: return true;
 if (!(that instanceof Pet2))
 return false ;
 Pet2 pet = (Pet2) that;
 return new EqualsBuilder ( ).append (this.id, pet.id).append (
 this.name , pet.name ).isEquals ( );
 }
 /**
 * both fields id & name are used in equals( ),
 * so both fields must be used in hashCode( ) as well.
 */
 @Override
 public int hashCode ( ) {
 //pick 2 hard-coded, odd & >0 int values as arguments
 return new HashCodeBuilder (1, 31).append (this.id).append (
 this.name ).toHashCode ( );
 }

1) You should call the finalize method of the super class in case it has to clean up.
2) You should not depend on the finalize method being called. There is no guarantee that when (or if) objects will be garbage collected and thus no guarantee
that the finalize method will be called before the running program terminates.
3) Finally , the code in finalize method might fail and throw exceptions. Catch these exceptions so that the finalize method can continue.
How do you disable a clone( ) method?
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »protected void finalize ( ) throws Throwable {
 try{
 //finalize subclass state
 }
 catch (Throwable t){
 //log the exception
 }
 finally {
 super .finalize ( );
 }
public final Object clone ( ) throws CloneNotSupportedException {
 throw new CloneNotSupportedException ( );
}

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

