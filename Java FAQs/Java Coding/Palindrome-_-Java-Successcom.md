# Palindrome   Java Success.com

## Table of Contents

- [Q1: Can you write a function to determine if a given string input is a palindrome?
A](#q1)
- [Q2: How will you find the longest palindrome of a given string?](#q2)

---

## Q1: Can you write a function to determine if a given string input is a palindrome?
A palindrome is a word or sentence that reads the same forward as it does backward. For example, the terms “racecar”, “dad”, “madam” and the name
“Hannah”. The longest palindromic substring of “bananas” is “anana” — the left side of a palindrome is a mirror image of its right side.

**Answer:**

As per the diagram,
Palindrome
to be a palindrome
ndex [0] == index [4];
ndex [1] == index [3];
ndex [2] == index [2];
/loop from left to right

Alternatively , the reversed string must match the original string.

---

## Q2: How will you find the longest palindrome of a given string?

**Answer:**

Algorithm: Have 2 center pointers, and move one to the left and the other to the right. Cater for 2 scenarios where you have odd number of characters and
even number of characters as shown below .for (int i = 0; i < s.length () - 1; i++) {
if (s.charAt (i) != s.charAt (s.length () - 1 - i)) {
 return false ;
}
}
return true;
String reverse = "";
/loop from right to left
for ( int i = length - 1 ; i >= 0 ; i-- ) {
 reverse = reverse + s.charAt (i);
f (s.equals (reverse )) {
 return true;
else {
 return false ;

Longest Palindrome
public class PalindromeLongestSubstring {
private static String findLongestPalindrome (String s){
 if(s == null || s.length () == 1){
 return s;
 }
 String longest = s.substring (0,1);
 for (int i = 0; i < s.length (); i++) {
 //one center . odd number of characters (e.g 12321)
 String result = findPalindromeForGivenCenter (s, i, i);
 longest = result .length () > longest .length ()? result : longest ;
 //two centers. even number of characters (e.g 123321)
 result = findPalindromeForGivenCenter (s, i, i+1);

 longest = result .length () > longest .length ()? result : longest ;
 }
 return longest ;
}
// Given either same left and right center (e.g.12321) or
// 2 left and right centers (e.g. 123321) find the longest palindrome
private static String findPalindromeForGivenCenter (final String s, int leftCenter , int rightCenter ) {
 int length = s.length ();
 while (leftCenter >= 0 && rightCenter <= length -1 && s.charAt(leftCenter) == s.charAt(rightCenter)) {
 leftCenter --; //move from center to left
 rightCenter ++; //move from center to right
 }
 //leftCenter+1 because the index would have moved left before exiting the above loop
 return s.substring (leftCenter +1,rightCenter );

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
