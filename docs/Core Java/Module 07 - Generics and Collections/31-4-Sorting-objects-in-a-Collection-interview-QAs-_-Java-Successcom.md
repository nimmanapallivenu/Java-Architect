# 31. 4 Sorting objects in a Collection interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: If I mention the interface names Comparable or Comparator , what does come to yo](#q1)
- [Q2: What if your collection contains custom objects like a Pet class?](#q2)
- [Q3: What if you have an additional special sorting requirement to sort first by id a](#q3)
- [Q4: What contract do you need to watch out for when writing your own comparator?](#q4)

---

## 🔹 Q1: If I mention the interface names Comparable or Comparator , what does come to your mind? Why do we need these interfaces?

**Answer:**

Sorting . SortedSet and SortedMap interfaces maintain sorted order . The elements are sorted as you add or remove them. The other interfaces like List or
Set don’ t sort elements as you add or remove. So you need to sort them on a as needed basis.
If you store objects of type String or Integer in a List or Set, and would like to occasionally sort them, say for reporting purpose, you can do so as shown below
as String or Integer by default implements the Comparable interface and provides a compareT o(..) method to be called while sorting.
Output:
Before sorting: [Cereal, Apples, Soap, Brush]
After sorting: [Apples, Brush, Cereal, Soap]
As you can see that the items are sorted lexicographically . This is the default implementation provided by the compar eTo(…) method in the java.lang.String
class. What if you have a special reporting requirement to sort by length of the item name. This is where the Comparator interface comes in handy by giving youmport java.util.ArrayList ;
mport java.util.Collections ;
mport java.util.List;
public class Sorting1 {
 
 public static void main (String [ ] args) {
 List<String > myShoppingList = new ArrayList <String >( );
 myShoppingList .add("Cereal" );
 myShoppingList .add("Apples" );
 myShoppingList .add("Soap" );
 myShoppingList .add("Brush" );
 System .out.println ("Before sorting: " + myShoppingList );
 // invokes compareT o method implemented in the String class.
 Collections .sort(myShoppingList ); 
 System .out.println ("After sorting: " + myShoppingList ); 
 }

more control over ordering. You can define your own ordering logic through the compar e(…) method as shown below using the Comparator interface.
Output:mport java.util.ArrayList ;
mport java.util.Collections ;
mport java.util.Comparator ;
mport java.util.List;
public class Sorting2 {
 
 public static void main (String [ ] args) {
 List<String > myShoppingList = new ArrayList <String >( );
 myShoppingList .add("Cereal" );
 myShoppingList .add("Apples" );
 myShoppingList .add("Soap" );
 myShoppingList .add("Brush" );
 myShoppingList .add(null);
 System .out.println ("Before sorting: " + myShoppingList );
 
 //Anonymous inner class.
 Collections .sort(myShoppingList , new Comparator <String >( ) {
 @Override
 public int compare (String o1, String o2) {
 if(o1 == null) {
 o1 = "";
 }
 if(o2 == null) {
 o2 = "";
 }
 return new Integer (o1.length ( )).compareT o(o2.length ( ));
 }
 });
 System .out.println ("After sorting: " + myShoppingList ); 
 }

Before sorting: [Cereal, Apples, Soap, Brush]
After sorting: [Soap, Brush, Cereal, Apples]
Note: The above class is using an anonymous class to sort, but if you require to reuse the sorting in a number of places, you must move the compare(…) method
to its own class as shown below .
You can use it as follows
Comparable interface for natural ordering

---

## 🔹 Q2: What if your collection contains custom objects like a Pet class?

**Answer:**

You can provide the default sorting behavior by having the Pet class implement the Comparable interface and implementing the compareT o(…) method as
shown below:mport java.util.Comparator ;
public class NameLengthComparator implements Comparator <String > {
 
 public int compare (String o1, String o2) {
 //implementation goes here. same as above
 }
} 
Collections .sort(myShoppingList , new NameLengthComparator ( ));

Take note of generics being used above. The above Pet class can be used as shown below .public class Pet implements Comparable <Pet> {
 int id;
 String name ;
 public Pet(int id, String name ) {
 this.id = id;
 this.name = name ;
 }
 // getters and setters go here
 //invoked during sorting
 public int compareT o(Pet o) {
 Pet petAnother = o;
 // natural alphabetical ordering by name
 return this.name .compareT o(petAnother .name );
 }
 //invoked when the list is printed
 public String toString ( ) {
 return "[id=" + id + ", name=" + name + "]";
 }
mport java.util.ArrayList ;
mport java.util.Collections ;
mport java.util.List;
public class Sorting3 {
 
 public static void main (String [ ] args) {
 List<Pet> myPetList = new ArrayList <Pet>( );
 myPetList .add(new Pet(1, "Dog" ));
 myPetList .add(new Pet(2,"Rabit" ));
 myPetList .add(new Pet(3,"Cat" ));

Output:
Before sorting: [[1,Dog], [2,Rabit], [3,Cat], [2,Hamster]]
After sorting: [[3,Cat], [1,Dog], [2,Hamster], [2,Rabit]]

---

## 🔹 Q3: What if you have an additional special sorting requirement to sort first by id and then by name?

**Answer:**

You can use the Comparator interface to sort based on multiple attributes as shown below . myPetList .add(new Pet(2, "Hamster" ));
 System .out.println ("Before sorting: " + myPetList );
 //compareT o method gets invoked on Pet.class
 Collections .sort(myPetList ); 
 System .out.println ("After sorting: " + myPetList ); 
 }
mport java.util.ArrayList ;
mport java.util.Collections ;
mport java.util.Comparator ;
mport java.util.List;
public class Sorting4 {
 public static void main (String [ ] args) {
 List<Pet> myPetList = new ArrayList <Pet>( );
 myPetList .add(new Pet(1, "Dog" ));
 myPetList .add(new Pet(2, "Rabit" ));
 myPetList .add(new Pet(3, "Cat" ));
 myPetList .add(new Pet(2, "Hamster" ));
 System .out.println ("Before sorting: " + myPetList );
 Collections .sort(myPetList , new Comparator <Pet>( ) {
 @Override
 public int compare (Pet o1, Pet o2) {
 int byIds = o1.getId ( ).compareT o(o2.getId ( ));
 // if ids are same, compare by name
 if (byIds == 0) {
 return o1.getName ()

Output:
Before sorting: [[1,Dog], [2,Rabit], [3,Cat], [2,Hamster]]
After sorting: [[1,Dog], [2,Hamster], [2,Rabit], [3,Cat]]
Note: The above class is using an anonymous class to sort, but if you require to reuse the sorting in a number of places, you must move the compar e(…) method
to its own class.

---

## 🔹 Q4: What contract do you need to watch out for when writing your own comparator?

**Answer:**

As per the Java API for java.util.Comparator , caution should be exercised when using a comparator capable of imposing an ordering inconsistent with the
equals(…) method.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » .compareT oIgnoreCase (o2.getName ()); 
 }
 return byIds ;
 }
 });
 System .out.println ("After sorting: " + myPetList );
 }
f compare (o1,o2) == 0 then o1.equals (o2) should be true.
f compare (o1,o2) != 0 then o1.equals (o2) should be false .

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

