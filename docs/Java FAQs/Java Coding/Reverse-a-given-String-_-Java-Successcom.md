# Reverse a given String   Java Success.com

## Table of Contents

- [Q1: Can you write a method that reverses a given String?](#q1)

---

## Q1: Can you write a method that reverses a given String?

**Answer:**

Can be done a number of dif ferent ways.
Best Practice: Using the Proven Java API
It is always a best practice to reuse the API methods as shown above with the StringBuilder(input).reverse( ) method as it is fast, ef ficient (uses bitwise
operations) and knows how to handle Unicode surrogate pairs, which most other solutions ignore. The above code handles null and empty strings, and a
StringBuilder is used as opposed to a thread-safe String Buffer, as the StringBuilder is locally defined, and local variables are implicitly thread-safe.
Recursion:
Some interviewers might probe you to write other lesser elegant code using either recursion or iterative swapping. Some developers find it very dif ficult to
handle recursion, especially to work out the termination condition. All recursive methods need to have a condition to terminate the recursion.public class ReverseString {
 public static void main (String [ ] args) {
 System .out.println (reverse ("big brown fox" ));
 System .out.println (reverse ("")); 
 }
 public static String reverse (String input ) {
 if(input == null || input .length ( ) == 0){
 return input ;
 }
 
 return new StringBuilder (input ).reverse ( ).toString ( );
 }

Iteration:
Iteratively swapping the values.public class ReverseString {
 
 public String reverse (String str) {
 // exit or termination condition
 if ((null == str) || (str.length ( ) <= 1)) {
 return str;
 }
 
 // put the first character (i.e. charAt(0)) to the end. String indices are 0 based.
 // and recurse with 2nd character (i.e. substring(1)) onwards 
 return reverse (str.substring (1)) + str.charAt (0);
 }
public class ReverseString3 {
 
 public String reverse (String str) {
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
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
