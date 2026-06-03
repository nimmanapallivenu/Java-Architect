# 11 6 must know Java modifiers interview questions and answers

## Table of Contents

- [Q1: What is the dif ference between ‘ final ‘ and ‘ const ‘ modifiers on a variable?](#q1)
- [Q2: What is a volatile key word in Java?](#q2)
- [Q3: How does volatile keyword dif fer from the synchr onized keyword?](#q3)
- [Q4: Why is the modifier used for thread-safety is called “synchronized”, and NOT “lo](#q4)
- [Q5: What is a “transient” modifier? Can you mark a static variable as transient?](#q5)
- [Q6: In Java, what purpose does the key words final , finally , and finalize fulfill?](#q6)
- [Q7: What value will the following method return?ry (InputStream is = new FileInputSt](#q7)
- [Q8: What can prevent execution of a code in a finally block?](#q8)

---

## Q1: What is the dif ference between ‘ final ‘ and ‘ const ‘ modifiers on a variable?

**Answer:**

This is a bit of a tricky question because the ‘ const ‘ is a reserved keyword in Java, but not used. In C++ it means a variable is a constant(i.e. its values cannot be
changed).
1. final
The final keyword in Java can be used in many contexts – variable, method, and class. If you make any method as final, you cannot override it . If you make any class as final,
you cannot extend it .
Q. Can you inherit a final method in a subclass?
A. Yes. But can’ t override it.
Final modifier on a reference variable just means that the reference cannot be changed to reference a dif ferent object once assigned. This does not mean that the variable is a
constant because the values of the object it refers to can be modified unless the values themselves are marked as final . For example, an Employee object marked as final may
have member variables “firstName”, and “lastName”. The values of “firstName”, and “lastName” can be modified if they themselves are not marked as final.
2. const
is a reserved keyword in Java to make a variable constant, so that the referenced object values also cannot be modified, but it is currently not used in Java. The compiler will
complain if you use it.

---

## Q2: What is a volatile key word in Java?

**Answer:**

The volatile keyword is used with object and primitive variable references to indicate that a variable’ s value will be modified by dif ferent threads. final Employee emp1 = new Employee ("John" , "Peter" );
 emp1 = new Employee (); // compile-time error . object reference cannot be modified
 //if 'firstName' variable in Employee class is not marked as final
 emp1 .setFirstName ("Simon" ); //Line A - firstName value can be modified
 //if 'firstName' itself marked as final then, Line A will throw a compile-time error 

3. Volatile
means
The value of this variable will never be cached locally within the thread, and all the reads and writes must go to the main memory to be visible to the other threads . In
other words the keyword volatile guarantees visibility .
From JDK 5 onwards , writing to a volatile variable happens before reading from a volatile variable. In other words, the volatile keyword guarantees ordering , and
prevents compiler or JVM from reordering of the code.

---

## Q3: How does volatile keyword dif fer from the synchr onized keyword?

**Answer:**

1. The volatile keyword is applied to variables of both primitives and objects , whereas the synchronized keyword is applied to objects, methods, and code blocks.
2. The volatile keyword only guarantees visibility and ordering , but not atomicity , whereas the synchronized keyword can guarantee both visibility and atomicity if
done properly . So, the volatile variable has a limited use, and cannot be used in compound operations like incrementing a variable.
Wrong use of volatile in a compound operation
Right use of volatile. Example1 :volatile int counter = 0;
public void increment (){
 counter ++;
}
volatile boolean status = false ;
/...
public void process (){
 while (!status ){
 //....

Or in lazy singleton. Example2 : Double checked locking
Important : Synchronized keyword (i.e. locking) can guarantee both visibility and atomicity , whereas volatile variables can only guarantee visibility . A synchronized block
can be used in place of volatile but the inverse is not true.
Thread-safe eager or early loaded singleton: }
public final Class MySingleton {
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
public final Class MySingleton {
 private static MySingleton instance = new MySingleton (); //loaded early or eagerly

You can learn more in detail at “ 10+ Atomicity , Visibility , and Ordering interview Q&As on Java Memory Model (JMM) to understand multi-threading ”

---

## Q4: Why is the modifier used for thread-safety is called “synchronized”, and NOT “locked”?

**Answer:**

When a method or block of code is locked with the modifier “synchronized”, the memory (i.e. heap) where the shared data is kept is synchronized. This means, private MySingleton ( ){}
 // the method which gives access to the only instance of MySingleton
 public static MySingleton getInstance () {
 return instance ;
 }

JVM memory Vs. physical memory
When a synchronized block of code or method is entered after the lock has been acquired by a thread, it first reads (i.e. synchronizes ) any changes to the locked object from
the main heap memory to ensure that the thread that has the lock has the current info before start executing.
After the synchronized block has completed and the thread is ready to relinquish the lock, all the changes that were made to the object that was locked is written or flushed
back (i.e. synchronized ) to the main heap memory so that the other threads that acquire the lock next has the current info.
This is why it is called “synchronized” and not “locked”. This is also the reason why the immutable objects are inherently thread-safe and does not require any
synchronization. Once created, the immutable objects cannot be modified.
You can learn more in detail at “ 10+ Java Memory Model (JMM) interview Q&As to understand multi-threading ”

---

## Q5: What is a “transient” modifier? Can you mark a static variable as transient?

**Answer:**

It marks a member variable not to be serialized when it is persisted to streams of bytes. It cannot be used with a static variable as a static variable belongs to a class, not
to an object. Y ou can only serialize an object.
4. Transient
Serialization converts an object state to serial bytes (i.e. flattening an object). Those bytes are sent over the network and the object is recreated from those bytes. Member
variables marked by the java transient keyword are not transferred over the wire. A “File” object cannot be serialized.
Non memory objects like sockets, file handles, etc cannot be serialized, hence mark them as “transient”.
Note : @Transient annotation suggests that the object should not be persisted in Hibernate.

---

## Q6: In Java, what purpose does the key words final , finally , and finalize fulfill?

**Answer:**

‘final ‘ makes a variable reference not changeable, makes a method not overridable, and makes a class not inheritable.
5. finallyfinal class SerializeExample implements Serializable {
transient File f;
public Ser() throws FileNotFoundException {
 f = new File("c:\\temp\\filename" );
}
}

‘finally ‘ is used in a try/catch statement to almost always execute the code. Even when an exception is thrown, the finally block is executed. This is used to close non-memory
resources like file handles, sockets, database connections, etc till Java 7. This is is no longer true in Java 7.
Java 7 has introduced the AutoCloseable interface to avoid the unsightly try/catch/finally(within finally try/catch) blocks to close a resource. It also prevents potential
resource leaks due to not properly closing a resource.
//Pre Java 7
Java 7 – try can have AutoCloseble types. InputStr eam and OutputStr eam classes now implements the Autocloseable interface.BufferedReader br = null;
ry {
 File f = new File("c://temp/simple.txt" );
 InputStream is = new FileInputStream (f);
 InputStreamReader isr = new InputStreamReader (is);
 br = new BufferedReader (isr);
 String read;
 while ((read = br.readLine ()) != null) {
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

try can now have multiple statements in the parenthesis and each statement should create an object which implements the new java.lang.AutoClosable interface. The
AutoClosable interface consists of just one method. void close() throws Exception {}. Each AutoClosable resource created in the try statement will be automatically closed
without requiring a finally block. If an exception is thrown in the try block and another Exception is thrown while closing the resource, the first Exception is the one eventually
thrown to the caller . Think of the close( ) method as implicitly being called as the last line in the try block. If using Java 7 or later editions, use AutoCloseable statements
within the try block for more concise & readable code.
6. finalize
‘finalize ‘ is called when an object is garbage collected. Y ou rarely need to override it. It should not be used to release non-memory resources like file handles, sockets,
database connections, etc because Java has only a finite number of these resources and you do not know when the Garbage Collection (i.e. GC) is going to kick in to
release these non-memory resources through the finalize( ) method.
[ Further reading: 9 Java Garbage Collection interview Q&As to ascertain your depth of Java knowledge ]
So, final and finally are used very frequently in your Java code, but the key word finalize is hardly or never used.
final

---

## Q7: What value will the following method return?ry (InputStream is = new FileInputStream (new File("c://temp/simple.txt" ));
nputStreamReader isr = new InputStreamReader (is);
BufferedReader br2 = new BufferedReader (isr);) {
 String read;
 while ((read = br2.readLine ()) != null) {
 System .out.println (read);
 }
catch (IOException ioe) {
 ioe.printStackT race();
public static int getSomeNumber ( ){ 
 try{ 
 return 2;

**Answer:**

1 is returned because ‘finally’ has the right to override any exception/returned value by the try ..catch block. It is a bad practice to return from a finally block as it can
suppress any exceptions thrown from a try ..catch block. For example, the following code will not throw an exception.

---

## Q8: What can prevent execution of a code in a finally block?

**Answer:**

a) An end-less loop. } finally { 
 return 1; 
 } 
} 
public static int getSomeNumber ( ){ 
ry{ 
throw new RuntimeException ( ); 
} finally { 
return 12; 
} 
}
public static void main (String [ ] args) { 
ry { 
System .out.println ("This line is printed ....." ); 
//endless loop 
while (true){ 
 //... 
} 
 
finally { 
System .out.println ("Finally block is reached." ); // won't reach 

b) System.exit(1) statement.
c) Thread death or turning of f the power to CPU.
d) An exception arising in a finally block itself.
e) Process p = Runtime.getRuntime( ).exec(“”);
If using Java 7 or later editions, use AutoCloseable statements within the try block.public class Temp { 
 public static void main (String [ ] args) { 
 try { 
 System .out.println ("This line is printed ....." ); 
 System .exit(1); 
 } 
 finally { 
 System .out.println ("Finally block is reached." );// won't reach 
 } 
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
