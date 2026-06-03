# 26. Java FP Lambda expressions by examples   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

In Imperative (E.g. OOP , procedural programming, etc) programming you can say
where you are assigning x to x + 5. if x were to b 2, then after assignment it becomes 7 (i.e. 2 + 5)
In functional pr ogramming (FP), you can’t say “x = x + 5” why? if x were to be 2, “2 = 2 + 5” is wrong. FP does not have assignment statements. FP is all
about computation as the evaluation of “ mathematical functions ” and avoids changing-states and mutablity . In FP , you need to say
where f(x) and f(x,y) are functions. Similarly , in the example below “(el1, el2) -> el1 + “,” + el2″ is a lambda expr ession in FP . Where “el1” and “el2” are
consecutive elements in a given list.
Example 1: Reducing a list of strings to CSV
stream -> reduce -> getx = x + 5
y = x + y
f(x) -> x + 5
f(x,y) -> x + y
mport java.util.Arrays ;
mport java.util.List;

Output
Example 2: Reducing a list of Integers to string (e.g. CSV)
stream -> map -> reduce -> getpublic class Technology {
 public static void main (String [] args) {
 List<String > technologies = Arrays .asList ("Java" , "JEE" , "JDBC" , "Spring" , "Hibernate" );
 // Java 8 FP
 String csvTechnologies = technologies .stream ()
 .reduce ((el1, el2) -> el1 + "," + el2)
 .get();
 System .out.println (csvTechnologies );
 }
Java,JEE,JDBC ,Spring ,Hibernate
mport java.util.Arrays ;
mport java.util.List;
public class Weights {
 public static void main (String [] args) {
 List<Integer > weights = Arrays .asList (25, 32, 45, 66, 77);
 // Java 8 FP
 String csvWeights = weights .stream ()

Example 3: Converting a List of unique objects to a map
Key=name -> value =Employee .map(el1 -> el1.toString ())
 .reduce ((el1, el2) -> el1 + "," + el2)
 .get();
 System .out.println (csvWeights );
 }
25,32,45,66,77
mport java.util.Arrays ;
mport java.util.List;
mport java.util.Map;
mport java.util.stream .Collectors ;
public class ListT oMap {
 public static void main (String [] args) {
 List<Employee > employees = Arrays .asList (new Employee ("John" , 25, "Maths" ),
 new Employee ("Sam" , 35, "English" ),
 new Employee ("Alice" , 42, "Science" ));
 // Java 8 FP
 //Assume that names are unique, and use it as a key , and the value will be the Employee object
 Map<String , Employee > mapEmployees = employees .stream ()
 .collect (Collectors
 .toMap (emp -> emp.getName (), emp -> emp));

Output System .out.println (mapEmployees );
 }
 
 //inner pojo class
 static class Employee {
 private String name ;
 private int age;
 private String department ;
 
 public Employee (String name , int age, String department ) {
 super ();
 this.name = name ;
 this.age = age;
 this.department = department ;
 }
 
 //getters & setters
 public String getName () {
 return name ;
 }
 public int getAge () {
 return age;
 }
 public String getDepartment () {
 return department ;
 }
 
 //toString
 @Override
 public String toString () {
 return "Employee [name=" + name + ", age=" + age + ", department=" + department + "]";
 }
 }

Example 4: Converting a List of non unique objects to a map
Key=name -> value =List<Employee>{Alice =Employee [name =Alice , age=42, department =Science ], John =Employee [name =John , age=25, department =Maths ], Sam=Employee [name =Sam,
age=35, department =English ]}
mport java.util.Arrays ;
mport java.util.List;
mport java.util.Map;
mport java.util.stream .Collectors ;
public class ListT oMap {
 public static void main (String [] args) {
 List<Employee > employees = Arrays .asList (new Employee ("John" , 25, "Maths" ),
 new Employee ("John" , 35, "English" ),
 new Employee ("Alice" , 42, "Science" ));
 // Java 8 FP
 //Assume that names are NOT unique, and use it as a key , and the
 //value will be a List of Employees
 Map<String , List<Employee >> mapEmployees = employees .stream ()
 .collect (Collectors .groupingBy (emp -> emp.getName ()));
 System .out.println (mapEmployees );
 }
 
 //inner pojo class
 static class Employee {
 private String name ;
 private int age;
 private String department ;
 
 public Employee (String name , int age, String department ) {

Output
Example 5 : Converting a Map keys to a List, sorted by values
entrySet -> stream -> sorted -> map -> collect super ();
 this.name = name ;
 this.age = age;
 this.department = department ;
 }
 
 //getters & setters
 public String getName () {
 return name ;
 }
 public int getAge () {
 return age;
 }
 public String getDepartment () {
 return department ;
 }
 
 //toString
 @Override
 public String toString () {
 return "Employee [name=" + name + ", age=" + age + ", department=" + department + "]";
 }
 }
{Alice =[Employee [name =Alice , age=42, department =Science ]], John =[Employee [name =John , age=25, department =Maths ], Employee [name =John , age=35,
department =English ]]}

Output
Now , to sort by “length” of the faculty namemport java.util.Comparator ;
mport java.util.HashMap ;
mport java.util.List;
mport java.util.Map;
mport java.util.stream .Collectors ;
public class MapT oList {
 public static void main (String [] args) {
 Map<String , String > mapNameFaculty = new HashMap <String , String >();
 mapNameFaculty .put("John" , "Maths" );
 mapNameFaculty .put("Sam" , "English" );
 mapNameFaculty .put("Alice" , "Science" );
 
 //Convert to a List of names sorted by faculty
 List<String > listOfNamesSortedByFaculty = mapNameFaculty .entrySet ().stream ()
 .sorted (Comparator .comparing (Map.Entry ::getValue))
 .map(Map.Entry ::getKey )
 .collect (Collectors .toList ());
 
 System .out.println (listOfNamesSortedByFaculty );
 }
Sam, John , Alice ]

Output
FP is more memory intensive than imperative programming because in FP data is not overwritten, but sequences of versions are created to represent the data
modification. Nowadays, both the memory & disk are cheap. FP gives the programmer a lot more control about wrangling the data. V ery useful in big data formport java.util.HashMap ;
mport java.util.List;
mport java.util.Map;
mport java.util.stream .Collectors ;
public class MapT oList {
 public static void main (String [] args) {
 Map<String , String > mapNameFaculty = new HashMap <String , String >();
 mapNameFaculty .put("John" , "Maths" );
 mapNameFaculty .put("Sam" , "English" );
 mapNameFaculty .put("Alice" , "Science" );
 
 //Convert to a List of names sorted by faculty
 List<String > listOfNamesSortedByFaculty = mapNameFaculty .entrySet ().stream ()
 .sorted ((e1,e2) -> e1.getValue().length () - e2.getValue().length ())
 .map(Map.Entry ::getKey )
 .collect (Collectors .toList ());
 
 System .out.println (listOfNamesSortedByFaculty );
 }
John , Alice , Sam]

functions like map, flatMap, reduce, combine, sort, etc. The “map” applies a given function to every data record on different machines in a cluster . This can be
run in parallel . The “reduce” combines the individual results on different machines “by “applying a given function” to every data to reach a final result.
Example 6 : Sum the list of numbers across the Hadoop cluster with Apache Spark
Example 7 : Counting the number of blank lines in a given text input with Apache Spark.
More detailed explanation at: Apache Spark interview questions & answersSparkConf conf = new SparkConf ().setAppName ("Sequence T o Avro").setMaster ("local[2]" ); 
JavaSparkContext sc = new JavaSparkContext (conf);
List<Integer > data = Arrays .asList (1, 2, 3, 4, 5);
JavaRDD <Integer > distData = sc.parallelize (data);
distData .reduce ((a, b) -> a + b)
JavaRDD <String > lines = sc.textFile ("data.txt" ); //default no. of partitions
final Accumulator <Integer > blankLines = sc.accumulator (0); 
JavaPairRDD <String , Integer > counts = lines .flatMap (line ->
 {
 if ("".equals (line)) {
 blankLines .add(1); // increment the shared variable
 }
 return Arrays .asList (line.split(" "));
 }).mapT oPair (word -> new Tuple2 <String , Integer >(word , 1))
 .reduceByKey ((x, y) -> x + y);
System .out.println ("Blank lines count: " + blankLines .value ());

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »



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

