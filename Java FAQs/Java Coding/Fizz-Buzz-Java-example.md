# Fizz Buzz Java example

## Table of Contents

- [Q1: Write a program that prints numbers from 1 to 30, but for multiples of three pri](#q1)

---

## Q1: Write a program that prints numbers from 1 to 30, but for multiples of three print “Fizz” instead of the number and for the multiples of five print “Buzz”.
For numbers which are multiples of both three and five print “FizzBuzz”?

**Answer:**

This is a very basic coding question, but still some struggle or take too long (more than 5 minutes) to solve this coding problem. Be prepared for the
interviewer to quiz you on dif ferent approaches to solve this coding problem. Be prepared to discuss DRY principle and optimization techniques to compare
only once, etc.
There could be other variations. Arbitrary sets of number/word combinations (3 = Foo, 5=Bar , 7=Bazz, 1 1=Banana), etc.
One of the Java operators is “ modulus “, the name most programmers use for the remainder operator . It uses the symbol “%”. Basically , “A % B” represents the
remainder left over after dividing A by B. Y ou use this operator to determine if a number is odd (i%2 != 0) or even (i%2 == 0).
Approach 1:
public class FizzBuzz {
 public static void main (String [] args) {
 StringBuilder builder = new StringBuilder (500);
 for (int i = 1; i <= 30; i++) {
 if (i % 15 == 0) {
 builder .append ("FizzBuzz" );
 }
 else if (i % 3 == 0) {
 builder .append ("Fizz" );
 }
 else if (i % 5 == 0){
 builder .append ("Buzz" );
 }
 else {
 builder .append (i);
 }
 builder .append ('\n');
 }
 
 System .out.println (builder );
 }

Approach 2:
Don’ t Repeat Y ourself ( DRY) approach with use of “Fizz” and “Buzz” not repeated. The boolean flag “processed” is used to determine if a number was
multiples of 3, 5 or both.
Approach 3:
Same as DRY approach shown above, but instead of using a boolean flag to determine if multiples of 3,5, or both, the length of the builder is compared before
and after to see if already processedpublic class FizzBuzzDry {
 public static void main (String [] args) {
 StringBuilder builder = new StringBuilder (500);
 for (int i = 1; i <= 30; i++) {
 boolean processed = false ;
 if (i % 3 == 0) {
 builder .append ("Fizz" );
 processed = true;
 }
 if (i % 5 == 0) {
 builder .append ("Buzz" );
 processed = true;
 }
 if (!processed ) builder .append (i);
 builder .append ('\n');
 }
 
 System .out.println (builder );
 }

I am sure there are many more approaches possible. For example, using a conditional operator , etc.public class FizzBuzzDry2 {
public static void main (String [] args) {
StringBuilder builder = new StringBuilder (500);
for (int i = 1; i <= 30; i++) {
 int length = builder .length ();
 if (i % 3 == 0){
 builder .append ("Fizz" );
 }
 if (i % 5 == 0){
 builder .append ("Buzz" );
 }
 if (length == builder .length ()){
 builder .append (i);
 }
 
 builder .append ('\n');
}
System .out.println (builder );
builder .append (
 i % 15 == 0
 ? "FizzBuzz"
 : (i % 3 == 0
 ? "Fizz"
 : (i % 5 == 0
 ? "Buzz"
 : i)));
 builder .append ("\n");

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
