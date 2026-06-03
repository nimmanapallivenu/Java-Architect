# 45. Java 8 way to reading files   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

Output:package com.read.file;
mport java.nio.charset .StandardCharsets ;
mport java.nio.file.Files ;
mport java.nio.file.Path;
mport java.nio.file.Paths ;
mport java.util.stream .Stream ;
public class MyFileReader {
 public static void main (String [] args) {
 Path file = Paths .get("C:\\Users\\akumaras\\workspace\\test\\src\\com\\read\\file\\readme.txt" );
 try
 {
 //Java 8: Stream class
 Stream <String > lines = Files .lines ( file, StandardCharsets .UTF_8 );
 for( String line : (Iterable <String >) lines ::iterator )
 {
 System .out.println ("read=" + line);
 }
 } catch (Exception e){
 e.printStackT race();
 }
 }

#1 double colon notation ::
The new double colon ( ::) operator that Java 8 has to convert a normal method into lambda expression. So,
Instead of:
You can do:
#2 Why is stream::iterator used?
“lines::iterator ” where iterator() is an instance method on “BaseStream<T ,Stream<T>>” from which java.util.Str eam<T> extends. The “iterator()” returns an
“Iterator<T>”. The for each loop works on Iterable<T>.
So, given a Stream s, the following results in an Iterable:read=A big brown fox
read=jumped over the fence
ist.forEach (n -> System .out.println (n));
ist.forEach (System .out::println );
for (element : iterable );

If you want to use this directly in an enhanced-for loop, you have to apply a cast in order to establish a tar get type for the method reference.
#3 Iterator Vs Iterable difference?
An Iterable<T> is a simple representation of a series of elements that can be iterated over , and it does not have any iteration state such as a “current element”.
Instead, it has a “ iterator() ” method that produces an Iterator . Implementing this interface allows an object to be the tar get of the “ for-each loop ” statement.
An Iterator<T> is the object with iteration state to let you check if it has more elements using hasNext() and move to the next() element.
Read from the classpaths::iterator
for( String line : (Iterable <String >) lines ::iterator )
package com.read.file;
mport java.net.URL ;
mport java.nio.charset .StandardCharsets ;
mport java.nio.file.Files ;
mport java.nio.file.Path;
mport java.nio.file.Paths ;
mport java.util.stream .Stream ;

Output:
Filter the line that has “fox”public class MyFileReader {
 public static void main (String [] args) {
 Path file = null;
 try
 {
 //read from the classpath
 URL url = MyFileReader .class .getResource ("readme.txt" );
 file = Paths .get(url.toURI ());
 //Java 8: Stream class
 Stream <String > lines = Files .lines ( file, StandardCharsets .UTF_8 );
 for( String line : (Iterable <String >) lines ::iterator )
 {
 System .out.println ("read=" + line);
 }
 } catch (Exception e){
 e.printStackT race();
 }
 }
 
read=A big brown fox
read=jumped over the fence
package com.read.file;

Output:mport java.net.URL ;
mport java.nio.charset .StandardCharsets ;
mport java.nio.file.Files ;
mport java.nio.file.Path;
mport java.nio.file.Paths ;
mport java.util.Optional ;
mport java.util.stream .Stream ;
public class MyFileReader {
 public static void main (String [] args) {
 Path file = null;
 try
 {
 //read from the classpath
 URL url = MyFileReader .class .getResource ("readme.txt" );
 file = Paths .get(url.toURI ());
 //Java 8: Stream class
 Stream <String > lines = Files .lines ( file, StandardCharsets .UTF_8 );
 //Java 8 Optional class with Lambda expression s -> s.contains("fox")
 Optional <String > lineThatHasFox = lines .filter (s -> s.contains ("fox" )).findFirst ();
 if(lineThatHasFox .isPresent ()){
 System .out.println (lineThatHasFox .get());
 }
 } catch (Exception e){
 e.printStackT race();
 }
 }

Note: filter() is an intermediate operation , returning a Stream, and findFirst() is a terminal operation .
Count lines
Output:A big brown fox
package com.read.file;
mport java.net.URL ;
mport java.nio.charset .StandardCharsets ;
mport java.nio.file.Files ;
mport java.nio.file.Path;
mport java.nio.file.Paths ;
public class MyFileReader {
 public static void main (String [] args) {
 Path file = null;
 try
 {
 //read from the classpath
 URL url = MyFileReader .class .getResource ("readme.txt" );
 file = Paths .get(url.toURI ());
 long count = Files .lines ( file, StandardCharsets .UTF_8 ).count ();
 System .out.println ("count lines=" + count );
 } catch (Exception e){
 e.printStackT race();
 }
 }

Note: count() is a terminal operation as it does not return a stream.
Can you workout the output of the following code?count lines =2
package com.read.file;
mport static java.util.stream .Collectors .toList ;
mport java.net.URL ;
mport java.nio.charset .StandardCharsets ;
mport java.nio.file.Files ;
mport java.nio.file.Path;
mport java.nio.file.Paths ;
mport java.util.List;
mport java.util.Arrays ;
public class MyFileReader {
 public static void main (String [] args) {
 Path file = null;
 try
 {
 //read from the classpath
 URL url = MyFileReader .class .getResource ("readme.txt" );
 file = Paths .get(url.toURI ());
 List<String > output = Files .lines ( file, StandardCharsets .UTF_8 )
 .filter (s -> s.contains ("fox" ))
 .map(line -> line.split("\\s+" ))

Output:
An array of size two containing …..
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » .flatMap (Arrays ::stream )
 .limit (2)
 .collect (toList ());
 System .out.println (output );
 } catch (Exception e){
 e.printStackT race();
 }
 }
A, big]



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03


---

## 📚 Related Topics

- [Java Overview](../Module%2001%20-%20Java%20Overview/)
- [Java Data Types](../Module%2002%20-%20Java%20Data%20Types/)
- [OOP Concepts](../Module%2006%20-%20OOP%20and%20FP/)

---

## 💡 Key Takeaways

Review the questions above and ensure you understand:
- Core concepts and their practical applications
- Real-world scenarios and use cases
- Best practices and common pitfalls

---

**[⬆ Back to Top](#)**

