# Immutable Objects

> **Module**: Java Objects  
> **Topic**: Immutable Objects

---

## 📋 Table of Contents



- [Q2: Immutable objects are objects whose state (the object’ s data) cannot change aft](#q2)
- [Q3: Is the following class immutable?public class User {
 private final String first](#q3)
- [Q4: What are the advantages of immutable objects?](#q4)
- [Q5: Why is it a best practice to implement the user defined key class as an immutabl](#q5)
- [Q6: How would you defensively copy a Date field in your immutable class?](#q6)
- [Q7: How will you prevent the caller from adding or removing elements from a collecti](#q7)
- [Q8: How about data that needs to be mutable, but less frequently? Is there any way t](#q8)
- [Q9: Can builder design pattern be used to create immutable objects?](#q9)
- [Q10: Can you give a builder design pattern example to create immutable objects?](#q10)

---

## 🔹 Q2: Immutable objects are objects whose state (the object’ s data) cannot change after construction. Examples of immutable objects from the JDK include String
and wrapper classes like Integer, Double, Character, etc.
Q2.How do you create an immutable type?

**Answer:**

1. Make the class final so that it cannot be extended or use static factories and keep the constructors private.
2. Make the fields private and final.
3. Don’ t provide any methods that can change the state of the immutable object in any way from outside the object – not just setXXX methods, but any methods
which can change the state.
4. The “ this” reference is not allowed to escape during construction from the immutable class, and the immutable class should have exclusive access to fields
that contain references to other mutable objects like arrays, collections and mutable classes like Date by:
a) Declaring the mutable references as private.
b) Not returning or exposing the mutable references to the caller. This can be done by defensively copying the objects by deeply cloning them.
Example: satisfying the above conditionspublic final class MyImmutable { … }
private final int[ ] myArray ;

---

## 🔹 Q3: Is the following class immutable?public class User {
 private final String firstName; //final, and String class itself is immutable
 private final String lastName; //final, and String class itself is immutable
 // constructor is private
 private User (String firstName, String lastName ) {
 this.firstName = firstName ;
 this.lastName = lastName ;
 }
 //factory method
 public static User getInstance (String firstName, String lastName ) {
 return new User (firstName, lastName );
 }
 //only getters, no setters
 
 public String getFirstName () {
 return firstName ;
 }
 public String getLastName () {
 return lastName ;
 }
mport java.util.Arrays ;
public final class MyImmutable {

**Answer:**

No. The above class is not immutable as it fails #4 condition where the “myArray” reference can escape, and mutated from outside as demonstrated below .
Output: private final Integer [ ] myArray ;
 public MyImmutable (Integer [ ] anArray ) {
 this.myArray = anArray ;
 }
 public Integer [ ] getMyArray ( ) {
 return myArray ;
 }
 //....equals(), hashCode(), etc
 @Override
 public String toString () {
 return Arrays .toString (myArray );
 }
public class MyImmutableT est {
 public static void main (String [] args) {
 Integer [] array1 = {1,2,3};
 MyImmutable mi = new MyImmutable (array1 );
 System.out.println ("before modifying: " + mi);
 mi.getMyArray ()[2] = 4; //change 3 to 4
 System.out.println ("after modifying: " + mi);
 }

FIX: “myArray” reference is deeply copied, and can’t escape
If you run the “MyImmutableT est” again,before modifying : [1, 2, 3]
after modifying : [1, 2, 4] 
mport java.util.Arrays ;
public final class MyImmutable {
 private final Integer [] myArray ;
 public MyImmutable (Integer [] anArray ) {
 this.myArray = anArray .clone (); //cloned array assigned
 }
 public Integer [] getMyArray () {
 return myArray .clone (); // cloned array is returned
 }
 // ....equals(), hashCode(), etc
 @Override
 public String toString () {
 return Arrays .toString (myArray );
 }

---

## 🔹 Q4: What are the advantages of immutable objects?

**Answer:**

1) Immutable classes can greatly simplify programming by freely allowing you to cache and share the references to the immutable objects without having to
defensively copy them or without having to worry about their values becoming stale or corrupted.
2) Immutable classes are inherently thread-safe and you do not have to synchronize access to them to be used in a multi-threaded environment. So there are no
chances for negative performance consequences as multiple threads can share the same instance.
3) Eliminates the possibility of data becoming inaccessible when used as keys in HashMaps or as elements in Sets. These types of errors are hard to debug and
fix.
4) Eliminates the need for class invariant check once constructed.
5) Allow hashCode( ) method to use lazy initialization, by caching its return value.
6) Cloning is not required.
7) Simpler to construct, use, and test due to its deterministic state.

---

## 🔹 Q5: Why is it a best practice to implement the user defined key class as an immutable object?

**Answer:**

Immutable objects generally make the best map keys as the keys cannot be modified once they have been added to the Map. In general String, Integer, or
Long are used as keys, which are immutable objects. If you define your own key class, make sure that they are immutable. Otherwise, if the keys are
accidentally modified after adding to a Map, you will never be able to retrieve the stored value as the key values have been changed. This is a common pitfall
many Java developers, especially beginners fall for .
Example :
Immutable key classbefore modifying : [1, 2, 3]
after modifying : [1, 2, 3]

mport java.util.Date ;
public final class MyKey {
 private final String name ;
 private final Date myDate ;
 public MyDiary (Date aDate ) {
 this.myDate = new Date (aDate .getTime( ));//defensive copying by not exposing the “myDate” reference 
 }
 public Date getDate ( ) {
 return new Date (myDate .getTime( )); //defensive copying by not exposing the “myDate”
 }
 public String getName ( ) {
 return name; //String is immutable
 }
 
 //...equals( ), hashCode(), etc

hashCode( ) Vs. equals( )

As shown, when Maps are used in Java, the equals( ) and hashCode( ) methods are implicitly invoked. If these methods are incorrectly implemented or the keys
are modified once added to the map, then unpredictable behavior will be experienced, and these behaviors are harder to debug. The hashCode( ) and equals( )
methods are implicitly invoked to determine where the key is stored, and to retrieve the value for a particular key respectively. More than one key/value pairs
can be stored in the same bucket.
The hashCode( ) method does not give a unique value each time. Its duty is to spread out the numbers so that your key value pairs get spread out in multiple
buckets. So, always remember this.
The hashCode( ) method is used to store the key/value in a bucket, and both the hashCode() and equals() methods are called to retrieve the stored key/value. If
they are implemented inconsistently or the key is mutated, then the stored object cannot be retrieved as the returned values of these methods will vary in
between different invocations. T o be more specific, the hashCode( ) method is called to determine the key index (aka the bucket) of the array, and the equals( )
method is called to retrieve the exact key from the list of keys belonging to that particular key index (or bucket) as the same bucket will be holding multiple
keys linked to multiple values. Remember the contract between these two methods?
“If 2 objects are equal, they must r eturn the same hashCode( ) value, but the r everse is not true. Which means, if 2 objects r eturn the same hashCode( )
value does not mean that those 2 objects are equal( )” .

---

## 🔹 Q6: How would you defensively copy a Date field in your immutable class?

**Answer:**

public final class MyDiary {
 private final Date myDate ;
 public MyDiary (Date aDate ) {
 this.myDate = new Date (aDate .getTime( )); //defensive copying by not exposing the “myDate” reference
 }
 public Date getDate ( ) {
 return new Date (myDate .getTime( )); //defensive copying by not exposing the “myDate” 
 }

---

## 🔹 Q7: How will you prevent the caller from adding or removing elements from a collection of pets?

**Answer:**

It ensures that you cannot add or remove pets. However, there is no guarantee that the pets are also immutable. T o make this instance fully immutable, the Pet
instance itself must be immutable or use the decorator pattern as a wrapper around each of the pets to make them also immutable. For example, The Integer
wrapper class pr ovides immutability to mutable primitive int value. You could also defensively deep copy the list of pets in the constructor and getPets( )
method.

---

## 🔹 Q8: How about data that needs to be mutable, but less frequently? Is there any way to obtain the benefits of immutability with the added benefit of thread-safety
for data that changes less frequently?

**Answer:**

The Copy-On-W rite collections like CopyOnW riteArrayList and CopyOnW riteArraySet classes introduced from JSE 5.0 util.concurrent package are good
examples of how to harness the power of immutability whist permitting occasional modifications for infrequently changing data. CopyOnW riteArrayList
behaves much like the ArrayList class, except that when the list is modified, instead of mutating the underlying array, a new array is created and the old array is
discarded. CopyOnW riteArrayList is designed for cases where:
— reads hugely outnumber writes.
— the array is small (or writes are very infrequent).
— the caller genuinely needs the functionality of a list rather than an array .mport java.util.ArrayList ;
mport java.util.Collections ;
mport java.util.List;
public class PetCage {
 private final List<Pet> pets;
 public PetCage (List<Pet> pets) {
 this.pets = Collections .unmodifiableList (new ArrayList <Pet>(pets)); 
 }
 public List<Pet> getPets ( ) {
 return pets; 
 }

When you obtain an iterator, which holds a reference to the underlying array, the array referenced by the iterator is ef fectively immutable and therefore can be
traversed without synchronization or risk of concurrent modification. This eliminates the need to either clone the list before traversal or synchronize on the list
during traversal. If reads are much more frequent than insertions or removals, which is the case very often, the Copy-on-W rite collections and
Concurr entHashMaps offer better performance and development convenience. The development convenience is provided not needing to deal with
synchronization, deep cloning, or “ConcurrentModificationException”. The ConcurrentModificationException is generally thrown by an ArrayList, HashSet, or
a HashMap implementation when you try to remove an object from a collection while iterating over it.

---

## 🔹 Q9: Can builder design pattern be used to create immutable objects?

**Answer:**

Yes.

---

## 🔹 Q10: Can you give a builder design pattern example to create immutable objects?

**Answer:**

Example : Builder pattern and immutability in Java .
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