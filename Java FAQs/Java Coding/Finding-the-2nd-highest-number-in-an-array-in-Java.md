# Finding the 2nd highest number in an array in Java

## Table of Contents

Requirements gathering
1. Does the array allow duplicates?
2. If duplicates are allowed, then do you need to report duplicates? For example, in {2,4, 6, 3, 6, 5}, is 6 or 5 the second highest?
Analysis
If duplicates are not allowed, sort the array (Arrays.sort(…)) and get the second last element, which executes in O(nlogn)
If duplicates are allowed, loop through each element have two variables to store highest & second highest values. which executes in O(n)
Solution: duplicates are allowed
public class SecondHighest {
 public static void main (String [] args) {
 
 int[] numbers = {2,4, 6, 3, 6, 5};
 
 int highest = Integer .MIN_V ALUE + 1;
 int sec_highest = Integer .MIN_V ALUE ;
 for (int i : numbers ) // b is array of integers
 {
 if (i > highest ) // new highest found?
 {
 // highest becomes "second highest"
 sec_highest = highest ; // make current highest to second highest
 highest = i; // make current value to highest
 }
 // "i != highest "is to ensure duplicates are not reported as
 // highest & "second highest"
 else if (i > sec_highest && i != highest) // new "second highest" found? 
 {
 sec_highest = i;
 }

Output :
sec_highest=5
Java 8 way }
 
 System .out.println ("sec_highest=" + sec_highest ); 
 }
package com.java8 .examples ;
mport java.util.Arrays ;
mport java.util.List;
mport java.util.stream .Collectors ;
public class SecondHighest {
 public static void main (String [] args) {
 
 List<Integer > numbers = Arrays .asList (2,4, 6, 3, 6, 5);
 
 List<Integer > sortedUniqueNumbers = numbers .stream ()
 .distinct () // remove duplicates
 .sorted () // sort
 .collect (Collectors .toList ()); // convert stream to list
 
 System .out.println ("sec_highest=" +
 sortedUniqueNumbers .get(sortedUniqueNumbers .size()-2)); 
 }

Output :
sec_highest=5
So how about parallelizing the code?
In Java SE 8 it’ s easy: just replace stream() with parallelStr eam() .
package com.java8 .examples ;
mport java.util.Arrays ;
mport java.util.List;
mport java.util.stream .Collectors ;
public class SecondHighest {
 public static void main (String [] args) {
 
 List<Integer > numbers = Arrays .asList (2,4, 6, 3, 6, 5);
 
 List<Integer > sortedUniqueNumbers = numbers .parallelStream ()
 .distinct () // remove duplicates
 .sorted () // sort
 .collect (Collectors .toList ()); // convert stream to list
 
 System .out.println ("sec_highest=" +
 sortedUniqueNumbers .get(sortedUniqueNumbers .size()-2)); 
 }



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
