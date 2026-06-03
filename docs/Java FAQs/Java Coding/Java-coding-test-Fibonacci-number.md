# Java coding test   Fibonacci number

## Table of Contents

- [Q1: Can you write a function to determine the nth Fibonacci number?
The Fibonacci nu](#q1)

---

## Q1: Can you write a function to determine the nth Fibonacci number?
The Fibonacci numbers under 2000 are : 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597.
Where the zeroth number being 0, first number being 1, second number being 1 (i.e. 0+1), third number being 2 (i.e. 1+1), fourth number being 3 (i.e. 1+2) and
so on till the 17th number being 1597 (i.e 610 + 987).

**Answer:**

Iteration

Fibonacci series
public class IterativeFibonacci {
public int fibonacci (int n) {
 if (n < 0) {

Recursion throw new IllegalAr gumentException ("Input parameter is invalid " + n);
 }
 int num1 = 0, num2 = 1;
 //zeroth fibonacci number is 0
 if (n == 0) {
 return 0;
 }
 //first and second fibonacci numbers are 1 and 1
 if (n == 1 || n == 2) {
 return 1;
 }
 
 int current = num1 + num2 ;
 //compute from the third number onwards by adding the previous fibonacci number
 for (int i = 3; i <= n; i++) {
 num1 = num2 ; 
 num2 = current ; 
 current = num1 + num2 ;
 }
 return current ;
public static void main (String [] args) {
 int nThfibonacciNo = new IterativeFibonacci ().fibonacci (5); //Ans 5
 System .out.println (nThfibonacciNo );

The iterative solution has O(N) complexity . So, if you are after the 17th fibonacci number , then N is 17.
The recursive solution is O(1.6^N) . So, if N is 17, then O(1.6^17) will recurs 2952 times. So, recursion is not ef ficient at all, and shown for demonstration
only.public class RecursiveFibonacci {
public int fibonacci (int n){
 if(n<0){
 throw new IllegalAr gumentException ("Input parameter is invalid " + n);
 }
 if(n == 0){
 return 0;
 }
 else if(n <= 2){
 return 1;
 }
 else {
 return fibonacci (n-1)+fibonacci (n-2); // head recursion
 }
}
public static void main (String [] args) {
 int nThfibonacciNo = new RecursiveFibonacci ().fibonacci (5); //Ans 5
 System .out.println (nThfibonacciNo );
}

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
