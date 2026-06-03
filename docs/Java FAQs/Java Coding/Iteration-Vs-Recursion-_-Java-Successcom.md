# Iteration Vs Recursion   Java Success.com

## Table of Contents

- [Q1: Can you write a sample code that will count the number of “A”s in a given text “](#q1)

---

## Q1: Can you write a sample code that will count the number of “A”s in a given text “ AAA rating “? Show both iterative and recursive approaches?

**Answer:**

Iteration:
Recursion:public class Iteration {
 public int countA (String input ) {
 if (input == null || input .length ( ) == 0) {
 return 0;
 }
 int count = 0;
 for (int i = 0; i < input .length ( ); i++) {
 if(input .substring (i, i+1).equals ("A")){
 count ++;
 }
 }
 return count ;
 }
 public static void main (String [ ] args) {
 System .out.println (new Iteration ( ).countA ("AAA rating" )); // Ans.3
 }
 
public class RecursiveCall {

A re-entrant method would be one that can safely be entered, even when the same method is being executed, further down the call stack of the same thread. A
function is recursive if it calls itself . Given enough stack space, recursive method calls are perfectly valid in Java though it is tough to debug. Recursive
functions are useful in removing iterations from many sorts of algorithms. All recursive functions are re-entrant, but not all re-entrant functions are recursive.
Stack uses LIFO (Last In First Out), so it remembers its ‘caller ’ and knows whom to return when the function has to return. Recursion makes use of system
stack for storing the return addresses of the function calls. Java is a stack based language .
Many find it harder to understand recursion, hence let’ s try it with a simple diagram. public int countA (String input ) {
 
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
 System .out.println (new RecursiveCall ( ).countA ("AAA rating" )); // Ans. 3
 }

Recursion

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
