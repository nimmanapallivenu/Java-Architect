# Java coding practice problems    prime and powerful

## Table of Contents

- [Q1: Can you write code to check if a given number is prime?](#q1)
- [Q2: Can you write a method that gives a list of prime numbers within a given range?](#q2)
- [Q3: Can you list “powerful numbers” between a given range, where the definition of a](#q3)

---

## Q1: Can you write code to check if a given number is prime?

**Answer:**

A prime number is a number that is divisible only by itself and of course 1.
More facts about prime numbers
1 is not a prime number . Prime numbers start from 2
2 is the first and only even prime number
all the other prime numbers apart from 2 are odd numbers starting from 3. E.g. 3, 5, 7
A naive solution
public static boolean isPrimeNaive (int number ) {
 if (number % 2 == 0 || number == 1) {
 return (number == 2);
 }
 
 //if the number is divisible by other than itself (i.e number)
 //it is not a prime 
 for (int i = 3; i < number ; i++) {
 if(number % i == 0){
 return false ;
 }
 }
 
 return true; 

Q. What is wrong with the above solution?
A. It is not optimal. if the number is 97, you will end up dividing 97 by from 3 to 96. Goes through the loop 93 times.
A better solution
Two improvements can be made
1) Instead of i++, do i+2, to check for only odd numbers as 2 is the only even prime number . All the others are odd
2) Instead of i < number , do i*i < number because if you look at factors of 99, the factors are repeated half way mark
You can see repeated factors –> 9 * 1 1 and 1 1 * 9, 3 * 33 and 33 * 3, etc. So, the revised solution will take advantage of this finding. So, for 97, it will only loop
through 4 times for numbers 3, 5, 7, 9. When it gets to 1 1, it exits the loop as 1 1 * 1 1 = 121 which is > 97.99 = 1 * 99 == 3 * 33 == 9 * 11 == 11 * 9 == 33 * 3 == 99 * 1
public static boolean isPrime (int number ) {
 // even numbers stop here.
 // Only 2 is an even prime number & the first prime number
 // 1 is not a prime number
 if (number % 2 == 0 || number == 1) {
 return (number == 2);
 }
 // odd numbers from 3 to n get here
 // goes inside a loop only if i*i <= number
 // so, numbers 3, 5, 7 skip this for loop
 // 9, 1 1, 13, 15, etc get in as 3*3 = 9, 3*3 < 1 1, and so on
 for (int i = 3; i * i <= number ; i += 2) {
 // divisible by other than itself
 if (number % i == 0){
 return false ;

Another more ef fective approach for isPrime(int number) is:

---

## Q2: Can you write a method that gives a list of prime numbers within a given range?

**Answer:**

The range will be supplied via “from” and “to”. }
 }
 // if gets here, it is a prime
 return true;
private static boolean isPrime (int number ) {
 // even numbers stop here.
 // Only 2 is an even prime number & the first prime number
 // 1 is not a prime number
 if (number % 2 == 0 || number == 1) {
 return (number == 2);
 }
 int i = 2;
 while (i < Math .sqrt(number )) {
 if (number % i == 0) {
 return false ;
 }
 i++;
 }
 // if gets here, it is a prime
 return true;

mport java.util.List;
public class PrimeNumber {
 public static void main (String [] args) {
 System .out.println ("Prime numbers=" + getPrimeNumbers (1, 200));
 }
 public static List<Integer > getPrimeNumbers (int from , int to) {
 List<Integer > primeNumbers = new ArrayList <Integer >();
 for (int number = from ; number <= to; number ++) {
 if(isPrime (number )){
 primeNumbers .add(number );
 }
 }
 return primeNumbers ;
 }
 private static boolean isPrime (int number ) {
 // even numbers stop here.
 // Only 2 is an even prime number & the first prime number
 // 1 is not a prime number
 if (number % 2 == 0 || number == 1) {
 return (number == 2);
 }
 int count = 0;
 // odd numbers from 3 to n get here
 // goes inside a loop only if i*i <= number
 // so, numbers 3, 5, 7 skip this for loop
 // 9, 1 1, 13, 15, etc get in as 3*3 = 9, 3*3 < 1 1, and so on
 for (int i = 3; i * i <= number ; i += 2) {
 System .out.println ("i=" + i);
 count ++;
 // divisible by other than itself
 if (number % i == 0){
 return false ;
 }
 }
 
 System .out.println ("count=" + count );
 // if gets here, it is a prime

Output:

---

## Q3: Can you list “powerful numbers” between a given range, where the definition of a powerful number is — A positive integer m which is for every ” p” that
divides “m”, “p*p” must also divide m, where “p” is a prime number
The powerful numbers fr om 1 to 40 ar e: 1, 4, 8, 9, 16, 25, 27, 32, 36, …
12 is not a powerful number because: 3 divides 12 but 3*3=9 does not.
18 is not a powerful number because: 2 divides 18, but 2*2=4 does not.
1 is a powerful number because: neither p nor p*p divides it.

**Answer:**

return true;
 } 
Prime numbers =[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89,
97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199]
package algorithms ;
mport java.util.ArrayList ;
mport java.util.List;
public class PowerfulNumber {
 public static void main (String [] args) {
 System .out.println ("powerfulNums= " + getPowerfulNumbers (1, 40));
 }

 public static List<Integer > getPowerfulNumbers (int from , int to) {
 List<Integer > powerfulNums = new ArrayList <Integer >();
 List<Integer > primeNumbers = getPrimeNumbers (from , to);
 for (int m = from ; m <= to; m++) {
 boolean isPowerfulNumber = true;
 for (Integer p : primeNumbers ) {
 // every p that divides m, p*p must also divide m.
 if(m % p == 0 && m % (p*p) != 0){
 isPowerfulNumber = false;
 break ;
 }
 }
 
 if(isPowerfulNumber ){
 powerfulNums .add(m);
 }
 }
 return powerfulNums ;
 }
 private static List<Integer > getPrimeNumbers (int from , int to) {
 List<Integer > primeNumbers = new ArrayList <Integer >();
 for (int number = from ; number <= to; number ++) {
 if(isPrime (number )){
 primeNumbers .add(number );
 }
 }
 return primeNumbers ;
 }
 private static boolean isPrime (int number ) {
 if (number % 2 == 0 || number == 1) {
 return (number == 2);
 }
 
 for (int i = 3; i * i <= number ; i += 2) {
 // divisible by other than itself
 if (number % i == 0){
 return false ;
 }
 }

Output:
50+ Java coding practice pr oblems Links:
Can you write code in Java? | Designing your classes & interfaces in Java | Java Data Structures & Algorithms | Passing the unit tests | What is wrong with this
code? return true;
 }
 
powerfulNums = [1, 4, 8, 9, 16, 25, 27, 32, 36]

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
