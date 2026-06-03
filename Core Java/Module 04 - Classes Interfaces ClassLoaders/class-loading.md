# Class Loading Mechanism

> **Module**: Classes Interfaces ClassLoaders  
> **Topic**: Class Loading Mechanism

---

## 📋 Table of Contents



- [Q1: What do you know about Java class loading? Explain Java class loaders?](#q1)
- [Q2: Explain static vs. dynamic class loading?](#q2)
- [Q3: What tips would you give to someone who is experiencing a class loading or “Clas](#q3)

---

## 🔹 Q1: What do you know about Java class loading? Explain Java class loaders?

**Answer:**

Class loaders are hierarchical. Classes are introduced into the JVM as they are referenced by name in a class that is already running in the JVM. So, how is
the very first class loaded? The very first class is specially loaded with the help of static main( ) method declared in your class. All the subsequently loaded
classes are loaded by the classes, which are already loaded and running. A class loader creates a namespace. All JVMs include at least one class loader that is
embedded within the JVM called the primordial (or bootstrap) class loader. The JVM has hooks in it to allow user defined class loaders to be used in place of
primordial class loader. Let us look at the class loaders created by the JVM.

Java Class Loader Basics
Class loaders are hierarchical and use a delegation model when loading a class. Class loaders request their parent to load the class first before attempting to load
it themselves. When a class loader loads a class, the child class loaders in the hierarchy will never reload the class again. Hence uniqueness is maintained.
Classes loaded by a child class loader have visibility into classes loaded by its parents up the hierarchy but the reverse is not true as explained in the above
diagram.

---

## 🔹 Q2: Explain static vs. dynamic class loading?

**Answer:**

Classes are statically loaded with Java’ s “new” operator .
Dynamic loading is a technique for programmatically invoking the functions of a class loader at run time. Let us look at how to load classes dynamically .
The above static method returns the class object associated with the class name. The string className can be supplied dynamically at run time. Unlike the static
loading, the dynamic loading will decide whether to load the class Car or the class Jeep at runtime based on a properties file and/or other runtime conditions.
Once the class is dynamically loaded the following method returns an instance of the loaded class. It’ s just like creating a class object with no arguments.class MyClass {
 public static void main (String args[]) {
 Car c = new Car( );
 }
}
/static method which returns a Class
Class .forName (String className );

Static class loading throws “ NoClassDefFoundErr or” if the class is not found and the dynamic class loading throws “ ClassNotFoundException ” if the class
is not found.
Q. What is the difference between the following approaches?
and
A.
Class.forName(“com.SomeClass”)
— Uses the caller ’s classloader and initializes the class (runs static intitializers, etc.)
classLoader.loadClass(“com.SomeClass”)/ A non-static method, which creates an instance of a
/ class (i.e. creates an object).
class .newInstance ( );
Class .forName ("com.SomeClass" );
classLoader .loadClass ("com.SomeClass" );

— Uses the “supplied class loader”, and initializes the class lazily (i.e. on first use). So, if you use this way to load a JDBC driver, it won’ t get registered, and
you won’ t be able to use JDBC.
The “java.lang.API” has a method signature that takes a boolean flag indicating whether to initialize the class on loading or not, and a class loader reference.
So, invoking
is same as invoking
Q. What are the different ways to create a “ClassLoader” object?
A.forName (String name, boolean initialize, ClassLoader loader )
Class .forName ("com.SomeClass" )
forName ("com.SomeClass", true, currentClassLoader )

Q. How to load property file from classpath?
A. getResourceAsStr eam() is the method of java.lang.Class. This method finds the resource by implicitly delegating to this object’ s class loader .
Note: “Try with AutoCloseable resources” syntax introduced with Java 7 is used above.
Q. What is the benefit of loading a property file from classpath?
A. It is portable as your file is relative to the classpath. You can deploy the “jar” file containing your “property” file to any location where the JVM is.
Loading it from outside the classpath is NOT portableClassLoader classLoader = Thread .currentThread ().getContextClassLoader ();
ClassLoader classLoader = MyClass .class .getClassLoader (); // Assuming in class "MyClass"
ClassLoader classLoader = getClass ().getClassLoader (); // works in any class
final Properties properties = new Properties ();
ry (final InputStream stream = this.getClass ().getResourceAsStream ("myapp.properties" )) {
 properties .load(stream );
 /* or properties.loadFromXML(...) */
}
final Properties properties = new Properties ();
final String dir = System .getProperty ("user .dir");
 
ry (final InputStream stream = new FileInputStream (dir + "/myapp/myapp.properties" )) {
 properties .load(stream );

As the above code is NOT portable, you must document very clearly in the installation or deployment document as to where the property file is loaded from
because if you deploy your “jar” file to another location, it might not already have the path “dir” and “myapp” configured.
So, loading it via the classpath is recommended as it is a portable solution.

---

## 🔹 Q3: What tips would you give to someone who is experiencing a class loading or “Class Not Found” exception?

**Answer:**

“ClassNotFoundException” could be quite tricky to troubleshoot. When you get a ClassNotFoundException, it means the JVM has traversed the entire
classpath and not found the class you’ve attempted to reference.
1) Stand alone Java applications use -cp or -classpath to define all the folders and jar files to look for. In windows separated by “;” and in Unix separated by “:”.
You can also search for the class at www .jarfinder .com
3) Check the version of the jar in the manifest file MANIFEST .MF, access rights (e.g. read-only) of the jar file, presence of multiple versions of the same jar file
and any jar corruption by trying to unjar it with “jar -xvf …”. If the class is dynamically loaded with Class.forName(“com.myapp.Util”), check if you have
spelled the class name correctly .
4) Check if the application is running under the right JDK? Check the JA VA_HOME environment property}
ava -classpath "C:/myproject/classes;C:/myproject/lib/my-utility .jar;C:/myproject/lib/my-dep.jar" MyApp 
</code >>/pre>
2) Determine the jar file that should contain the class file within the classpath -- war/ear archives and application server lib directories. Search recursively for the
class .
<pre class ="prettyprint" ><code class ="language-java" >
$ find. -name "*.jar" -print -exec jar -tf '{}' \; | grep -E "jar$|String\.class" 

5) -verbose:class option in your JVM. With the -verbose option all the classes that are loaded are listed, along with the JAR file or directory from which they
were loaded. The “class” output shows additional information, such as when superclasses are being loaded, and when static initializers are being run.
6) Creating a Java dump and analyzing the Java dump for class loading issues. The Java dumps are created under following circumstances.
— When a fatal native JVM error .
— When the JVM runs out of heaps memory space.
— When a signal is sent to the JVM (e.g. Control-Break is pressed on Windows, Control-\ on Linux, or kill -3 on Unix)
There are tools like jstack, jmap, hprof, and Eclipse Memory Analyzer (MA T) to analyze the Java dumps.
7) Some of the libraries provide API to list the version number. For example, The Eclipse link MOXy library provides a method as shown below .
8) The or g.jboss.test.util.Debug class has a method displayClassInfo(Class clazz, StringBuffer r esults) to display the loaded class details. This is done
programmatically. What this class essentially does is$ echo $JAVA_HOME 
 org.eclipse .persistence .Version .getVersion ();
 URL loc = MyClass .class .getProtectionDomain ().getCodeSource ().getLocation ();

9) The http://www .findjar .com is an online search engine that can list possible jar files in which a particular class file like java.sql.Connection can be found.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

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