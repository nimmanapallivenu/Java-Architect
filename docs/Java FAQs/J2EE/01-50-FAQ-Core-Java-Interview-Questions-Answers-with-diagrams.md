# 1 50+ FAQ Core Java Interview Questions & Answers with diagrams

## Table of Contents

- [Q1: What is the dif ference between “==” and “equals(…)” in comparing Java String ob](#q1)
- [Q2: Can you explain how Strings are interned in Java?](#q2)
- [Q3: Can you describe what the following code does and what parts of memory the local](#q3)
- [Q4: Why String class has been made immutable in Java?](#q4)
- [Q5: In Java, what purpose does the key words final , finally , and finalize fulfill?](#q5)
- [Q6: What is wrong with the following Java code?
mport java.util.concurrent .TimeUnit](#q6)
- [Q7: When using Generics in Java, when will you use a wild card, and when will you mo](#q7)
- [Q8: Can you describe “method overloading” versus “method overriding”? Does it happen](#q8)
- [Q9: What do you know about class loading? Explain Java class loaders? If you have a ](#q9)
- [Q10: Explain static vs. dynamic class loading?](#q10)

---

## Q1: What is the dif ference between “==” and “equals(…)” in comparing Java String objects?

**Answer:**

When you use “==” (i.e. shallow comparison), you are actually comparing the two object references to see if they point to the same object . When you use
“equals(…)”, which is a “deep comparison” that compares the actual string values . For example:
The variable s1 refers to the String instance created by “Hello”. The object referred to by s2 is created with s1 as an initializer , thus the contents of the two
String objects are identical, but they are 2 distinct objects having 2 distinct references s1 and s2. This means that s1 and s2 do not refer to the same object and
are, therefore, not ==, but equals( ) as they have the same value “Hello”. The s1 == s3 is true, as they both point to the same object due to internal caching. The
references s1 and s3 are interned and points to the same object in the string pool.public class StringEquals {
public static void main (String [ ] args) {
 String s1 = "Hello" ;
 String s2 = new String (s1);
 String s3 = "Hello" ;
 System .out.println (s1 + " equals " + s2 + "--> " + s1.equals (s2)); //true
 System .out.println (s1 + " == " + s2 + " --> " + (s1 == s2)); //false
 System .out.println (s1 + " == " + s3 + " -->" + (s1 == s3)); //true

String Pool Caches and you create a String object as a literal without the “new” keyword for
caching
In Java 6 — all interned strings were stored in the PermGen – the fixed size part of heap mainly used for storing loaded classes and string pool.
In Java 7 – the string pool was relocated to the heap . So, you are not restricted by the limited size.

---

## Q2: Can you explain how Strings are interned in Java?

**Answer:**

String class is designed with the Flyweight design pattern in mind. Flyweight is all about re-usability without having to create too many objects in
memory .
A pool of Strings is maintained by the String class. When the intern( ) method is invoked, equals(..) method is invoked to determine if the String already exist
in the pool. If it does then the String from the pool is returned instead of creating a new object. If not already in the string pool, a new String object is added to
the pool and a reference to this object is returned. For any two given strings s1 & s2, s1.intern( ) == s2.intern( ) only if s1.equals(s2) is true.
Two String objects are created by the code shown below . Hence s1 == s2 returns false.
/Two new objects are created. Not interned and not recommended.
String s1 = new String ("A");
String s2 = new String ("A");

s1.intern() == s2.intern() returns true, but you have to remember to make sure that you actually do intern() all of the strings that you’re going to compare. It’ s
easy to for get to intern() all strings and then you can get confusingly incorrect results. Also, why unnecessarily create more objects?
Instead use string literals as shown below to intern automatically:
s1 and s2 point to the same String object in the pool. Hence s1 == s2 returns true.
Since interning is automatic for String literals String s1 = “A”, the intern( ) method is to be used on Strings constructed with new String(“A”).

---

## Q3: Can you describe what the following code does and what parts of memory the local variables, objects, and references to the objects occupy in Java?String s1 = "A";
String s2 = "A";
public class Person {
 
 //instance variables
 private String name ;
 private int age;
 
 //constructor
 public Person (String name , int age) {
 this.name = name ;
 this.age = age;
 }
 public static void main (String [] args) {

**Answer:**

The above code outputs “Hello! John you are now 26”. The following diagram depicts how the dif ferent variable references & actual objects get stored. Person p1 = new Person ("John" , 25); // "p1" is object refrence in stack pointing to the object in the heap
 String greeting = "Hello" ; // "greeting" is a reference in stack pointing to an Object in Heap String pool
 p1.addOneAndPrint (greeting );
 }
 
 public void addOneAndPrint (String param ) {
 int valToAdd = 1; // local primitive variable in stack
 this.age = this.age + valToAdd ; // instance variable in heap is mutated to 26 via "param" reference in stack
 System .out.println (param + "! " + this); // calls toString() method
 }
 @Override
 public String toString () {
 return name + " you are now " + age;
 }

Java Stack, Heap, and String Pool
Stack: is where local variables both primitives like int, float, etc & references to objects in the heap and method parameters are stored as shown in the above
diagram.
Heap: is where objects are stored. For example, an instance of “Person” with name=”John” and age=25. Strings will be stored in String Pool within the heap.

---

## Q4: Why String class has been made immutable in Java?

**Answer:**

For performance & thread-safety .

1. Performance : Immutable objects are ideal for representing values of abstract data (i.e. value objects) types like numbers, enumerated types, etc. If you
need a dif ferent value, create a dif ferent object. In Java, Integer , Long , Float , Character , BigInteger and BigDecimal are all immutable objects. Optimization
strategies like caching of hashcode, caching of objects, object pooling, etc can be easily applied to improve performance. If Strings were made mutable, string
pooling would not be possible as changing the string with one reference will lead to the wrong value for the other references.
2. Thr ead safety as immutable objects are inherently thread safe as they cannot be modified once created. They can only be used as read only objects. They can
easily be shared among multiple threads for better scalability .
Q. Why is a char array i.e char[] preferred over String to store a password?
A. String is immutable in Java and stored in the String pool. Once it is created it stays in the pool until garbage collected. This has greater risk of 1) someone
producing a memory dump to find the password 2) the application inadvertently logging password as a readable string.
If you use a char[] instead, you can override it with some dummy values once done with it, and also logging the char[] like “[C@5829428e” is not as bad as
logging it as String “password123”.

---

## Q5: In Java, what purpose does the key words final , finally , and finalize fulfill?

**Answer:**

‘final ‘ makes a variable reference not changeable, makes a method not overridable, and makes a class not inheritable.
‘finally ‘ is used in a try/catch statement to almost always execute the code. Even when an exception is thrown, the finally block is executed. This is used to
close non-memory resources like file handles, sockets, database connections, etc till Java 7. This is is no longer true in Java 7.
Java 7 has introduced the AutoCloseable interface to avoid the unsightly try/catch/finally(within finally try/catch) blocks to close a resource. It also prevents
potential resour ce leaks due to not properly closing a resource.
//Pre Java 7
BufferedReader br = null;
ry {
 File f = new File("c://temp/simple.txt" );
 InputStream is = new FileInputStream (f);
 InputStreamReader isr = new InputStreamReader (is);
 br = new BufferedReader (isr);
 String read;

Java 7 – try can have AutoCloseble types. InputStr eam and OutputStr eam classes now implements the Autocloseable interface.
try can now have multiple statements in the parenthesis and each statement should create an object which implements the new java.lang.AutoClosable interface.
The AutoClosable interface consists of just one method. void close() throws Exception {}. Each AutoClosable resource created in the try statement will be
automatically closed without requiring a finally block. If an exception is thrown in the try block and another Exception is thrown while closing the resource, the while ((read = br.readLine ()) != null) {
 System .out.println (read);
 }
 catch (IOException ioe) {
 ioe.printStackT race();
 finally {
 //Hmmm another try catch. unsightly
 try {
 if (br != null) {
 br.close ();
 }
 } catch (IOException ex) {
 ex.printStackT race();
}
ry (InputStream is = new FileInputStream (new File("c://temp/simple.txt" ));
nputStreamReader isr = new InputStreamReader (is);
BufferedReader br2 = new BufferedReader (isr);) {
 String read;
 while ((read = br2.readLine ()) != null) {
 System .out.println (read);
 }
catch (IOException ioe) {
 ioe.printStackT race();

first Exception is the one eventually thrown to the caller . Think of the close( ) method as implicitly being called as the last line in the try block. If using Java 7 or
later editions, use AutoCloseable statements within the try block for more concise & readable code.
In Java 7 : When a resource (i.e. dbConnect) is defined outside the try-with-resource block you need to close it explicitly in the finally block. Y ou can’ t define
an object reference in your try-with-resource block, which was a bug.
Java 9 fixed it:Connection dbConnect = DriverManager .getConnection ("db-url" , "db-user" , "db-password" );
ry (ResultSet rs = dbConnect .createStatement ().executeQuery ("select * from mytable" )) {
 while (rs.next()) {
 //read results from rs
 }
 catch (SQLException e) {
 e.printStackT race();
 finally {
 if (null != dbConnect )
 dbConnect .close ();
 }
Connection dbConnect = DriverManager .getConnection ("db-url" , "db-user" , "db-password" );
ry (dbConnect ; ResultSet rs = dbConnect .createStatement ().executeQuery ("select * from mytable" )) {
 while (rs.next()) {
 //read results from rs
 }
} catch (SQLException e) {
 e.printStackT race();
}

‘finalize ‘ is called when an object is garbage collected. Y ou rarely need to override it. It should not be used to release non-memory resources like file handles,
sockets, database connections, etc because systems have only a finite number of these r esour ces and you do not know when the Garbage Collection (i.e.
GC) is going to kick in to release these non-memory resources through the finalize( ) method.
So, final and finally are used very frequently in your Java code, but the key word finalize is hardly or never used.

---

## Q6: What is wrong with the following Java code?
mport java.util.concurrent .TimeUnit ;
class Counter extends Thread {
 //instance variable
 Integer count = 0;
 // method where the thread execution will start
 public void run() {
 int fixed = 6; //local variable
 
 for (int i = 0; i < 3; i++) {
 System .out.println (Thread .currentThread ().getName () + ": result="
 + performCount (fixed ));
 try {
 TimeUnit .SECONDS .sleep (1);
 } catch (InterruptedException e) {
 e.printStackT race();
 }
 } 
 }
 // let’ s see how to start the threads
 public static void main (String [] args) {
 System .out.println (Thread .currentThread ().getName () + " is executing..." );
 Counter counter = new Counter ();
 
 //5 threads
 for (int i = 0; i < 5; i++) {

**Answer:**

Above code is NOT thread-safe. This is explained in detail at
Heap Vs Stack, Thread safety & Synchronized keyword

---

## Q7: When using Generics in Java, when will you use a wild card, and when will you mot use a wild card?

**Answer:**

It depends on what you are trying to do with a collection.
1. Use the ? extends wildcard if you need to retrieve object from a data structure. That is read only . You can’ t add elements to the collection.
2. Use the ? super wildcard if you need to add objects to a data structure.
3. If you need to do both things (i.e. read and add objects), don’ t use any wildcard.

---

## Q8: Can you describe “method overloading” versus “method overriding”? Does it happen at compile time or runtime ?

**Answer:**

Overloading deals with multiple methods in the same class with the same name but differ ent method signatur es. Both the below methods have the same
method names but dif ferent method signatures, which mean the methods are overloaded. Thread t = new Thread (counter );
 t.start();
 }
 
 }
 
 //multiple threads can access me concurrently
 private int performCount (int fixed ) {
 return (fixed + ++count );
 }
public class {
public static void evaluate (String param1 ); // overloaded method #1
public static void evaluate (int param1 ); // overloaded method #2
}

This happens at compile-time . This is also called compile-time polymorphism because the compiler must decide which method to run based on the data types
of the ar guments. If the compiler were to compile the statement:
it could see that the ar gument was a “string” literal, and generates byte code that called method #1.
If the compiler were to compile the statement:
it could see that the ar gument was an “int”, and generates byte code that called method #2.
Overloading lets you define the same operation in differ ent ways for differ ent data .
Overriding deals with two methods, one in the parent class and the other one in the child class and has the same name and same signatur es. Both the below
methods have the same method names and the signatur es but the method in the subclass “B” overrides the method in the superclass (aka the parent class)
“A”.
Parent classevaluate (“My Test Argument passed to param1 ”);
evaluate (5);
public class A {

Child class
This happens at runtime . This is also called runtime polymorphism because the compiler does not and cannot know which method to call. Instead, the JVM
must make the determination while the code is running.
The method compute(..) in subclass “B” overrides the method compute(..) in super class “A”. If the compiler has to compile the following method,
The compiler would not know whether the input ar gument ‘ reference ‘ is of type “A” or type “B”. This must be determined during runtime whether to call
method #3 or method #4 depending on what type of object (i.e. instance of Class A or instance of Class B) is assigned to the input variable “reference”.public int compute (int input ) { //method #3
 return 3 * input ;
}
}
public class B extends A {
@Override
public int compute (int input ) { //method #4
 return 4 * input ;
}
}
public int evaluate (A reference , int arg2) {
int result = reference .compute (arg2);
}

Overriding lets you define the same operation in differ ent ways for differ ent object types . It is determined by the “stored” object type, and NOT by the
“referenced” object type.
Q. Are overriding & polymorphism applicable static methods as well?
A. No. If you try to override a static method, it is known as hiding or shadowing .
Core Java Interview Questions & answers will not be complete without Class Loaders .

---

## Q9: What do you know about class loading? Explain Java class loaders? If you have a class in a package, what do you need to do to run it? Explain dynamic
class loading?

**Answer:**

Class loaders are hierarchical. Classes are introduced into the JVM as they are referenced by name in a class that is already running in the JVM. So, how is
the very first class loaded? The very first class is specially loaded with the help of static main( ) method declared in your class. All the subsequently loaded
classes are loaded by the classes, which are already loaded and running. A class loader creates a namespace. All JVMs include at least one class loader that is
embedded within the JVM called the primordial (or bootstrap) class loader . The JVM has hooks in it to allow user defined class loaders to be used in place of
primordial class loader . Let us look at the class loaders created by the JVM.A obj1 = new B( );
A obj2 = new A( );
evaluate (obj1, 5); // 4 * 5 = 20. method #4 is invoked as stored object is of type B
evaluate (obj2, 5); // 3 * 5 = 15. method #3 is invoked as stored object is of type A

Java Class Loader Basics
Class loaders are hierarchical and use a delegation model when loading a class. Class loaders request their parent to load the class first before attempting to load
it themselves. When a class loader loads a class, the child class loaders in the hierarchy will never reload the class again. Hence uniqueness is maintained.
Classes loaded by a child class loader have visibility into classes loaded by its parents up the hierarchy but the reverse is not true as explained in the above
diagram.
3 Java class loading interview Q&As to ascertain your depth of Java knowledge .

---

## Q10: Explain static vs. dynamic class loading?

**Answer:**

Classes are statically loaded with Java’ s “new” operator .

Dynamic loading is a technique for programmatically invoking the functions of a classloader at runtime . Let us look at how to load classes dynamically .
The above static method “forName” & the below instance (i.e. non-static method) “loadClass”
return the class object associated with the class name.Once the class is dynamically loaded the following method returns an instance of the loaded class. It’ s just
like creating a class object with no ar guments.class MyClass {
public static void main (String args[]) {
 Car c = new Car( );
}
}
/static method which returns a Class
Class clazz = Class .forName ("com.Car" ); // The value "com.Car" can be evaluated at runtime & passed in via a variable
ClassLoader classLoader = MyClass .class .getClassLoader ();
Class clazz = classLoader .loadClass (("com.Car" ); //Non-static method that returns a Class
/ A non-static method, which creates an instance of a

The string class name like “com.Car” can be supplied dynamically at runtime. Unlike the static loading, the dynamic loading will decide whether to load the
class “com.Car” or the class “com.Jeep” at runtime based on a runtime condition.
Static class loading throws NoClassDefFoundError if the class is NOT FOUND whereas the dynamic class loading throws ClassNotFoundException if
the class is NOT FOUND.
Q. What is the dif ference between the following approaches?
and/ class (i.e. creates an object).
Car myCar = (Car) clazz .newInstance ( );
public void process (String classNameSupplied ) {
 Object vehicle = Class .forName (classNameSupplied ).newInstance ();
 //......
}
Class .forName ("com.SomeClass" );
classLoader .loadClass ("com.SomeClass" );

A.
Class.forName(“com.SomeClass”)
— Uses the caller ’s classloader and initializes the class (runs static intitializers, etc.)
classLoader.loadClass(“com.SomeClass”)
— Uses the “supplied class loader”, and initializes the class lazily (i.e. on first use). So, if you use this way to load a JDBC driver , it won’ t get registered, and
you won’ t be able to use JDBC.
The “java.lang.API” has a method signature that takes a boolean flag indicating whether to initialize the class on loading or not, and a class loader reference.
So, invoking
is same as invokingforName (String name , boolean initialize , ClassLoader loader )
Class .forName ("com.SomeClass" )
forName ("com.SomeClass" , true, currentClassLoader )

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
