# Generating random numbers in Java   Java Success.com

## Table of Contents

- [Q1: Can you write Java code to generate random numbers between a given range?](#q1)
- [Q2: Can you write Java code to generate random numbers between 100 and 200 using the](#q2)
- [Q3: What are the practical use-cases of generating random numbers?](#q3)
- [Q4: How will you go about generating a random UUID in Java](#q4)

---

## Q1: Can you write Java code to generate random numbers between a given range?

**Answer:**

E.g. 0 and 9 or 5 to 35, and so on.
The “ nextInt() ” method works from “ 0” onwards. So, “rand.nextInt(9)” will return 9 numbers between 0 and 8 inclusive. T o start with 1, add 1 to the result. i.e.
rand.nextInt(9) + 1 to go from 1 to 9.
If the range starts from a higher number than one you will need to like from 5 to 35,
1) minus the starting number from the upper limit number and then add one.
2) add the starting number to the result of the nextInt() method.
For e.g. T o pick numbers between 5 and 35, The upper limit will be 5 + rand.nextInt((35 – 5) + 1) as “rand.nextInt(31)” returns numbers from 0 to 30. Hence
we can deduce a formula as shown below with
Here is the working code that takes min and max values to generate random numbers between MIN and MAX inclusive.Random rand = new Random ();
nt randomNum = min + rand.nextInt ((max - min) + 1);
mport java.util.Random ;

Run it a few times and see if the randomly generated numbers are within a range.

---

## Q2: Can you write Java code to generate random numbers between 100 and 200 using the method randomMinT oMax(0, 9) shown above?

**Answer:**

MIN = 100 + 0*0 + 0 + 0 = 100
MAX = 100 + 9*10 + 9 + 1 = 200public class RandomGen {
 
 public static void main (String [] args) {
 int n1 = randomMinT oMax (0, 9); // 0 to 9 inclusive
 int n2 = randomMinT oMax (5, 35); // 5 to 35 inclusive
 
 System .out.println (n1);
 System .out.println (n2);
 }
 
 private static int randomMinT oMax (int min, int max ) {
 Random rand = new Random ();
 int randomNum = min + rand.nextInt ((max - min) + 1);
 return randomNum ;
 }
mport java.util.Random ;
public class RandomGen {
 
 public static void main (String [] args) {
 
 int n1 = randomMinT oMax (0, 9);
 int n2 = randomMinT oMax (0, 9);
 int n3 = randomMinT oMax (0, 9);
 
 int result = 100 + n1*10 + n2 + n3%2;

---

## Q3: What are the practical use-cases of generating random numbers?

**Answer:**

1. Used in cryptography for generating keys, salts, passwords, etc.
2. Used in gaming and gambling.
3. In simulating and modelling complex scenarios, and for selecting samples from lar ge data-sets.

---

## Q4: How will you go about generating a random UUID in Java

**Answer:**

UUID stands for Universally Unique ID. A UUID is known in the Microsoft world as as a GUID. Generating a UUID (aka GUID) in Java . System .out.println (result );
 }
 
 private static int randomMinT oMax (int min, int max ) {
 Random rand = new Random ();
 int randomNum = min + rand.nextInt ((max - min) + 1);
 return randomNum ;
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
