# 54. 5 Java unit testing interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

Q1 Why use mock objects in unit testing?
A1 Unit testing is widely accepted as a “best practice” for software development. When you write an object, you must also provide an automated test class
containing methods by calling its various public methods with various parameters and making sure that the values returned are appropriate.
When you’re dealing with simple data or service objects, writing unit tests is straightforward. However , in reality the object under test rely on other objects or
layers of infrastructure, and it is often expensive, impractical, or inef ficient to instantiate these collaborators.
For example, to unit test an object that uses a database, it may be burdensome to install a local copy of the database, run your tests, then tear the local database
down again. Mock objects provide a way out of this dilemma. A mock object conforms to the interface of the real object, but has just enough code to simulate
the tested object and track its behavior . For example, a database connection for a particular unit test might record the query while always returning the same hard
coded result. As long as the class being tested behaves as expected, it won’ t notice the difference, and the unit test can check that the proper query was emitted.
Here are some reasons why mock objects are handy:
The unit tests as the name implies must test only a unit of the code and not all its collaborating dependencies. You only have to worry about the class
under test. Mock objects allow you to achieve this by mocking external resource and coding dependencies. The example in the next question demonstrates
how we can mock reading from a file, which is an external resource.
The unit tests need to test for the proper boundary conditions. For example, positive values, negative values, zero value, etc. The mock object make your
life easier for mimicking these boundary conditions.
One of the biggest mistake one can make in writing quality unit tests is to have state dependencies between unit tests. The unit tests must be able to run in
any order . The mock objects will help you isolate these state dependencies, and make your tests isolated and independent.For example
Test the DAO in isolation by mocking the calls to external resources like database, file, etc.
Test your service in isolation by mocking the calls to your DAO.
Having said this, too much mocking can make your code hard to read and understand. So, it is important to have the right balance without overdoing.
Q2 How would you go about using mock objects in your unit tests?
A2
Step 1: Have the relevant dependencies required to write unit tests.
<properties >
<junit.version >4.8.1 </junit.version >
<mockito .version >1.8.5 </mockito .version >

Step 2: The UserDaoImpl is an implementation of the interface UserDao. The implementation read the user names from a text file “users.txt”.<powermock .version >1.4.8 </powermock .version > 
</properties >
<dependencyManagement >
<dependencies >
 ...
 <dependency >
 <groupId >junit</groupId >
 <artifactId >junit</artifactId >
 <version >${junit.version }</version >
 <scope >test</scope >
 </dependency >
 <dependency >
 <groupId >org.mockito </groupId >
 <artifactId >mockito -all</artifactId >
 <version >${mockito .version }</version >
 <scope >test</scope >
 </dependency >
 <dependency >
 <groupId >org.powermock </groupId >
 <artifactId >powermock -module -junit4 </artifactId >
 <version >${powermock .version }</version >
 <scope >test</scope >
 </dependency >
 <dependency >
 <groupId >org.powermock </groupId >
 <artifactId >powermock -api-mockito </artifactId >
 <version >${powermock .version }</version >
 <scope >test</scope >
 </dependency >
...
</dependencies >
<dependencyManagement >
Peter Smith
Aaron Lachlan

The “UserDao.java” interface
The “UserDaoImpl.java” class that reads from the “user .txt” file implements the interface “UserDao.java”.Zara John
Felix Chan
mport java.util.List;
public interface UserDao {
 public List<string > readUsers () throws UsersException ;
}
mport java.io.InputStream ;
mport java.util.Arrays ;
mport java.util.Collections ;
mport java.util.List;
mport java.util.Scanner ;
public class UserDaoImpl implements UserDao {
 private static final String DELIMITER = System .getProperty ("line.separator" );
 
 public UserDaoImpl (){}
 @Override
 public List<string > readUsers () throws UsersException {
 InputStream is = getResource ();
 
 if (is == null) {
 throw new UsersException ("users file is not found" );
 }
 Scanner sc = new Scanner (is);
 String value = sc.useDelimiter (DELIMITER + "\r").next();
 String [] users = value .split(DELIMITER );

STEP 3: Finally the unit test that uses the Mockito framework to mock the actual loading of the user names from text file. The user names will be supplied via
the method getDummyIs(). The UserDaoImpl is partially mocked with the spy method. This means the getResource() method is mocked by supplying some
dummy data within the test itself. The readUsers() method is executed from the class under test, which is UserDaoImpl. The getResource() method is mocked to
return a user name of “John Patrick” evey time it is invoked. return (users == null || users .length > 0 ? Arrays
 .asList (users ) : Collections .<string > emptyList ());
 }
 private InputStream getResource () {
 ClassLoader cl = Thread .currentThread () .getContextClassLoader ();
 InputStream is = cl.getResourceAsStream ("unittest/users.txt" );
 return is;
 }
mport java.io.ByteArrayInputStream ;
mport java.io.InputStream ;
mport java.util.List;
mport junit.framework .Assert ;
mport org.junit.Test;
mport org.junit.runner .RunW ith;
mport org.powermock .api.mockito .PowerMockito ;
mport org.powermock .core.classloader .annotations .PrepareForT est;
mport org.powermock .modules .junit4 .PowerMockRunner ;
@RunW ith(PowerMockRunner .class )
@PrepareForT est(UserDaoImpl .class )
public class UserDaoW ithMockT est {
 @Test
 public void testGetUsers () throws Exception {

Q3 What mocking frameworks have you used?
A3 Mockito, EasyMock, and PowerMock.
PowerMock is a framework that extends other mock libraries such as EasyMock and Mockito with more powerful capabilities like mocking of static methods,
constructors, final classes and methods, private methods, removal of static initializers and more.
Q4 What is the difference between a mock object and a stub?
A4 The key difference to note is the ability of the mock objects to verify if a particular method was invoked and if yes, how many times was invoked. This is
demonstrated with the last two lines with the verify statement. This is a common Java interview question quizzing the candidate’ s understanding of the
difference between a mock object and stub. final UserDao partiallyMockedUserDao = PowerMockito .spy(new UserDaoImpl ());
 PowerMockito .doReturn (getDummyIs ()).when (partiallyMockedUserDao , "getResource" );
 List<string > users = partiallyMockedUserDao .readUsers ();
 Assert .assertEquals (1, users .size());
 }
 @Test(expected = unittest .UsersException .class )
 public void testGetUsersNegative () throws Exception {
 final UserDao partiallyMockedUserDao = PowerMockito .spy(new UserDaoImpl ());
 PowerMockito .doReturn (null).when (partiallyMockedUserDao ,"getResource" );
 partiallyMockedUserDao .readUsers ();
 }
 @Test(expected = unittest .UsersException .class )
 public void testGetUsers2 () throws Exception {
 final UserDao partiallyMockedUserDao = PowerMockito .spy(new UserDaoImpl ());
 PowerMockito .doReturn (new ByteArrayInputStream ("".getBytes ())).when (partiallyMockedUserDao , "getResource" );
 List<string > users = partiallyMockedUserDao .readUsers ();
 Assert .assertEquals (0, users .size());
 }
 public InputStream getDummyIs () {
 String str = "John Patrick" ;
 return new ByteArrayInputStream (str.getBytes ());
 }

Q5 What is BDD?
A5 BDD is principally an idea about how software development should be managed by both business interests and technical insight. T est-driven development
focuses on the developer ’s opinion on how parts of the software should work. Behavior -driven development focuses on the users’ opinion on how they want
your application to behave. So, when you start writing a test, you need to think about the stories, and each story should cover three things:
Given : an input value of 2
When : you multiply the input with 3
Then : result should be 6
Even you write unit tests as part of TDD (T est Driven Development) or without TDD , you need to think about Given … When … Then …
Here is a simple example using the jBehave framework in Java.
Step 1: Maven pom.xml file on jBehave dependency/The test case
 @Test
 public void testGetPositionFeedCSV () throws Exception
 {
 String str = "dummyCSV" ;
 //Set up behavior
 when (mockMyAppService .getPositionFeedCSV (any(PositionFeedCriteria .class ))).thenReturn (str);
 when (response .getW riter()).thenReturn (writer );
 
 //Invoke controller
 controller .getPositionFeedCSV (POR TFOLIO_CODE , VALUA TION_DA TE, response );
 
 //Verify behavior
 verify (mockMyAppService , times (1)).getPositionFeedCSV (any(PositionFeedCriteria .class ));
 verify (writer , times (1)).write (any(String .class ));
 }
<dependency >

Step 2: Define the story in plain English that business users and testers can understand using Given… When Then… style. The “math.story” file under
“src/main/resources/jbehave” folder
Step 3: Map the above scenarios based stories to Java equivalent. <groupId >org.jbehave </groupId >
 <artifactId >jbehave -core</artifactId >
 <version >3.8</version >
</dependency >
Scenario : 2 squared
Given a variable input with value 2
When I multiply input by 2
Then result should equal 4
Scenario : 3 squared
Given a variable input with value 3
When I multiply input by 3
Then result should equal 9
mport org.jbehave .core.annotations .Given ;
mport org.jbehave .core.annotations .Named ;
mport org.jbehave .core.annotations .Then ;
mport org.jbehave .core.annotations .When ;
mport org.jbehave .core.steps .Steps ;
public class MathSteps extends Steps
{
 private int input ;
 private int result ;

Step 4: Write a main class to excute the scenarios. @Given ("a variable input with value $value" )
 public void givenInputV alue(@Named ("value" ) int value )
 {
 input = value ;
 }
 
 @When ("I multiply input by $value" )
 public void whenImultiplyInputBy (@Named ("value" ) int value )
 {
 result = input * value ;
 }
 
 @Then ("result should equal $value" )
 public void thenInputshouldBe (@Named ("value" ) int value )
 {
 if (value != result ) {
 throw new RuntimeException ("result is " + result + ", but should be " + value );
 }
 }
mport java.util.Arrays ;
mport java.util.List;
mport org.jbehave .core.embedder .Embedder ;
public class JBehaveT est
{
 private static Embedder embedder = new Embedder ();
 private static List<String > storyPaths = Arrays .asList ("jbehave/math.story" );
 
 public static void main (String [] args)
 {
 embedder .candidateSteps ().add(new MathSteps ());
 try
 {
 embedder .runStoriesAsPaths (storyPaths );

Behaviour -Driven Development ( BDD ) is an evolution in the thinking behind T est Driven Development ( TDD — W riting tests before writing code) and
Acceptance T est Driven Development ( ATDD — write acceptnce tests, and for many agile teams, acceptance tests are the main form of functional specification
and the formal expression of the business requirements). The BDD basically combines TDD and Domain Driven Design . It aims to provide common
vocabulary that can be used between business and technology .
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » }
 catch (Exception e)
 {
 e.printStackT race();
 }
 
 }



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

