# Autoboxing & Unboxing

> **Module**: Java Data Types  
> **Topic**: Autoboxing & Unboxing

---

## 📋 Table of Contents



- [Q1: What do you understand by the terms “autoboxing” and “autounboxing” in Java?](#q1)
- [Q2: What are the benefits of autoboxing?](#q2)
- [Q3: What are some of the pitfalls of autoboxing?](#q3)
- [Q4: How will you go about debugging auto boxing and unboxing error?](#q4)

---

## 🔹 Q1: What do you understand by the terms “autoboxing” and “autounboxing” in Java?

**Answer:**

Java automatically converts a primitive type like “int” into corresponding wrapper object class Integer. This is known as the autoboxing. When it converts
a wrapper object class Integer back to its primitive type “int”, it is know as “ autounboxing “.
Example 1:
This can be applied to one of 8 primitivies in Java to convert from primitive to wrapper via autoboxing and from wrapper to primitive via autounboxing.
Autoboxing and unboxing can happen anywhere where an object is expected and primitive type is available
Example 2:package com.autoboxing ;
public class AutoBoxUnbox {
 public static void main (String [] args) {
 int i = 5;
 Integer objI = i; //autoboxing takes place by invoking Integer .valueOf(i);
 if(objI != null)
 {
 int result = objI + 3; //auto unboxing takes place by "objI.intV alue() + 3"
 System.out.println (result );
 }
 }
package com.autoboxing ;

---

## 🔹 Q2: What are the benefits of autoboxing?

**Answer:**

Less code to write, and the code looks cleaner .
For example, you don’ t have to do as shown below:
More readable with autoboxingmport java.util.ArrayList ;
mport java.util.HashMap ;
mport java.util.List;
mport java.util.Map;
public class AutoBoxUnbox {
 public static void main (String [] args) {
 List<Character > characters = new ArrayList <>();
 characters .add('C'); //autoboxed to Character object and then added to the list
 Map<Long, Double > myMap = new HashMap <>();
 myMap .put(5L, 12.50 ); //autoboxed to Long.valueOf(5L), Double.valueOf(12.50)
 char myChar = characters .get(0); //unboxed
 System.out.println (myChar );
 double myDouble = myMap .get(5L); //unboxed
 System.out.println (myDouble );
 }
ist.add(Integer .valueOf (6));

---

## 🔹 Q3: What are some of the pitfalls of autoboxing?

**Answer:**

It is very convenient to havae autoboxing, but it can cause issues and many beginners fall into it caveats.
1. Unnecessary Object cr eation due to Autoboxing
Q. How do you know unnecessary objects are being created?
A. jmap to the rescue.
Step 1: Run the above code.
Step 2: Open a DOS or Unix command prompt and run the following commands. “jps” to find the process id, and then “jmap” to print the object graphist.add(6);
package com.autoboxing ;
public class AutoBoxUnbox {
 public static void main (String [] args) throws InterruptedException {
 Integer sum = 0;
 for (int i = 1000; i < 500000; i++) {
 sum += i;
 Thread .sleep (100);
 }
 }

Step 3: Inspect the mem.txt file
after some time
You can see the growing instances and bytes.
Now try the samething after fixing the code as shown below .C:\>jps
8148
8420 Jps
3832 JConsole
8896 AutoBoxUnbox
10300 JConsole
10948 JConsole
C:\>jmap -histo :live 8896 > mem .txt
num #instances #bytes class name
---------------------------------------------
 7: 1318 21088 java.lang.Integer
num #instances #bytes class name
---------------------------------------------
 7: 1704 27264 java.lang.Integer

after some timepackage com.autoboxing ;
public class AutoBoxUnbox {
 public static void main (String [] args) throws InterruptedException {
 int sum = 0; //FIX. change to primitive type
 for (int i = 1000; i < 500000; i++) {
 sum += i;
 Thread .sleep (100);
 }
 }
num #instances #bytes class name
---------------------------------------------
 8: 256 4096 java.lang.Integer
num #instances #bytes class name
---------------------------------------------
8: 256 4096 java.lang.Integer

The improved code does not create unnecessary Integer objects. You may also like the detailed “ javap, jps, jmap, and jvisualvm tutorial – analyzing the heap
histogram ”
2. GC overhead
Unnecessarily creating too many objects and then discarding them will increase the Garbage Collection overhead. This may cause performance impact due to
more frequent garbage collection.
3. java.lang.NullPointerException
Especially when mixing object and primitive in equality and relational operator .
Conditional operators can cause NullPointerException .package com.autoboxing ;
public class AutoBoxUnbox {
 public static void main (String [] args) throws InterruptedException {
 Integer i = null;
 if(i > 6) { // tries to do i.intV alue(); i is null so java.lang.NullPointerException is thrown here
 System.out.println ("I am in here" );
 }
 }

Since d1 is primitive, d2 is implicitly tried to auto unbox. T o fix it, you need to change d1 to wrapper object type “ Double “. This way auto unboxing won’ t take
place.
4. Overloading
Q. What will be the output of the following code?package com.autoboxing ;
public class AutoBoxUnbox {
 public static void main (String [] args) throws InterruptedException {
 boolean b = false ;
 double d1 = 0d;
 Double d2 = null;
 Double d = b ? d1 : d2; //NullPointerException when "d2.doubleV alue()" is evaluated.
 }
package com.autoboxing ;
public class AutoBoxUnbox {
 public static void main (String [] args) throws InterruptedException {
 Integer value = 0;
 new AutoBoxUnbox ().eval(value );
 }
 void eval(long val) {
 System.out.println (1);
 }
 void eval(Long value ) {
 System.out.println (2);
 }

A. The result is 1, because there is no direct conversion from Integer to Long, so the “conversion” from Integer to long is used.

---

## 🔹 Q4: How will you go about debugging auto boxing and unboxing error?

**Answer:**

1) Being aware of the potential auto boxing and unboxing caveats discussed above.
2) Configuring your IDE to pick up auto boxing and unboxing error. For example, in eclipse
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »Java --> Compiler --> Errors /Warnings --> "Potential programming problems" --> "Boxing and unboxing conversions"

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