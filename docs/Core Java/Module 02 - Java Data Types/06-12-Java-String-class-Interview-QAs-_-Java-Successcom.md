# 06. 12 Java String class Interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: What is the difference between “==” and “equals(…)” in comparing Java String ob](#q1)
- [Q2: Can you explain how Strings are interned in Java?](#q2)
- [Q3: What will be the output of the following code snippet?](#q3)
- [Q4: What is the main difference between String , StringBuffer , and StringBuilder ?](#q4)
- [Q5: Can you write a method that reverses a given String?](#q5)
- [Q6: Can you remember a design pattern discussed in this post?](#q6)
- [Q7: Can you give some examples of the usage of the flyweight design pattern in Java?](#q7)
- [Q8: What is a static factory method, and when will you use it?](#q8)
- [Q9: How will you split the following string of text into individual vehicle types?
“](#q9)
- [Q10: What are the different ways to concatenate strings? and which approach is most ](#q10)
- [Q11: Java being a stack based language, allows you to make recursive method calls. Ca](#q11)

---

## 🔹 Q1: What is the difference between “==” and “equals(…)” in comparing Java String objects?

**Answer:**

When you use “==” (i.e. shallow comparison), you are actually comparing the two object references to see if they point to the same object . When you use
“equals(…)”, which is a “deep comparison” that compares the actual string values . For example:
The variable s1 refers to the String instance created by “Hello”. The object referred to by s2 is created with s1 as an initializer , thus the contents of the two
String objects are identical, but they are 2 distinct objects having 2 distinct references s1 and s2. This means that s1 and s2 do not refer to the same object and
are, therefore, not ==, but equals( ) as they have the same value “Hello”. The s1 == s3 is true, as they both point to the same object due to internal caching. The
references s1 and s3 are interned and points to the same object in the string pool.public class StringEquals {
public static void main (String [ ] args) {
 String s1 = "Hello" ;
 String s2 = new String (s1);
 String s3 = "Hello" ;
 System .out.println (s1 + " equals " + s2 + "--> " + s1.equals (s2)); //true
 System .out.println (s1 + " == " + s2 + " --> " + (s1 == s2)); //false
 System .out.println (s1 + " == " + s3+ " --> " + (s1 == s3)); //true

Create a String object as a literal without the “new” keyword for caching
In Java 6 — all interned strings were stored in the PermGen – the fixed size part of heap mainly used for storing loaded classes and string pool.
In Java 7 – the string pool was relocated to the heap . So, you are not restricted by the limited size.
How about comparing the other objects like Integer , Boolean, and custom objects like “Pet”? Object equals Vs “ ==“, and pass by reference Vs value .

---

## 🔹 Q2: Can you explain how Strings are interned in Java?

**Answer:**

String class is designed with the Flyweight design pattern in mind. Flyweight is all about re-usability without having to create too many objects in
memory .
[ Further Reading: Flyweight pattern and improve memory usage & performance ]
A pool of Strings is maintained by the String class. When the intern( ) method is invoked, equals(..) method is invoked to determine if the String already exist
in the pool. If it does then the String from the pool is returned instead of creating a new object. If not already in the string pool, a new String object is added to
the pool and a reference to this object is returned. For any two given strings s1 & s2, s1.intern( ) == s2.intern( ) only if s1.equals(s2) is true.
Two String objects are created by the code shown below . Hence s1 == s2 returns false.

s1.intern() == s2.intern() returns true, but you have to remember to make sure that you actually do intern() all of the strings that you’re going to compare. It’ s
easy to for get to intern() all strings and then you can get confusingly incorrect results. Also, why unnecessarily create more objects?
Instead use string literals as shown below to intern automatically:
s1 and s2 point to the same String object in the pool. Hence s1 == s2 returns true.
Since interning is automatic for String literals String s1 = “A”, the intern( ) method is to be used on Strings constructed with new String(“A”).

---

## 🔹 Q3: What will be the output of the following code snippet?

**Answer:**

The output will be/Two new objects are created. Not interned and not recommended.
String s1 = new String ("A");
String s2 = new String ("A");
String s1 = "A";
String s2 = "A";
String s = " Hello " ;
s += " World " ;
s.trim( );
System .out.println (s);

with the leading and trailing spaces . Some would expect a trimmed “Hello W orld”. So, what concepts does this question try to test?
1. String objects are immutable and there is a trick in s.trim( ) line.
2. Concept of object references and unreachable objects that are eligible for garbage collection. 3 String objects are created, and 2 of them become
unreachable as there are no references to them, and gets garbage collected.
What follow on questions can you expect?
1. You might get a follow on question on how many string objects are created in the above example and when will it become an unreachable object to be
garbage collected.
2. You might also be asked a follow on question as to if the above code snippet is ef ficient.
The best way to explain this is via a self-explanatory diagram as shown below . Click on it to enlarge.
" Hello World "

No of String objects created
If you want the above code to output “Hello W orld” with leading and trailing spaces trimmed then assign the s.trim( ) to the variable “s”. This will make the
reference “s” to now point to the newly created trimmed String object.
The above code can be rewritten as shown below
StringBuilder sb = new StringBuilder (" Hello " );
sb.append (" World " );
System .out.println (sb.toString ().trim( ));

---

## 🔹 Q4: What is the main difference between String , StringBuffer , and StringBuilder ?

**Answer:**

String is immutable in Java, and this immutability gives the benefits like security and performance discussed above.
StringBuffer is mutable , hence you can add strings to it, and when required, convert to an immutable String with the toString( ) method.
StringBuilder is very similar to a StringBuffer , but StringBuffer has one disadvantage in terms of performance as all of its public methods are
synchronized for thread-safety . StringBuilder in Java is a copy of StringBuffer but without synchronization to be used in local variables which are
inherently thread-safe. So, if thread-safety is required, use StringBuf fer, otherwise use StringBuilder .

---

## 🔹 Q5: Can you write a method that reverses a given String?

**Answer:**

A popular Java interview coding question.
Example 1 : It is always a best practice to reuse the API methods as shown below with the StringBuilder(input).r everse( ) method as it is fast, ef ficient (uses bit
wise operations) and knows how to handle Unicode surrogate pairs, which most other solutions ignore. The code shown below handles null and empty strings,
and a StringBuilder is used as opposed to a thread-safe StringBuf fer, as the StringBuilder is locally defined, and local variables are implicitly thread-safe.
Example 2 : Some interviewers might probe you to write other lesser elegant code using either recursion or iterative swapping. Some developers find it very
difficult to handle recursion, especially to work out the termination condition. All recursive methods need to have a condition to terminate the recursion.
Recursive solution.public static String reverse (String input ) { 
 if(input == null || input .length ( ) == 0){ 
 return input ; 
 } 
 
 return new StringBuilder (input ).reverse ( ).toString ( ); 

Java Recursion – String examplepublic String reverse (String str) { 
 // exit or termination condition 
 if ((null == str) || (str.length ( ) <= 1)) { 
 return str; 
 } 
 
 // put the first character (i.e. charAt(0)) to the end. String indices are 0 based. 
 // and recurse with 2nd character (i.e. substring(1)) onwards 
 return reverse (str.substring (1)) + str.charAt (0); 

Step 1: reverse(“RA W”)
Step 2: reverse(A W) + “R” [Note: charAt[0] = “R”, and str .substring(1) = “A W” ]
Step 3: reverse(W) + “A” + “R” [Note: charAt[0] = “A”, and str .substring(1) = “W” ]
Step 4: return “W” + “A” + “R” [Exit condition is r eached when “str.length( ) <=1” ]
outputs: “WAR”
Example 3 : Iterative solution.

---

## 🔹 Q6: Can you remember a design pattern discussed in this post?

**Answer:**

Flyweight design pattern . The flyweight design pattern is a structural pattern used to improve memory usage (i.e. due to fewer objects and object reuse)
and performance (i.e. due to shorter and less frequent garbage collections).public String reverse (String str) { 
 // validate 
 if ((null == str) || (str.length ( ) <= 1)) { 
 return str; 
 } 
 
 char[ ] chars = str.toCharArray ( ); 
 int rhsIdx = chars .length - 1; 
 
 //iteratively swap until exit condition lhsIdx < rhsIdx is reached 
 for (int lhsIdx = 0; lhsIdx < rhsIdx ; lhsIdx ++) { 
 char temp = chars [lhsIdx ]; 
 chars [lhsIdx ] = chars [rhsIdx ]; 
 chars [rhsIdx --] = temp ; 
 } 
 
 return new String (chars );

---

## 🔹 Q7: Can you give some examples of the usage of the flyweight design pattern in Java?

**Answer:**

Example 1 : As discussed above, String objects are managed as flyweight. Java puts all fixed String literals into a literal pool. For redundant literals, Java keeps
only one copy in the pool.
Example 2 : The W rapper classes like Integer , Float , Decimal , Boolean , and many other classes like BigDecimal having the valueOf static factory method to
apply the flyweight design pattern to conserve memory by reusing the objects.String author = "Little brown fox" ; 
String authorCopy = "Little brown fox" ; 
/only 1 String object is created. Both author and authorCopy point to that
f(author == authorCopy ) { 
 System .out.println ("referencing the same object" ); 
} 
public class FlyWeightW rapper {
public static void main (String [] args) {
 Integer value1 = Integer .valueOf (5);
 Integer value2 = Integer .valueOf (5);
 //only one object is created
 if (value1 == value2 ) {
 System .out.println ("referencing the same object" );
 }

If you use new Integer(5), a new object will be created every time.
Both the above examples will print “ referencing the same object “.

---

## 🔹 Q8: What is a static factory method, and when will you use it?

**Answer:**

The factory method pattern is a way to encapsulate object creation. It has the benefits like
1. Factory can choose what to return from many subclasses or implementations of an interface. This allows the caller to specify the behavior desired via
parameters, without having to know or understand a potentially complex class hierarchy . The lesser a caller knows about a callee’ s internal details, the more
loosely coupled a callee is from the caller .
2. The factory can apply the fly weight design pattern to cache objects and return cached objects instead of creating a new object every time. In other words,
objects can be pooled and reused. This is the reason why you should favor using Integer .valuOf(6) as opposed to new Integer(6) .
3. The factory methods have more meaningful names than the constructors. For example, getInstance( ) , valueOf( ) , getConnection( ) , deepCopy( ) , etc.

---

## 🔹 Q9: How will you split the following string of text into individual vehicle types?
“Car,Jeep, W agon Scooter Truck, V an”

**Answer:**

Regular expressions to the rescue.public static List<Car> deepCopy (List<Car> listCars ) {
 List<Car> copiedList = new ArrayList <Car>(10);
 for (Car car : listCars ) { //JDK 1.5 for each loop
 Car carCopied = new Car( ); //instantiate a new Car object
 carCopied .setColor ((car.getColor ( )));
 copiedList .add(carCopied );
 }
 return copiedList ;

---

## 🔹 Q10: What are the different ways to concatenate strings? and which approach is most ef ficient?

**Answer:**

Plus (“+”) operator :
Using a StringBuilder or StringBuffer class.public class String3 {
 public static void main (String [ ] args) {
 String pattern = "[,\\s]+" ; //regex pattern – a comma or white space repeated 1 or more times
 
 String vehicles = "Car,Jeep, W agon Scooter Truck, V an";
 String [ ] result = vehicles .split(pattern );
 for (String vehicle : result ) {
 System .out.println ("Vehicle = \"" + vehicle + "\"");
 }
 }
String s1 = ”John ” + “Davies ”;
StringBuilder sb = new StringBuilder (“John ”);
sb.append (“Davies ”);

Using the concat(…) method.
The ef ficiency depends on what you are concatenating and how you are concatenating it.
Concatenating constants: Plus operator is more ef ficient than the other two as the JVM optimizes constants.
Concatenating String variables: Any one of the three methods should do the job.
Concatenating in a for/while loop: StringBuilder or StringBuf fer is the most ef ficient. Avoid using plus operator as it is the worst of fender .“John ”.concat (“Davies ”);
String s1 = ”John ” + “Davies ”;
String s1 = s2 + s3 + s4;
String s1 = “name =”;
s1 += name ;

Prefer StringBuilder to StringBuf fer unless multiple threads can have access to it.

---

## 🔹 Q11: Java being a stack based language, allows you to make recursive method calls. Can you write a recursion based solution to count the number of A ’s in
string “AAA rating”?

**Answer:**

A function is recursive if it calls itself. Given enough stack space, recursive method calls are perfectly valid in Java though it is tough to debug. Recursive
functions are useful in removing iterations from many sorts of algorithms.
Recursion in stack based language like JavaStringBuilder sb = new StringBuilder (250);
for( int i=0; i<SIZE ; i++ ) {
 sb.append (“Item:” + i);
}

Recursion might not be the ef ficient way to code, but recursive functions are shorter , simpler , and easier to read and understand. Recursive functions are very
handy in working with tree structures and avoiding unsightly nested for loops.
Bonus Java String Q&A
Q12 . How do you stream a string class in Java 8? ★ ♟
A12 . chars() method.public class RecursiveCall {
 public int countA (String input ) {
 
 // exit condition – recursive calls must have an exit condition
 if (input == null || input .length ( ) == 0) {
 return 0;
 }
 int count = 0;
 
 //check first character of the input
 if (input .substring (0, 1).equals ("A")) {
 count = 1;
 }
 
 //recursive call to evaluate rest of the input
 //(i.e. 2nd character onwards)
 return count + countA (input .substring (1));
 }
 public static void main (String [ ] args) {
 System .out.println (new RecursiveCall ( ).countA ("AAA rating" )); // 3
 }

Q. Does parallel processing as shown below preserve the order?
A. No.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »public static void main (String [] args) {
 "cactus" .chars ().forEach (c -> System .out.println ((char)c));
}
public static void main (String [] args) {
 "cactus" .chars ().parallel ().forEach (c -> System .out.println ((char)c));
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

