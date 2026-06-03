# Finding the perfect number in Java

## Table of Contents

Q. Can you write code to output the perfect number between a given range?
Definition: A perfect number is a positive integer that is equal to the sum of its proper divisors. The smallest perfect number is 6, which is the sum of 1, 2, and
3. Other perfect numbers are 28, 496, and 8,128.
The function should take from and to as a range.
A The key steps are
1. Determine the devisors for a given number i
2. Sum the divisors
3. If a number i is greater than 0 and the sum of the divisors equals the number i then output it as a perfect number .
Solution
package algorithms ;
mport java.util.ArrayList ;
mport java.util.HashSet ;
mport java.util.List;
mport java.util.Set;
public class PerfectNumber {
 public static void main (String [] args) {
 List<Integer > perfectNumbers = getPerfectNumbers (0, 10000 );
 System .out.println (perfectNumbers );
 }
 public static List<Integer > getPerfectNumbers (int from , int to) {
 List<Integer > list = new ArrayList <Integer >();
 for (int i = from ; i <= to; i++) {
 Set<Integer > divisors = new HashSet <Integer >();
 //identify the divisors

Output: for (int j = 1; j < i; j++) {
 if (i % j == 0 && i != j) {
 divisors.add(j);
 }
 }
 //compute total of the divisors
 int total = 0;
 for (Integer integer : divisors ) {
 total += integer ;
 }
 // if i > 0 as 0 is not a perfect number &
 // total of the divisors = i then it is a perfect number
 if (total == i && i > 0) {
 list.add(i);
 }
 }
 return list;
 }
6, 28, 496, 8128 ]



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
