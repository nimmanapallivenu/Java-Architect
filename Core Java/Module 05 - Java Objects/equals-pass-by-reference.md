# equals() vs == & Pass by Reference

> **Module**: Java Objects  
> **Topic**: equals() vs == & Pass by Reference

---

## 📋 Table of Contents



- [Q1: What is the difference between “==” and equals(..) method when comparing 2 obje](#q1)
- [Q2: What happens when you run the following code?public final class Pet {
 int id;
 ](#q2)
- [Q3: Can you discuss the output of the following code?Boolean b1 = Boolean .valueOf (](#q3)
- [Q4: Can you discuss the output of the following code?primitives a and b are ==
Objec](#q4)
- [Q5: Explain the statement Java is always pass by value?](#q5)
- [Q6: The value of Point p before the following method calls is (10,20). What will be ](#q6)
- [Q7: If there is a source array list with 15MB of data, and then you create a new tar](#q7)

---

## 🔹 Q1: What is the difference between “==” and equals(..) method when comparing 2 objects?

**Answer:**

It is important to understand the difference between identity (i.e. ==) comparison, which is a shallow comparison that compares only the object references,
and the equals( ) comparison, which is a deeper comparison that compares the object attributes. The diagram below explains the difference between the two.
There are some exceptional conditions when using primitives, String objects, and enums.
If the equals(..) method is not overridden, then the Object class’ s default implementation is invoked, which only compares the object references. Invoking the
equals(..) method of the Object class is equivalent to making a shallow comparison with “==”. This is why it is imperative to override the equals( ) method, and
the hashCode( ) methods in your custom classes like Pet. The Java API objects like String, and the wrapper classes like Integer, Double, Float, etc override the
equals(..), hashCode( ), and the toString(..) methods.

Java identity ==

Deeper equals( ) compares values by invoking equals(
) method
If you wer e to implement the equals(…) and hashCode(…) methods:

You can learn more at “ Sorting objects in Java interview Q&As ”

---

## 🔹 Q2: What happens when you run the following code?public final class Pet {
 int id;
 String name ;
 
 @Override
 public boolean equals (Object that){
 if ( this == that ) return true;
 if ( ! (that instanceof Pet) ){
 return false ;
 }
 
 Pet pet = (Pet)that;
 return this.id == pet.id && this != null && this.name.equals(pet.name); 
 }
 @Override
 public int hashCode ( ) {
 int hash = 9;
 hash = (31 * hash) + id;
 hash = (31 * hash) + (null == name ? 0 : name .hashCode ( ));
 return hash;
 }
Boolean b1 = new Boolean (false );
Boolean b2 = Boolean .FALSE ;

**Answer:**

Prints “Not Equal”.
The == is a shallow comparison that only compares the references. The references are not equal. If you want to print “Equal”, perform a deeper comparison as
shown below, which compares the values.
Or, you need to take advantage of the flyweight design pattern that reuses objects.
orf(b1 == b2) {
 System.out.println ("Equal" );
else{
 System.out.println ("Not Equal" );
f (b1.equals (b2)){
 System.out.println ("Equal" );//gets printed
}
else {
 System.out.println ("Not Equal" );
}
Boolean b1 = Boolean .valueOf ("false" ); // create a false object if not already present
Boolean b2 = Boolean .FALSE; //points to the same object as above

---

## 🔹 Q3: Can you discuss the output of the following code?Boolean b1 = Boolean .valueOf ("false" ); // create a false object if not already present
Boolean b2 = Boolean .valueOf ("false" ); //points to the same object as above
public class PrimitiveAndObjectEquals {
 public static void main (String [ ] args) {
 int a = 5;
 int b = 5;
 Integer c = new Integer (5);
 Integer d = new Integer (5);
 if (a == b) { //Line 1
 System.out.println ("primitives a and b are ==" );
 }
 if (c == d) { //Line 2
 System.out.println ("Objects c and d are ==" );
 }
 if (c.equals (d)) { //Line 3
 System.out.println ("Objects c and d are equals( )" );
 }
 if (a == d) { //Line 4
 System.out
 .println ("Primitive a and Object d are == due to auto unboxing" );
 }
 }

**Answer:**

Output is:
1) Line 1 is printed as both a and b are primitive data types, and primitives are compared with == as they don’ t have an equals() method.
2) Line 2 will not get printed as they are comparing the object references (shallow comparison). Line 3 will get printed as they are comparing the actual values
(deep er comparison).
3) Line 4 is printed because the object reference “d” is auto-unboxed to a primitive int value and then compared with the primitive reference “a”. This also
illustrates a hidden chance of a NullPointerException being thrown if the reference “d” were to be null.

---

## 🔹 Q4: Can you discuss the output of the following code?primitives a and b are ==
Objects c and d are equals ( )
Primitive a and Object d are == due to auto unboxing
public class EnumEquals {
 
public enum Action {START, STOP, CONTINUE }
 
private static Action action = Action .STOP;
 
public static void main (String [ ] args) {
 
 if(Action .STOP == action ){
 System.out.println ("Enumurations can be compared to ==." );
 }
 
 if(Action .STOP.equals (action )){

**Answer:**

Output is:
The best practice is to use the referential == for enums.

---

## 🔹 Q5: Explain the statement Java is always pass by value?

**Answer:**

Other languages use pass-by-reference or pass-by-pointer. But in Java, no matter what type of argument (i.e. a primitive variable or an object reference) you
pass, the corresponding parameter will get a copy of that data, which is exactly how pass-by-value (i.e. copy-by-value) works. Even though the definition is
quite straight forward, the way the primitives and object references behave when passed by value, will be different .
For example, If the passed in argument was a primitive value like int, char, etc, the passed in primitive value is copied to the method parameter. Modifying the
copied parameter will not modify the original primitive value. System.out.println ("Enumurations can be compared to equals( ) also." );
 }
 }
Enumurations can be compared to ==.
Enumurations can be compared to equals ( ) also.

pass-by-value primitive variables like int, long, etc
On the contrary, if the passed in argument was an object reference, the passed in reference is copied to the method parameter. The copied reference will still be
pointing to the same object. So if you modify the object value through the copied reference, the original object will be modified.
pass by value for objects like int[], Pet, Car, etc

---

## 🔹 Q6: The value of Point p before the following method calls is (10,20). What will be the value of Point p after executing the following method calls?
Scenario 1:
Scenario 2:

**Answer:**

Scenario 1:
Point p = (50,100), as the copied reference will still be pointing and modifying the original Point (10,2 0) object through the mutatePoint( ) method.
Scenario 2:
Point p = (10,20), as the copied reference will be creating and pointing to the newly created Point (50, 100) object.

---

## 🔹 Q7: If there is a source array list with 15MB of data, and then you create a new tar get empty array list and copy the source to tar get with tar get.addAll(source).
How much memory will be consumed after invoking the addAll(…) method?static void mutatePoint (Point p) {
 p.x = 50;
 p.y=100;
}
static void mutatePoint (Point p) {
 p = new Point (50,100);
}

**Answer:**

The memory will still be 15MB because of “ pass-by-value ” where new objects are not created when addAll(…) is invoked. Only the references are copied,
but the copied references will still be pointing to the source list objects. For example,
Java is pass by value
package com.test;
mport java.util.ArrayList ;
mport java.util.Arrays ;

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »mport java.util.List;
public class PassByReference {

 public static void main (String [] args) {
 
 List<String > source = Arrays .asList ("One", "Two", "Three", "Four" ); //say 15MB
 
 List<String > target = new ArrayList <String >();
 
 target.addAll (source );
 
 //memory will still be 15MB, why?
 
 System.out.println (source .get(0) == target.get(0)); //outputs true.
 
 //This is because source and tar get lists reference the same objects.
 //No new objects are created by addAll(...)
 //Only the references are copied 
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