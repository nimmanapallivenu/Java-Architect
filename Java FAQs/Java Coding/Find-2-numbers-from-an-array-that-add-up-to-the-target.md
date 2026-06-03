# Find 2 numbers from an array that add up to the target

## Table of Contents

- [Q1: Given an array of integers, find two numbers such that they add up to a specific](#q1)

---

## Q1: Given an array of integers, find two numbers such that they add up to a specific tar get number?
For example,
Given numbers: {2, 3, 8, 7, 5}
Target number: 9
Result: 2 and 7

**Answer:**

Solution 1: Store processed numbers in a set.

Solution 2: Sort the numbers and use 2 pointers. If sum < tar get, move the pointer1 to the right. If sum > tar get, move the pointer2 to the left.mport java.util.HashSet ;
mport java.util.Iterator ;
mport java.util.Set;
mport javax .management .RuntimeErrorException ;
public class Sum2NumsT oATargetNumberT est {
public static int[] findNumsThatSumT oTargetNum (int[] numbers , int target) {
 Set<Integer > processedNumbers = new HashSet <>();
 boolean foundSum = false ;
 int[] result = new int[2];
 for (int i = 0; i < numbers .length ; i++) {
 int reqNumber = target - numbers [i];
 System .out.println ("current number=" + numbers [i] + " , requiredNumber = " + reqNumber );
 if(processedNumbers .contains (reqNumber )){
 result [0] = numbers [i];
 result [1] = reqNumber ;
 foundSum = true;
 break ;
 }
 else {
 processedNumbers .add(numbers [i]);
 }
}
if(!foundSum ){
 throw new RuntimeException ("Sum is not found !!" );
}
return result ;

mport java.util.Arrays ;
public class Sum2NumsT oATargetNumberT est2 {
 // System.out.println("current number=" + numbers[i] + " , requiredNumber = " + reqNumber);
 static int[] findNumsThatSumT oTargetNum (int[] numbers , int target) {
 int pointer1 = 0;
 int pointer2 = numbers .length -1;
 int[] result = new int[2];
 Arrays .sort(numbers ); // sort the numbers
 while (pointer2 >= pointer1 ) {
 int sum = numbers [pointer1 ] + numbers [pointer2 ];
 if(sum == target) {
 result [0] = numbers [pointer1 ] ;
 result [1] = numbers [pointer2 ] ;
 break ;
 }
 //if sum is greater than the tar get
 if(sum > target) {

 pointer2 --; //move pointer2 to the left to reduce the sum
 }
 //if sum is less than the tar get
 if(sum < target) {
 pointer1 ++; //move pointer1 to the right to increase the sum
 }
 }
 return result ;

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
