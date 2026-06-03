# Java 8 String streams and finding the first non repeated character with functional programming   Java Success.com

## Table of Contents

- [Q1: Find the first non repeated character in a given string input using Java 8 or la](#q1)

---

## Q1: Find the first non repeated character in a given string input using Java 8 or later?

**Answer:**

Extends Find the first non repeated character in a given string input with Java 8 functional pr ogramming .
Examples to understand string streams:mport java.util.*;
mport java.util.function .*;
mport java.util.stream .Collectors ;
public class FirstNonRepeatedCharacter {
public static void main (String [] args) {
 System .out.println (" Please enter the input string :" );
 Scanner in = new Scanner (System .in); // read from System input
 String input = in.nextLine ();
 Character firstNonRepeatedChar = logic (input );
 System .out.println ("The first non repeated character is : " + firstNonRepeatedChar );
 in.close ();
}
private static Character logic (String input ) { 
 Character result = input .chars () //string stream
 .mapT oObj (i -> Character .toLowerCase (Character .valueOf ((char) i))) //convert to lowercase & then to Character object
 .collect (Collectors .groupingBy (Function .identity (), LinkedHashMap ::new, Collectors .counting ())) //store in a map with the count
 .entrySet ().stream ()
 .filter (entry -> entry .getValue() == 1L)
 .map(entry -> entry .getKey ())
 .findFirst ().get();
 
 return result ; 
}

Example 1
Output:
Example 2public class Java8StringStream {
public static void main (String [] args) {
 "Cactus" .chars ().forEach (c -> System .out.println ((char)c));
}
}
C
a
c
u
s
public class Java8StringStream {
public static void main (String [] args) {
 "Cactus" .chars ()
 .mapT oObj (c -> Character .valueOf ((char)c))
 .findFirst ()
 .ifPresent (System .out::println );

Output:
Example 3
Output:}
}
C
mport java.util.stream .Collectors ;
public class Java8StringStream {
 public static void main (String [] args) {
 "stress" .chars ()
 .mapT oObj (c -> Character .valueOf ((char)c))
 .collect (Collectors .toSet ()) //remove duplicates
 .forEach (s -> System .out.println (s));
 }

Example 4
Output:
Example 5e
s
r
mport java.util.function .Function ;
mport java.util.stream .Collectors ;
public class Java8StringStream {
public static void main (String [] args) {
 "stress" .chars ()
 .mapT oObj (c -> Character .valueOf ((char)c))
 .collect (Collectors .groupingBy (Function .identity ()))
 .entrySet ()
 .stream ()
 .forEach (s -> System .out.println (s));;
}
e=[e]
=[t]
s=[s, s, s]
r=[r]

Output:public class Java8StringStream {
 public static void main (String [] args) {
 "stress" .chars ()
 .mapT oObj (c -> Character .valueOf ((char)c))
 .sorted ((s1, s2) -> {
 System .out.printf ("sort: %s; %s\n" , s1, s2);
 return s1.compareT o(s2);
 })
 .forEach (s -> System .out.println (s));;
 }
e
r
s
s
s

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
