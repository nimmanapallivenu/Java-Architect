# Java Modifiers

> **Module**: Modifiers Annotations Initializers  
> **Topic**: Java Modifiers

---

## 📋 Table of Contents



- [Q1: In Java, what purpose does the key words final, finally, and finalize fulfill?](#q1)
- [Q2: What is the difference between ‘ final ‘ and ‘ const ‘ modifiers on a variable?](#q2)
- [Q3: What is a volatile key word in Java?](#q3)
- [Q4: How does volatile keyword differ from the synchr onized keyword?](#q4)
- [Q5: What is a “transient” modifier? Can you mark a static variable as transient?](#q5)
- [Q6: What value will the following method return?](#q6)
- [Q7: What can prevent execution of a code in a finally block?](#q7)

---

## 🔹 Q1: In Java, what purpose does the key words final, finally, and finalize fulfill?

**Answer:**

‘final ‘ makes a variable reference not changeable, makes a method not overridable, and makes a class not inheritable.
‘finally ‘ is used in a try/catch statement to almost always execute the code. Even when an exception is thrown, the finally block is executed. This is used to
close non-memory resources like file handles, sockets, database connections, etc till Java 7. This is is no longer true in Java 7.
Java 7 has introduced the AutoCloseable interface to avoid the unsightly try/catch/finally(within finally try/catch) blocks to close a resource. It also prevents
potential resource leaks due to not properly closing a resource.
//Pre Java 7
BufferedReader br = null;
ry {
 File f = new File("c://temp/simple.txt" );
 InputStream is = new FileInputStream (f);
 InputStreamReader isr = new InputStreamReader (is);
 br = new BufferedReader (isr);
 String read;
 while ((read = br.readLine ()) != null) {
 System.out.println (read);
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

Java 7 – try can have AutoCloseble types. InputStr eam and OutputStr eam classes now implements the Autocloseable interface.
try can now have multiple statements in the parenthesis and each statement should create an object which implements the new java.lang.AutoClosable interface.
The AutoClosable interface consists of just one method. void close() throws Exception {}. Each AutoClosable resource created in the try statement will be
automatically closed without requiring a finally block. If an exception is thrown in the try block and another Exception is thrown while closing the resource, the
first Exception is the one eventually thrown to the caller. Think of the close( ) method as implicitly being called as the last line in the try block. If using Java 7 or
later editions, use AutoCloseable statements within the try block for more concise & readable code.
‘finalize ‘ is called when an object is garbage collected. You rarely need to override it. It should not be used to release non-memory resources like file handles,
sockets, database connections, etc because Java has only a finite number of these resources and you do not know when the Garbage Collection (i.e. GC) is
going to kick in to release these non-memory resources through the finalize( ) method.

---

## 🔹 Q2: What is the difference between ‘ final ‘ and ‘ const ‘ modifiers on a variable?

**Answer:**

This is a bit of a tricky question because the ‘ const ‘ is a reserved keyword in Java, but not used. In C++ it means a variable is a constant(i.e. its values
cannot be changed).
final
Final modifier on a reference variable just means that the reference cannot be changed to reference a different object once assigned. This does not mean that the
variable is a constant because the values of the object it refers to can be modified unless the values themselves are marked as final. For example, an Employeery (InputStream is = new FileInputStream (new File("c://temp/simple.txt" ));
nputStreamReader isr = new InputStreamReader (is);
BufferedReader br2 = new BufferedReader (isr);) {
 String read;
 while ((read = br2.readLine ()) != null) {
 System.out.println (read);
 }
catch (IOException ioe) {
 ioe.printStackT race();

object marked as final may have member variables “firstName”, and “lastName”. The values of “firstName”, and “lastName” can be modified if they
themselves are not marked as final.
const
is a reserved keyword in Java to make a variable constant, so that the referenced object values also cannot be modified, but it is currently not used in Java. The
compiler will complain if you use it.

---

## 🔹 Q3: What is a volatile key word in Java?

**Answer:**

The volatile keyword is used with object and primitive variable references to indicate that a variable’ s value will be modified by different threads.
Volatile
means
The value of this variable will never be cached locally within the thread, and all the reads and writes must go to the main memory to be visible to the
other threads. In other words the keyword volatile guarantees visibility .
From JDK 5 onwards, writing to a volatile variable happens before reading from a volatile variable. In other words, the volatile keyword guarantees
ordering, and prevents compiler or JVM from reordering of the code.

---

## 🔹 Q4: How does volatile keyword differ from the synchr onized keyword?

**Answer:**

1. The volatile keyword is applied to variables of both primitives and objects, whereas the synchronized keyword is applied to only objects. final Employee emp1 = new Employee ("John", "Peter" );
 emp1 = new Employee (); // compile-time error. object reference cannot be modified
 //if 'firstName' variable in Employee class is not marked as final
 emp1 .setFirstName ("Simon" ); //Line A - firstName value can be modified
 //if 'firstName' itself marked as final then, Line A will throw a compile-time error 

2. The volatile keyword only guarantees visibility and ordering, but not atomicity, whereas the synchronized keyword can guarantee both visibility and
atomicity if done properly. So, the volatile variable has a limited use, and cannot be used in compound operations like incrementing a variable.
Wrong use of volatile in a compound operation
Right use of volatile. Example1 :
Or in lazy singleton. Example2 : Double checked lockingvolatile int counter = 0;
public void increment (){
 counter ++;
}
volatile boolean status = false ;
/...
public void process (){
 while (!status ){
 //....
 }

Important : Synchronized keyword (i.e. locking) can guarantee both visibility and atomicity, whereas volatile variables can only guarantee visibility. A
synchronized block can be used in place of volatile but the inverse is not true.
So, if you are not sure where to use, then use the “synchronized” keyword, and stay clear of the volatile modifier. You can learn more in detail at “ 10+
Atomicity, Visibility, and Ordering interview Q&As on Java Memory Model (JMM) to understand multi-threading ”

---

## 🔹 Q5: What is a “transient” modifier? Can you mark a static variable as transient?

**Answer:**

It marks a member variable not to be serialized when it is persisted to streams of bytes. It cannot be used with a static variable as a static variable belongs to
a class, not to an object. You can only serialize an object.
Transient
Serialization converts an object state to serial bytes (i.e. flattening an object). Those bytes are sent over the network and the object is recreated from those bytes.
Member variables marked by the java transient keyword are not transferred over the wire. A “File” object cannot be serialized.public final Class MySingleton {
 private static volatile MySingleton instance = null;
 private MySingleton ( ){}
 public static MySingleton getInstance () {
 if(instance == null) {
 synchronized (MySingleton .class ) {
 if(instance == null) {
 instance = new MySingleton ();
 }
 }
 }
 return instance ;
 }

Non memory objects like sockets, file handles, etc cannot be serialized, hence mark them as “transient”.
Note : @Transient annotation suggests that the object should not be persisted in Hibernate.

---

## 🔹 Q6: What value will the following method return?

**Answer:**

1 is returned because ‘finally’ has the right to override any exception/returned value by the try ..catch block. It is a bad practice to return from a finally block
as it can suppress any exceptions thrown from a try ..catch block. For example, the following code will not throw an exception.final class SerializeExample implements Serializable {
transient File f;
public Ser() throws FileNotFoundException {
 f = new File("c:\\temp\\filename" );
}
}
public static int getSomeNumber ( ){ 
 try{ 
 return 2; 
 } finally { 
 return 1; 
 } 
} 
public static int getSomeNumber ( ){ 
ry{

---

## 🔹 Q7: What can prevent execution of a code in a finally block?

**Answer:**

a) An end-less loop.
b) System.exit(1) statement.throw new RuntimeException ( ); 
} finally { 
return 12; 
} 
}
public static void main (String [ ] args) { 
ry { 
System.out.println ("This line is printed ....." ); 
//endless loop 
while (true){ 
 //... 
} 
 
finally { 
System.out.println ("Finally block is reached." ); // won't reach 

public class Temp { 
 public static void main (String [ ] args) { 
 try { 

c) Thread death or turning of f the power to CPU.
d) An exception arising in a finally block itself.
e) Process p = Runtime.getRuntime( ).exec(“”);
If using Java 7 or later editions, use AutoCloseable statements within the try block.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » System.out.println ("This line is printed ....." ); 
 System .exit(1); 
 } 
 finally { 
 System.out.println ("Finally block is reached." );// won't reach 
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