# Finding the missing numbers Java example   Java Success.com

## Table of Contents

Q. Can you write code to identify missing numbers in a given array of numbers?
Solution 1: Assuming that the given numbers ar e in order
Finding the missing number
public class MissingNumber {

 public static void main (String [] args) {
 int numbers [] = { 2, 3, 4, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 18 };
 new MissingNumber ().findMissingNumbers (numbers , 0, 20);
 }
 private void findMissingNumbers (int[] input , int first, int last) {
 printMissingNumbersFront (input , first);
 printMissingNumbersInTheMiddle (input , last);
 printMissingNumbersBack (input , last);
 }
 /**
 * before 2
 * @param input
 * @param first
 */
 private void printMissingNumbersFront (int[] input , int first) {
 //0 and 1 are less than 2 as per the above input
 for (int i = first; i < input [0]; i++) {
 System .out.println (i);
 }
 }
 /**
 * after 18
 * @param input
 * @param last
 */
 private void printMissingNumbersBack (int[] input , int last) {
 // 1 + the last element value is 24. 24 to 30 are less than or equal to the last value of 30
 for (int i = 1 + input [input .length - 1]; i <= last; i++) {
 System .out.println (i);
 }
 }
 /**
 * between 2 and 18
 * @param input
 * @param last
 */
 private void printMissingNumbersInTheMiddle (int[] input , int last) {
 //for a given index i, index [i-1]+1 should be equal if present.

Output:
The above solution assumes that the numbers are in order (i.e. sorted). What if the numbers are random?
Solution 2: Numbers ar e added randomly . for (int i = 1; i < input .length ; i++) {
 //e.g. for input[1] = 3, j=1+2
 //input[2] = 4, j=1+3
 //input[3] = 6, j=4+1, Oops 5 is less than 6 so print
 //input[4] = 7, j=6+1
 //if index[i] < index[i-1] + 1, then number index[i-1] + 1 is missing
 for (int j = 1+input [i-1]; j < input [i]; j++) {
 System .out.println (j);
 }
 }
 }
0
1
5
16
17
19
20
mport java.util.Set;
mport java.util.concurrent .CopyOnW riteArraySet ;

Output:public class MissingNumber {
 public static void main (String [] args) {
 int numbers [] = { 7, 3, 6, 4, 2, 8, 9, 10, 12, 14, 13, 11, 15, 18 };
 new MissingNumber ().findMissingNumbers (numbers , 0, 20);
 }
 private void findMissingNumbers (int[] input , int first, int last) {
 Set<Integer > expectedNumbers = new CopyOnW riteArraySet <Integer >();
 for (int i = first; i <= last; i++) {
 expectedNumbers .add(i);
 }
 
 //remove numbers from the above set that are in the input.
 for (int i = 0; i < input .length ; i++) {
 if(expectedNumbers .contains (input [i])) {
 expectedNumbers .remove (input [i]); //CopyOnW riteArraySet is used that
 //You won't get ConcurrentModificationException
 }
 }
 
 //left over numbers in the set are the missing numbers
 for (Integer number : expectedNumbers ){
 System .out.println (number );
 }
 }
0
1
5
16
17
19
20



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
