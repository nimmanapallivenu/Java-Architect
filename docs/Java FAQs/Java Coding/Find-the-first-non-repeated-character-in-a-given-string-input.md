# Find the first non repeated character in a given string input

## Table of Contents

Pseudocode
1) Precodnition check for null or empty input.
2) Loop throught the input string, and store each “character” as a key in a map with the value being the “character count”.
For example, an input string of “stress” would have
Key -> Value 
s -> 3
t -> 1
r -> 1
e -> 1
3) Loop through the input string again, and check the character count in the map previously populated, and if the count is 1, then return the character .
Questions to ask?
Experienced Java developers ask questions.
1. Is it case sensitive? Should “Stress” and “stress” to be treated the same way?
2. Throw error or return null when a given string is empty or null?
3. Should this be a good candidate for Java 8 functional pr ogramming ?
4. Should the map maintain the order of character keys? if yes, use a LinkedHashMap .
The following implementation assumes “NOT CASE SENSITIVE”
Java code
mport java.util.HashMap ;
mport java.util.Map;
public class FirstNonRepeatedCharacter {

Points to note :
1) precondition check is made for null or empty string input
2) mapChars is coded to interface and uses Java 8 style genericspublic static void main (String [] args) {
 System .out.println (logic ("stress" )); //'t' is first non repeated
 System .out.println (logic ("tweet" )); //'w' is first non repeated
}
private static Character logic (String input ) {
 
 if(input == null || input .trim().length () == 0){ //pre-condition check
 return null;
 }
 
 input = input .toLowerCase (); // Case insensitive
 char[] characters = input .toCharArray (); // convert to character array
 
 Map<Character ,Integer > mapChars = new HashMap <>(20); // Java 8, generics.
 
 //store characters and count
 for (char c : characters ) {
 if(mapChars .containsKey (c)) { //already exist
 mapChars .put(c, mapChars .get(c) + 1); // autoboxing & unboxing takes place
 }
 else { // not already exist
 mapChars .put(c, 1); 
 }
 }
 
 //find first char in the char array with count=1
 for (char c : characters ) {
 if(mapChars .get(c) == 1) { //auto unboxing takes place
 return c;
 }
 }
 
 return null; 
}

3) Autoboxing and unboxing takes place from “char to Character” and “int to Integer”.
Get user input interactively
Just the main method has changed. The logic is still the same.
mport java.util.HashMap ;
mport java.util.Map;
mport java.util.Scanner ;
public class FirstNonRepeatedCharacter {
public static void main (String [] args) {
 System .out.println ("Please enter the input string :" );
 Scanner in = new Scanner (System .in); //read from System input
 String input = in.nextLine ();
 char firstNonRepeatedChar = logic (input );
 System .out.println ("The first non repeated character is: " + firstNonRepeatedChar );
}
private static Character logic (String input ) {
 
 if(input == null || input .trim().length () == 0){
 return null;
 }
 
 input = input .toLowerCase (); // Case insensitive
 char[] characters = input .toCharArray (); // convert to character array
 
 Map<Character ,Integer > mapChars = new HashMap (); // Java 8, generics.
 
 //store characters and count
 for (char c : characters ) {
 if(mapChars .containsKey (c)) { //already exist
 mapChars .put(c, mapChars .get(c) + 1); // autoboxing & unboxing takes place
 }
 else { // not already exist

Output
LinkedHashmap to maintain the order of keys mapChars .put(c, 1); 
 }
 }
 
 //find first char in the char array with count=1
 for (char c : characters ) {
 if(mapChars .get(c) == 1) { //auto unboxing takes place
 return c;
 }
 }
 
 return null; 
}
Please enter the input string :
cactus
The first non repeated character is : a
mport java.util.HashMap ;
mport java.util.LinkedHashMap ;
mport java.util.Map;
mport java.util.Scanner ;
mport java.util.Set;

public class FirstNonRepeatedCharacter {
public static void main (String [] args) {
 System .out.println (" Please enter the input string :" );
 Scanner in = new Scanner (System .in); // read from System input
 String input = in.nextLine ();
 char firstNonRepeatedChar = logic (input );
 System .out.println ("The first non repeated character is : " + firstNonRepeatedChar );
}
private static Character logic (String input ) {
 if (input == null || input .trim().length () == 0) {
 return null;
 }
 
 input = input .toLowerCase ();
 char[] characters = input .toCharArray ();
 Map<Character , Integer > mapChars = new LinkedHashMap <>(20);
 // store characters and count
 for (char c : characters ) {
 if (mapChars .containsKey (c)) { // already exist
 mapChars .put(c, mapChars .get(c) + 1);
 } else { // not already exist
 mapChars .put(c, 1);
 }
 }
 //now loop through the mapChars
 Set<Character > characterSet = mapChars .keySet ();
 for (Character key : characterSet ) {
 if(mapChars .get(key) == 1){
 return key;
 }
 }
 
 return null;
 
}



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
