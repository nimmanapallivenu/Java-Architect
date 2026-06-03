# 17 Java Coding Tips for job interviews and pre interview coding tests

## Table of Contents

Java coding questions are very common in job interviews. Good coding skills are essentials for passing the peer code reviews with flying colors. Here are 17
coding tips with Java examples.
Tip #1 : If you are asked to write a function or code, make sure that your code fails fast . Check for pre-conditions and post conditions. For , example, if you are
writing a function to “apply given interest rate to given balance”
Pre-condition : Balance is a positive number and greater than zero. Interest rate is not a percentage and can’ t be > 1.
Post-condition : new balance is a positive number by applying the interest rate.
public BigDecimal updateBalance (BigDecimal balance , BigDecimal rate) {<br />
//pre-condition check
if(balance == null || rate == null) {
 throw new IllegalAr gumentException ("balance or interestRate cannot be null" );
}
if(!isGretaerThanZero (balance ))
{
 throw new IllegalAr gumentException ("balance must be a +ve number > zero " );
}
if(isInputPercentage (rate) || !isGretaerThanZero (rate)){
 throw new IllegalAr gumentException ("rate is not valid" );
}
BigDecimal newBalance = balance .multiply (BigDecimal .ONE .add(rate));
//postcondition checks -- wrong calculation
if(!isGretaerThanZero (balance )) {
 throw new CalculationException ("balance must be a +ve number > zero." );
}
if(newBalance .compareT o(balance ) <= 0){
 throw new CalculationException ("new balance cannot be less or same as old balance " );
}
return newBalance ;

Tip #2 : Don’ t write big functions , and divide them into smaller meaningful functions as shown above with isGreaterThanZero , isInputPercentage , etc.
 Favor meaningful function names as opposed to cluttering your code with comments. The function names must be self explanatory as to what you are doing.
Tip #3 : Write JUnit tests to test your function or code. T est for both the positive and negative scenarios. Y ou need junit-xxx.jar for the example below .boolean isGretaerThanZero (BigDecimal input ){
return input .compareT o(BigDecimal .ZERO ) > 0 ;
boolean isInputPercentage (BigDecimal input ){
return input .compareT o(BigDecimal .ONE ) > 0 ;
mport junit.framework .Assert ;
mport org.junit.Before ;
mport org.junit.Test;
public class BalanceCalcT est {
BalanceCalc calc;
@Before
public void init() {
calc = new BalanceCalc ();
@Test
public void testUpdateBalance () {
BigDecimal updatedBalance =
 calc.updateBalance (new BigDecimal ("100.00" ), (new BigDecimal ("0.10" )));
Assert .assertT rue(new BigDecimal ("110.0" ).compareT o(updatedBalance ) == 0);
@Test(expected = java.lang.IllegalAr gumentException .class )
public void negativeT estUpdateBalance () {

Tip #4 : If you ar e asked to write iterative version of function then you may be drilled to write r ecursive version or vice-versa . When
writing loops, don’ t use i or j as index variables. Use meaningful names. Also avoid nested loops where possible.
Reverse a given string input : iteration
Reverse a given string input : recursionBigDecimal updatedBalance =
 calc.updateBalance (new BigDecimal ("-100.00" ), (new BigDecimal ("0.10" )));
public static String reverse (String input ) {
 StringBuilder strBuilder = new StringBuilder ();
 char[] strChars = input .toCharArray ();
 for (int charIndex = strChars .length - 1; charIndex >= 0; charIndex --) {
 strBuilder .append (strChars [charIndex ]);
 }
 return strBuilder .toString ();

Note : You may be quizzed on tail recursion. Recursion Vs T ail Recursion
Tip #5 : Familiarize yourself with order of complexity with the Big O notation to answer any follow up question like — What is the order of
complexity of the above function you wrote?
Tip #6 : Code to interface . Initialize your collection with appropriate capacity . Don’ t return a null collection, instead return an empty collection like Collections. emptyList( ) ;.
Don’t do :
Do: public static String reverseRecursion (String input ) {
 //exit condition. otherwise endless loop
 if (input .length () < 2) {
 return input ;
 }
 return reverseRecursion (input .substring (1)) + input .charAt (0);
 HashMap <String , String > keyV alues = new HashMap ()<String , String >();
 ArrayList <String > myList = new ArrayList <String >();
 Map<String , String > keyV alues = new HashMap ()<String , String >(20);
 List<String > myList = new ArrayList <String >(100);

Map and List are interfaces.
Tip #7 : Favor composition over interface . 
Tip #8 : Use generics wher e requir ed. Recursion with Generics know how . Understanding generics with scenarios .
Tip #9 : Reduce the accessibility , and make your objects immutable wher e appr opriate . For example, a custom key class that is used to store objects in a map.
How to make a Java class immutable? .
Tip #10: D on’t forget to override Object class’ s methods that ar e meant to be overridden like equals(…), hashCode( ) , and toString( ) .
Tip #1 1: When you have large if/else or switch statements , see if the Open/Closed design principle can be applied.
Tip #12: Don’t assume that your objects and classes ar e going to be accessed by a single-thr ead.
Safeguard your objects and classes against multi-threading. Java multi-threading questions and answers .
Tip #13: Use the right data types. For example, use either BigDecimal to represent monetary values in dollars and cents or long to represent
them in their lowest units as just cents (400 cents for $4.0). Beware of max and minimum values for data types. If you want values lar ger than long, use
BigInteger , which has no theoretical limit. The BigInteger class allocates as much memory as it needs to hold all the bits of data it is asked to hold
byte (-128 to 127) –> short (2 bytes, -32,768 to 32,767) –> char (2 bytes, 65,535) –> int (4 bytes, -2^31 to 2^31-1) –> long(8 bytes, -2^63 to 2^63-1) –> float
(4 bytes) –> double (8 bytes). boolean (true or false)
Also, never compare floating point variables to compare exact amounts like

Tip #14: Handle and pr opagate exceptions pr operly . Have a consistent usage of checked and unchecked exceptions.
Exception is swept under the carpet.
CustomCheckedException is never reached as exceptions are polymorphic in nature. CustomCheckedException must be declared first above Exception.double amount = 12.05 ;
f(amount == 12.05 ) { //don't
 //...do something
}
ry {
 //do something
}
catch (Exception ex){
 //do nothing
}
ry {
 //do something
catch (Exception ex){
 //do something

Exception handling in Java questions and answers .
Tip #15 : Write pseudo code for more dif ficult coding questions. At times drawing diagrams can help you solve the problem.
Tip #16 : Keep in mind some of the design principles like SOLID design principles, Don’ t Repeat Y ourself ( DRY), and Keep It Simple ans Stupid
(KISS ). Also, think about the OO concepts — A PIE . Abstraction, Polymorphism, Inheritance, and Encapsulation. These principles and concepts are all about
accomplishing
Low coupling : Coupling in software development is defined as the degree to which a module, class, or other construct, is tied directly to others. Y ou
can reduce coupling by defining intermediate components (e.g. a factory class or an IoC container like Spring) and interfaces between two pieces of a
system. Favoring composition over inheritance can also reduce coupling. 
High cohesion : Cohesion is the extent to which two or more parts of a system are related and how they work together to create something more
valuable than the individual parts. Y ou don’ t want a single class to perform all the functions (or concerns ) like being a domain object, data access object,
validator , and a service class with business logic. T o create a more cohesive system from the higher and lower level perspectives, you need to break out
the various needs into separate classes. 
How will you go about writing loosely coupled Java applications?
Tip #17 : If you ar e coding in fr ont of the interviewer , think out loud . It is not always possible to solve problems in an interview when you are
nervous, but how you will go about solving a coding problem is equally important even if you don’ t solve it. So, let the interviewer know that you know how to
approach a problem. Solve the problem first, and then find ways to optimize it in terms of performance and memory allocation.
Practice!, practice!!, practice!!!….W rite code!!, more code!!!, more code!!!!. That is the only way . Any mor e tips or corr ections folks?catch (CustomCheckedException ex){
 //do nothing



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
