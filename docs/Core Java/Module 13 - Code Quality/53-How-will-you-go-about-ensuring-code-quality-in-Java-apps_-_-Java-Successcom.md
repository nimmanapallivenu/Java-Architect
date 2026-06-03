# 53. How will you go about ensuring code quality in Java apps    Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: How do you ensure code quality in your application?](#q1)
- [Q2: Do you use test driven development? Why / Why not?](#q2)
- [Q3: Write a program that will return whichever value is nearest to the value of 100 ](#q3)

---

## 🔹 Q1: How do you ensure code quality in your application?

**Answer:**

Code quality means writing readable , maintainable and robust code, that conforms as much as possible to the style-guideline that is used, and that has as
little as possible defects. It also means writing maintainable code with proper automated and manual tests.
1. Write a number of automated tests
Unit tests using JUnit or TestNG . For unit tests use mock objects to ensure that your tests don’ t fail due to volatility of the data changes. There are
mocking frameworks like EasyMock , Mockito , and PowerMock .
Integration testing of your services with JUnit or TestNG . Your integration tests are only as good as the quality of the data. You could either use
dedicated test databases or use frameworks like DBUnit to manage extraction and insertion of data.
Web testing using Selenium + WebDriver . Selenium + WebDriver ( Selenium interview questions and answers) allows you to reenact web user
experience and run it as an automated unit test using JUnit or T estNG.Your tests are only as good as the quality of the data. You could either use
dedicated system test databases or use frameworks like DBUnit. DBUnit allows you to extract the data from databases into flat XML files, and then
refresh (i.e. insert or update) the data into the database during setup phase of running the unit tests. There are handy proxy JDBC driver tool called
P6SPY , which logs the SQL queries that are executed against the database by the DBUnit . This P6SPY also very handy in debugging Hibernate’ s
generated SQL by acting as a proxy driver between JDBC and the real driver so that all generated SQL will be logged. There are other Web testing
tools like Badboy .
Load testing your application with tools like JMeter , OpenST A, etc. The Badboy compliments JMeter by allowing you to record scripts and then
exporting the scripts as a JMeter file to be used in JMeter .JMeter Interview Questions and Answers
2. Have regular code reviews. There are tools like Crucible from Atlassian that gives your team an ef ficient way to benefit from the power of constant code
review with features like inline commenting, simple workflow , asynchronous reviews, email and RSS notifications, JIRA integration and much more.
3. Using a number of code quality tools.
Checkstyle ensures the style of your Java code is standardized and “nice”. It checks white spaces, new lines, formatting, etc. (i.e. it looks on the
code line by line). This only ensure style of your code.
On the other hand there is PMD which not necessarily checks the style of your code but it checks the structure of the whole code. PMD scans Java
source code and looks for potential problems like possible bugs, dead code, suboptimal code, overcomplicated expressions, duplicate code, etc.
FindBugs is a static analysis tool to look for bugs in Java code. It discovers possible NullPointerExceptions and a lot more bugs.
Sonar is a very powerful tool covering 7 axes of code quality as shown below .

Sonar code quality tool
4. Using continuous integration servers (on a clean separate machine) like Bamboo , Hudson , CruiseContr ol, etc to continuously integrate and test your
code.
5. Not stopping to code once the code works. T oo many developers feel their job stops at making something happen. It is a best practice to constantly
refactor code with proper unit tests in place.

---

## 🔹 Q2: Do you use test driven development? Why / Why not?

**Answer:**

[Hint] Y es.
Gives you a better understanding of what you’re going to write. Gets you to clearly think what the inputs are and what the output is. Helps you separate
the concerns by getting you to think about the single responsibility principle (SRP).
Enforces a better test coverage. This gives you the confidence to refactor your code in the future, since you have a good coverage.
You won’ t waste time writing features you don’ t need.

---

## 🔹 Q3: Write a program that will return whichever value is nearest to the value of 100 from two given int numbers.

**Answer:**

1. Write pseudo code first:
Compute the difference to 100.
Find out the absolute difference as negative numbers are valid.
Compare the differences to find out the nearest number to 100.
2. Draw a diagram if it helps

3. Consider the edge cases and write unit tests
Write test cases for +ve, -ve, equal to, > than and < than values. For example, {25, 65}, {-25, -65}, {30, 30}, {65, 25}, {1 10, 145}, etc.
mport org.junit.Assert ;
mport org.junit.Test;

junit-xxx.jar and hamcrest0core-xxx.jar files need to be in the classpath.
4. Write code/{25, 65}, {-25, -65}, {30, 30}, {65, 25}, {1 10, 145},
public class CloseT o100T est {
 
 @Test
 public void testPositiveNumbers (){
 Assert .assertEquals (65,CloseT o100 .calculate (25, 65));
 }
 
 @Test
 public void testNegativeNumbers (){
 Assert .assertEquals (-25,CloseT o100 .calculate (-25, -65));
 }
 
 @Test
 public void testEqualNumbers (){
 Assert .assertEquals (30,CloseT o100 .calculate (30, 30));
 }
 
 @Test
 public void testLessThan100Numbers (){
 Assert .assertEquals (65,CloseT o100 .calculate (65, 25));
 }
 
 @Test
 public void testGreaterThan100Numbers (){
 Assert .assertEquals (110,CloseT o100 .calculate (110, 145));
 }
 
 @Test
 public void testNegativeNumbers2 (){
 Assert .assertEquals (-110,CloseT o100 .calculate (-110, -145));
 }

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »public class CloseT o100 {
 
 public static int calculate (int input1 , int input2 ) {
 
 //compute the difference. Negative values are allowed as well
 int iput1Dif f = Math .abs(100 - input1 );
 int iput2Dif f = Math .abs(100 - input2 );
 
 //compare the difference
 if (iput1Dif f < iput2Dif f) {
 return input1 ;
 }
 else if (iput2Dif f < iput1Dif f) {
 return input2 ;
 }
 else{
 return input1 ; //if tie, just return one
 }
 }

---



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

