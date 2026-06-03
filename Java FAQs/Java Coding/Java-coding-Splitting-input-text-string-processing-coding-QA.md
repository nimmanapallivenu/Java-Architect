# Java coding   Splitting input text & string processing coding Q&A

## Table of Contents

- [Q2: Given a string, return a version without the first 2 chars. Except keep the firs](#q2)
- [Q3: Given a string and allowed repetition number , remove characters consecutively r](#q3)

---

## Q2: Given a string, return a version without the first 2 chars. Except keep the first char if it is ‘a’ and keep the second char if it is ‘b’.
For example:

**Answer:**

Solution: } finally {
 total += nextNumber ;
 }
 }
 return total;
 }
15
removeCharacters ("Absolute" ) --> absolute
removeCharacters ("base" ) --> se 
removeCharacters ("Ant" ) --> at
removeCharacters ("ubiquitous" ) --> biquitous

---

## Q3: Given a string and allowed repetition number , remove characters consecutively repeated more than the repetition number?
For example:package algorithms ;
public class RemoveCharactersFromStringInput {
 public static void main (String [] args) {
 System .out.println (removeCharacters ("Absolute" ));
 System .out.println (removeCharacters ("base" ));
 System .out.println (removeCharacters ("Ant" ));
 System .out.println (removeCharacters ("ubiquitous" ));
 }
 public static String removeCharacters (String input ) {
 input = input .toLowerCase ();
 String firstChar = (input .length () > 0 && input.charAt(0) == 'a') ? "a" : "";
 String secondChar = (input .length () > 1 && input.charAt(1) == 'b') ? "b" : "";
 String remaing = input .length () > 2 ? input .substring (2) : "";
 return firstChar + secondChar + remaing ;
 }
removeConsecutiveCharacters ("aaab" , 2) --> "aab"
removeConsecutiveCharacters ("aaaabbbb" , 3) --> "aaabbb" 
removeConsecutiveCharacters ("aabbbcccc" , 1) --> "abc"

**Answer:**

1. Convert the input string to a char array
2. Loop through each character , and if the current character == previous character , and within the repetition count, do nothing. If outside the repetition count,
replace the character with a space character (i.e. ‘ ‘). You could use some other characters unlikely to be used in the input text instead of a space character .
3. After looping, find and replace all the space (i.e. ‘ ‘) characters with no-space (i.e. ”).
Solution
package algorithms ;
public class RemoveCharactersFromStringInput {
 
 private static final char MARKER_FOR_REMOV AL = ' ';
 public static void main (String [] args) {
 System .out.println (removeConsecutiveCharacters ("aaab" , 2));
 System .out.println (removeConsecutiveCharacters ("aaaabbbb" , 3));
 System .out.println (removeConsecutiveCharacters ("aabbbcccc" , 1));
 System .out.println (removeConsecutiveCharacters ("aabbabcccc" , 2));
 }
 public static String removeConsecutiveCharacters (String input , int allowedRepetitionCount ) {
 char[] charArray = input .toCharArray ();
 char lastChar = charArray [0]; //first character
 int count = 1; //start with 1 as first char is already read
 
 //loop from 2nd character
 for (int i = 1; i < charArray .length ; i++) {
 //Oops bad "MARKER_FOR_REMOV AL" selection
 if(charArray [i] == MARKER_FOR_REMOV AL){
 throw new IllegalAr gumentException ("Place holder character =" + MARKER_FOR_REMOV AL + " is found in input." );
 }
 
 if(charArray [i] == lastChar ){
 ++count ;

Output: lastChar = charArray [i];
 if(count > allowedRepetitionCount ) {
 charArray [i] = MARKER_FOR_REMOV AL; //mark it for removal
 }
 } else {
 count = 1; //reset count 1 to include current char read
 lastChar = charArray [i];
 }
 }
 
 String output = new String (charArray );
 String result = output .replace (MARKER_FOR_REMOV AL, '\u0000' ); // '\u0000' means empty character
 //remove marked place holders
 
 return result ;
 
 }
aab
aaabbb
abc
aabbabcc

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
