# 8 12+ Java String class interview questions and answers

## Table of Contents

- [Q1: What will be the output of the following code snippet?](#q1)
- [Q2: What is the main dif ference between String , StringBuffer , and StringBuilder ?](#q2)
- [Q3: Can you write a method that reverses a given String?](#q3)
- [Q4: Can you remember a design pattern discussed in this post?](#q4)
- [Q5: Can you give some examples of the usage of the flyweight design pattern in Java?](#q5)
- [Q6: What is a static factory method, and when will you use it?](#q6)
- [Q7: How will you split the following string of text into individual vehicle types?
“](#q7)
- [Q8: What are the dif ferent ways to concatenate strings? and which approach is most ](#q8)
- [Q9: Java being a stack based language, allows you to make recursive method calls. Ca](#q9)

---

## Q1: What will be the output of the following code snippet?

**Answer:**

The output will be
with the leading and trailing spaces . Some would expect a trimmed “Hello W orld”. So, what concepts does this question try to test?
1. String objects are immutable and there is a trick in s.trim( ) line.
2. Concept of object references and unreachable objects that are eligible for garbage collection. 3 String objects are created, and 2 of them become
unreachable as there are no references to them, and gets garbage collected.
What follow on questions can you expect?
1. You might get a follow on question on how many string objects are created in the above example and when will it become an unreachable object to be
garbage collected.
2. You might also be asked a follow on question as to if the above code snippet is ef ficient.
The best way to explain this is via a self-explanatory diagram as shown below . Click on it to enlar ge.
String s = " Hello " ;
s += " World " ;
s.trim( );
System .out.println (s);
" Hello World "

No of String objects created
If you want the above code to output “Hello W orld” with leading and trailing spaces trimmed then assign the s.trim( ) to the variable “s”. This will make the
reference “s” to now point to the newly created trimmed String object.
The above code can be rewritten as shown below

---

## Q2: What is the main dif ference between String , StringBuffer , and StringBuilder ?

**Answer:**

String is immutable in Java, and this immutability gives the benefits like security and performance discussed above.
StringBuffer is mutable , hence you can add strings to it, and when required, convert to an immutable String with the toString( ) method.
StringBuilder is very similar to a StringBuffer , but StringBuffer has one disadvantage in terms of performance as all of its public methods are
synchronized for thread-safety . StringBuilder in Java is a copy of StringBuffer but without synchronization to be used in local variables which are
inherently thread-safe. So, if thread-safety is required, use StringBuf fer, otherwise use StringBuilder .

---

## Q3: Can you write a method that reverses a given String?

**Answer:**

A popular Java interview coding question.
Example 1 : It is always a best practice to reuse the API methods as shown below with the StringBuilder(input).r everse( ) method as it is fast, ef ficient (uses bit
wise operations) and knows how to handle Unicode surrogate pairs, which most other solutions ignore. The code shown below handles null and empty strings,
and a StringBuilder is used as opposed to a thread-safe StringBuf fer, as the StringBuilder is locally defined, and local variables are implicitly thread-safe.StringBuilder sb = new StringBuilder (" Hello " );
sb.append (" World " );
System .out.println (sb.toString ().trim( ));

Example 2 : Some interviewers might probe you to write other lesser elegant code using either recursion or iterative swapping. Some developers find it very
difficult to handle recursion, especially to work out the termination condition. All recursive methods need to have a condition to terminate the recursion.
Recursive solution.public static String reverse (String input ) { 
 if(input == null || input .length ( ) == 0){ 
 return input ; 
 } 
 
 return new StringBuilder (input ).reverse ( ).toString ( ); 
 
public String reverse (String str) { 
 // exit or termination condition 
 if ((null == str) || (str.length ( ) <= 1)) { 
 return str; 
 } 
 
 // put the first character (i.e. charAt(0)) to the end. String indices are 0 based. 
 // and recurse with 2nd character (i.e. substring(1)) onwards 
 return reverse (str.substring (1)) + str.charAt (0); 

Java Recursion – String example
Step 1: reverse(“RA W”)
Step 2: reverse(A W) + “R” [ Note: charAt[0] = “R”, and str .substring(1) = “A W” ]
Step 3: reverse(W) + “A” + “R” [ Note: charAt[0] = “A”, and str .substring(1) = “W” ]
Step 4: return “W” + “A” + “R” [ Exit condition is r eached when “str .length( ) <=1” ]
outputs: “WAR”
Example 3 : Iterative solution.

---

## Q4: Can you remember a design pattern discussed in this post?

**Answer:**

Flyweight design pattern . The flyweight design pattern is a structural pattern used to improve memory usage (i.e. due to fewer objects and object reuse)
and performance (i.e. due to shorter and less frequent garbage collections).

---

## Q5: Can you give some examples of the usage of the flyweight design pattern in Java?

**Answer:**

Example 1 : As discussed above, String objects are managed as flyweight. Java puts all fixed String literals into a literal pool. For redundant literals, Java keeps
only one copy in the pool.public String reverse (String str) { 
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
 
String author = "Little brown fox" ; 
String authorCopy = "Little brown fox" ; 
/only 1 String object is created. Both author and authorCopy point to that 

Example 2 : The W rapper classes like Integer , Float , Decimal , Boolean , and many other classes like BigDecimal having the valueOf static factory method to
apply the flyweight design pattern to conserve memory by reusing the objects.
If you use new Integer(5), a new object will be created every time.
Both the above examples will print “ referencing the same object “.

---

## Q6: What is a static factory method, and when will you use it?

**Answer:**

The factory method pattern is a way to encapsulate object creation. It has the benefits like
1. Factory can choose what to return from many subclasses or implementations of an interface. This allows the caller to specify the behavior desired via
parameters, without having to know or understand a potentially complex class hierarchy . The lesser a caller knows about a callee’ s internal details, the more
loosely coupled a callee is from the caller .f(author == authorCopy ) { 
 System .out.println ("referencing the same object" ); 
} 
public class FlyW eightW rapper {
public static void main (String [] args) {
 Integer value1 = Integer .valueOf (5);
 Integer value2 = Integer .valueOf (5);
 //only one object is created
 if (value1 == value2 ) {
 System .out.println ("referencing the same object" );
 }

2. The factory can apply the fly weight design pattern to cache objects and return cached objects instead of creating a new object every time. In other words,
objects can be pooled and reused. This is the reason why you should favor using Integer .valuOf(6) as opposed to new Integer(6) .
3. The factory methods have more meaningful names than the constructors. For example, getInstance( ) , valueOf( ) , getConnection( ) , deepCopy( ) , etc.

---

## Q7: How will you split the following string of text into individual vehicle types?
“Car,Jeep, W agon Scooter T ruck, V an”

**Answer:**

Regular expressions to the rescue.public static List deepCopy (List listCars ) {
 List copiedList = new ArrayList (10);
 for (Car car : listCars ) { //JDK 1.5 for each loop
 Car carCopied = new Car( ); //instantiate a new Car object
 carCopied .setColor ((car.getColor ( )));
 copiedList .add(carCopied );
 }
 return copiedList ;
public class String3 {
 public static void main (String [ ] args) {
 String pattern = "[,\\s]+" ; //regex pattern – a comma or white space repeated 1 or more times
 
 String vehicles = "Car,Jeep, W agon Scooter Truck, V an";
 String [ ] result = vehicles .split(pattern );
 for (String vehicle : result ) {
 System .out.println ("Vehicle = \"" + vehicle + "\"");

---

## Q8: What are the dif ferent ways to concatenate strings? and which approach is most ef ficient?

**Answer:**

Plus (“+”) operator :
Using a StringBuilder or StringBuffer class.
Using the concat(…) method.
The ef ficiency depends on what you are concatenating and how you are concatenating it. }
 }
String s1 = ”John ” + “Davies ”;
StringBuilder sb = new StringBuilder (“John ”);
sb.append (“Davies ”);
“John ”.concat (“Davies ”);

Concatenating constants: Plus operator is more ef ficient than the other two as the JVM optimizes constants.
Concatenating String variables: Any one of the three methods should do the job.
Concatenating in a for/while loop: StringBuilder or StringBuf fer is the most ef ficient. A void using plus operator as it is the worst of fender .
Prefer StringBuilder to StringBuf fer unless multiple threads can have access to it.

---

## Q9: Java being a stack based language, allows you to make recursive method calls. Can you write a recursion based solution to count the number of A ’s in string
“AAA rating”?

**Answer:**

A function is recursive if it calls itself. Given enough stack space, recursive method calls are perfectly valid in Java though it is tough to debug. Recursive
functions are useful in removing iterations from many sorts of algorithms.String s1 = ”John ” + “Davies ”;
String s1 = s2 + s3 + s4;
String s1 = “name =”;
s1 += name ;
StringBuilder sb = new StringBuilder (250);
for( int i=0; i<size; i++="" )="" {="" sb.append (“item:”="" +="" i);="" }="" <="" code =""></size;>

Recursion in stack based language like Java
public class RecursiveCall {
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

Recursion might not be the ef ficient way to code, but recursive functions are shorter , simpler , and easier to read and understand. Recursive functions are very
handy in working with tree structures and avoiding unsightly nested for loops.
Learn more at Recursion Vs. T ail Recursion .
Bonus Java String Q&A
Q10 . How do you stream a string class in Java 8? ★ ♟
A10 . chars() method.
Q. Does parallel pr ocessing as shown below preserve the order? 
 //recursive call to evaluate rest of the input
 //(i.e. 2nd character onwards)
 return count + countA (input .substring (1));
 }
 public static void main (String [ ] args) {
 System .out.println (new RecursiveCall ( ).countA ("AAA rating" )); // 3
 }
public static void main (String [] args) {
 "cactus" .chars ().forEach (c -> System .out.println ((char)c));
}
public static void main (String [] args) {
 "cactus" .chars ().parallel ().forEach (c -> System .out.println ((char)c));

A. No.}

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
