# Generics Wildcards

> **Module**: Generics and Collections  
> **Topic**: Generics Wildcards

---

## 📋 Table of Contents



- [Q1: If java.lang. Object is the super type for java.lang. Number and, Number is the ](#q1)
- [Q2: How will you go about deciding which of the following to use?
<Number>
<? extend](#q2)
- [Q3: Why was the read(….) method in “GenericSingleT ypeScenario.java” was implemented](#q3)
- [Q4: What if you use just use “ ?” instead of “? extends T”?](#q4)
- [Q5: Where will you apply the wild card “ ? super T “?](#q5)

---

## 🔹 Q1: If java.lang. Object is the super type for java.lang. Number and, Number is the super type for java.lang. Integer, am I correct in saying that List<Object> is
the super type for List<number> and, List<Number> is the super type for List<Integer> .

**Answer:**

No. List<Object> is not the the super type of List<Number>. If that were the case, then you could add objects of any type, and it defeats the purpose of
Generics.
* Compile Error: T ype mismatch. Cannot convert from ArrayList<Integer> to List<Number>*/
List<Number > numbers2 = new ArrayList <Integer >();
/ Compiles
List<Integer > numbers3 = new ArrayList <Integer >();
/ Compile Error: Can't instantiate with wildcard
List<? super Integer > numbers6 = new ArrayList <? super Integer >();
/ Compiles
List<? super Integer > numbers7 = new ArrayList <Integer >();
numbers7 .add(Integer .valueOf (5)); //ok
/ Compiles
List<? extends Number > numbers5 = new ArrayList <Number >();
/ Compile error: Read only. Can't add

In Generics, wildcards (i.e. ?), makes it possible to work with super classes and sub classes .

---

## 🔹 Q2: How will you go about deciding which of the following to use?
<Number>
<? extends Number>
<? super Number>

**Answer:**

Here is the guide:
1. Use the ? extends wildcard if you need to retrieve object from a data structure. That is read only. You can’ t add elements to the collection.
2. Use the ? super wildcard if you need to put objects in a data structure.
3. If you need to do both things (read and add elements), don’ t use any wildcard.
Scenario 1: A custom generic class GenericSingleT ypeScenario class that handles a given input of type T, where “T” can be any type.numbers5 .add(Integer .valueOf (5));
mport java.util.ArrayList ;
mport java.util.Collection ;
mport java.util.List;
public class GenericSingleT ypeScenario <T> {
 List<T> vals = new ArrayList <T>();
 public void read(List<? extends T> vals) {
 for (T val : vals) {
 System.out.println ("read: " + val);
 }
 }
 public void add(T val) {
 vals.add(val);
 }

Add a test class with a main method to test the above class with “T” being an “Integer” public void addAll (Collection <? extends T> val) {
 vals.addAll (val);
 }
 public void addAndRead (T valToAdd ) {
 vals.add(valToAdd );
 for (T valRead : vals) {
 System.out.println ("readAndW rite: " + valRead );
 }
 }
 public List<T> getValues () {
 return vals;
 }
mport java.util.Arrays ;
public class GenericSingleT ypeScenarioT est {
 public static void main (String [] args) {
 GenericSingleT ypeScenario <Integer > singleT ype = new GenericSingleT ypeScenario <Integer >();
 
 singleT ype.add(1);
 singleT ype.read(singleT ype.getValues ());
 singleT ype.add(6); // autoboxes 6 to type Integer
 
 Integer [] moreV als = {22, 33}; 
 
 singleT ype.addAll (Arrays .asList (moreV als));
 singleT ype.addAndRead (9);
 }

Output:
Scenario 2 : It can handle any given input of type Number like Integer, Long, and Double .
read: 1
readAndW rite: 1
readAndW rite: 6
readAndW rite: 22
readAndW rite: 33
readAndW rite: 9
mport java.util.Arrays ;

Output:
Scenario 3: “T” being a “String”.public class GenericSingleT ypeScenarioT est {
 public static void main (String [] args) {
 GenericSingleT ypeScenario <Number > singleT ype = new GenericSingleT ypeScenario <Number >();
 
 singleT ype.add(1.0);
 singleT ype.read(singleT ype.getValues ());
 singleT ype.add(6.0); // autoboxes 6.0 to type Double
 
 Long [] moreV als = {22L, 33L}; 
 
 singleT ype.addAll (Arrays .asList (moreV als));
 singleT ype.addAndRead (9);
 }
read: 1.0
readAndW rite: 1.0
readAndW rite: 6.0
readAndW rite: 22
readAndW rite: 33
readAndW rite: 9

Output:mport java.util.Arrays ;
public class GenericSingleT ypeScenarioT est {
 public static void main (String [] args) {
 GenericSingleT ypeScenario <String > singleT ype = new GenericSingleT ypeScenario <String >();
 
 singleT ype.add("Apple" );
 singleT ype.read(singleT ype.getValues ());
 singleT ype.add("Orange" ); 
 
 String [] moreV als = {"Tomato", "Avacado" }; 
 
 singleT ype.addAll (Arrays .asList (moreV als));
 singleT ype.addAndRead ("Grapes" );
 }

Scenario 4: “T” being any “Object”
Output:read: Apple
readAndW rite: Apple
readAndW rite: Orange
readAndW rite: Tomato
readAndW rite: Avacado
readAndW rite: Grapes
mport java.util.Arrays ;
public class GenericSingleT ypeScenarioT est {
 public static void main (String [] args) {
 GenericSingleT ypeScenario <Object > singleT ype = new GenericSingleT ypeScenario <Object >();
 singleT ype.add(1);
 singleT ype.add(6.0);
 
 Long [] moreV als = {22L, 33L}; 
 singleT ype.addAll (Arrays .asList (moreV als));
 singleT ype.addAndRead ("Apple" );
 singleT ype.read(singleT ype.getValues ());
 }

Why use wildcards?

---

## 🔹 Q3: Why was the read(….) method in “GenericSingleT ypeScenario.java” was implemented with wildcard “ ? extends T ” as opposed to just “T” as shown
below:

**Answer:**

The reason being what if you want to read a List<Double> from that method. If you were to NOT use the wildcard as in “ ? extends T “ the following code
would have a compile error .eadAndW rite: 1
eadAndW rite: 6.0
eadAndW rite: 22
eadAndW rite: 33
eadAndW rite: Apple
ead: 1
ead: 6.0
ead: 22
ead: 33
ead: Apple
public void read(List<T> vals) { //instead of List<? extends T>
 for (T val : vals) {
 System.out.println ("read: " + val);
 }
}

This is the reason why “? extends T” was introduced to be able to read List<Integer>, List<Double>, List<Long>, etc in a generic manner .

---

## 🔹 Q4: What if you use just use “ ?” instead of “? extends T”?

**Answer:**

The above code would compile, but the read(…) method must be changed to use “ Object ” as shown below as it could take a List<String> as well.
and the following code will compile.mport java.util.ArrayList ;
mport java.util.List;
public class GenericSingleT ypeScenarioT est {
 public static void main (String [] args) {
 GenericSingleT ypeScenario <Number > listOfInts = new GenericSingleT ypeScenario <Number >();
 listOfInts .add(1); // can add Number or subclasses of Number
 
 List<Double > numbers = new ArrayList <Double >();
 numbers .add(5.0);
 
 listOfInts .read(numbers ); // Compile Error because 'T' is a Number and
 // it won't work with a Double
 }
public void read(List<?> vals) {
 for (Object val : vals) {
 System.out.println ("read: " + val);
 }
}

---

## 🔹 Q5: Where will you apply the wild card “ ? super T “?

**Answer:**

If you were to add the following method to “GenericSingleT ypeScenario.java”
The following code will compile.mport java.util.ArrayList ;
mport java.util.List;
public class GenericSingleT ypeScenarioT est {
 public static void main (String [] args) {
 GenericSingleT ypeScenario <Number > listOfInts = new GenericSingleT ypeScenario <Number >();
 listOfInts .add(1); // can add Number or subclasses of Number
 
 List<String > fruits = new ArrayList <String >();
 fruits .add("Apple" );
 
 listOfInts .read(fruits );
 }
public void add(List<? super T> list, T val) {
 vals.add(val);
}

Output:
The following code without the wildcard “? super T”mport java.util.ArrayList ;
mport java.util.List;
public class GenericSingleT ypeScenarioT est {
 public static void main (String [] args) {
 GenericSingleT ypeScenario <Integer > listOfInts = new GenericSingleT ypeScenario <Integer >();
 listOfInts .add(2);
 
 List<Number > doubles = new ArrayList <Number >();
 doubles .add(5.0);
 
 listOfInts .add(doubles, 6);
 
 System.out.println (doubles );
 }
5.0, 6]

will throw a compile error as shown below as “ T” is an “Integer” and List<Number> cannot be assigned to add(…..) method’ s first argument List<Integer>
But it will be able to assigned to List<? super Integer> as “Number” is a super class of “Integer”.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »public void add(List<T> list, T val) {
 list.add(val);
}
mport java.util.ArrayList ;
mport java.util.List;
public class GenericSingleT ypeScenarioT est {
 public static void main (String [] args) {
 GenericSingleT ypeScenario <Integer > listOfInts = new GenericSingleT ypeScenario <Integer >();
 listOfInts .add(2);
 
 List<Number > doubles = new ArrayList <Number >();
 doubles .add(5.0);
 
 listOfInts .add(doubles, 6 // !!!! COMPILE ERROR
 
 System.out.println (doubles );
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