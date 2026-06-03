# Annotation Types & Processing

> **Module**: Modifiers Annotations Initializers  
> **Topic**: Annotation Types & Processing

---

## 📋 Table of Contents



- [Q1: What do you understand by the terms annotation types, annotations, and annotatio](#q1)
- [Q2: Can you describe the ways in which annotations can be used at pre-compile time, ](#q2)
- [Q3: What do you understand by annotation of annotation? How do you control when an a](#q3)
- [Q4: What are the different annotation types?](#q4)

---

## 🔹 Q1: What do you understand by the terms annotation types, annotations, and annotation processors?

**Answer:**

An annotation type is used for defining an annotation. Java 5 defines a number of annotation types like @Override, @SuppressW arnings, @Deprecated,
etc and meta annotation types that are used by annotation (i.e. a meta meta data) type like @T arget, @RetntionPolicy, @Inherited, and @Documented. For
example,
As you can see the difference between an interface definition and that of an annotation is the presence of @ before the interface keyword. Here are some of the
rules for defining an annotation:
– Method declarations inside an annotation should not have any parameters.
– Method declarations should not have any throws clauses.
– Return types of the methods should be primitives, String, Class, enum, or array of the above types.
An annotation is the meta tag that you use in your applications. For example, you can use the annotation types @Override, @Suppresswarnings, @Inherited,
etc included in Java and the custom annotation type @T oDo that was defined above as shown below:@Documented
@Inherited
@Retention (RetentionPolicy .RUNTIME )
@Target({ElementT ype.METHOD ,ElementT ype.TYPE })
public @interface ToDo {
 String comments ();
}
package annotation .example ;
mport java.util.ArrayList ;
mport java.util.List;
@ToDo(comments ="Not yet complete" )
public class MyClass {

Finally, annotating your code alone is not going to give you any functionality apart from some form of documentation unless you have processors that process
the annotations in special way to add behavior. The processors can be Java compiler itself, tools that are shipped with Java itself like Javadoc, apt (Annotation
Processing T ool), Java Runtime, Java IDEs like eclipse, Net Beans, etc and frameworks like Hibernate 3.0 and Spring 3.0, JEE CDI, etc.
1) @Override and @Suppr essW arnings are used by the Java compiler .
2) The annotation @Depr ecated is used by the Java compiler and the IDEs like eclipse.
3) The custom annotation @ToDo can be used at runtime to produce a summary report as a to do list by querying the annotations at runtime using the Java
reflection API. @Deprecated
 public void doSomething () {
 // some logic
 }
 @SuppressW arnings (value = "unchecked" )
 @ToDo(comments ="Need to confirm with legacy app" )
 public void doSomethingBetter () {
 List vocabulary = new ArrayList ();
 vocabulary .add("deliberate" ); 
 }
 
 @Override
 public String toString () {
 return super .toString ();
 }
 
 @Override
 public int hashCode () {
 return super .hashCode ();
 }
package annotation .example ;
mport java.lang.annotation .Annotation ;

---

## 🔹 Q2: Can you describe the ways in which annotations can be used at pre-compile time, compile time, post-compile time and runtime?

**Answer:**

Pre compile-time: You can generate additional boiler plate source code and descriptor files using tools like apt (Annotation Processing T ool) during the
build process. For example, a service framework can be developed using annotations where a developer provides a delegate class say UserDelegate with the
required business logic and relevant annotations. The service framework will make use of the apt tool to read this delegate class and produce additional artifacts
required to expose the business functions via RMI (using EJBs) and Web Services. The apt tool can be used to generate the required source files like local
interfaces, remote interfaces, wrapper implementation to expose the service as RMI and Web Service during build time (i.e. prior to compiling). The annotation
processing tool is very powerful.
Compile-time: By the Java compiler and IDEs to raise errors and warnings during compiling source code into byte code (i.e. .class files) as discussed earlier .
Post Compile-time: Annotations can be scanned on byte code files (i.e. .class files) using byte code processing libraries like Javaassist or ASM. Javaassist does
have reflection like API that allows you iterate over methods and fields of a class file. You can read your class files from the InputStreams from your classpath
or .jar. Based on annotations, additional code can be injected or existing code can be modified. Any form of byte code manipulation can make your code harder
to read or understand. Don’ t favor this approach unless you have a compelling reason to do so. For example, performance considerations associated with
scanning for all the annotations by loading each and every class using your Class loader and Java reflection.
Runtime: By the application itself and other frameworks to check the validity of the input passed by the clients and extract program behaviors at runtime using
Java reflection. The Java reflection API has been updated with facility to work with annotations since JDK 5.0.

---

## 🔹 Q3: What do you understand by annotation of annotation? How do you control when an annotation is need and where an annotation should go?

**Answer:**

JDK 5.0 provides four annotations in the java.lang.annotation package that are used only when writing annotations.
@Documented → Should the annotation be in Javadoc? Annotations on a class or method don’ t appear in the Javadocs by default. The @Documented is a
marker annotation (i.e. accepts no parameters) that changes this behavior .
@Retention → When the annotation is needed? There are three options as listed in the RetentionPolicy enumeration.public class QueryAnnotation {
 public static void main (String [] args) {
 Annotation [] typeAnnotations = MyClass .class .getAnnotations ();
 for (Annotation annotation : typeAnnotations ) {
 if (annotation .annotationT ype().getSimpleName ().equals (
 ToDo.class .getSimpleName ())) {
 System.out.println (" --> Comments: "
 + ((ToDo) annotation ).comments ());
 }
 }
 }

@Target → Where the annotation can go? You have eight options listed in the ElementT ype enumeration to tell where a particular annotation can be applied.
ElementT ype.TYPE (class, interface, enum)
ElementT ype.FIELD (instance variable)
ElementT ype.METHOD
ElementT ype.P ARAMETER
ElementT ype.CONSTRUCT OR
ElementT ype.LOCAL_V ARIABLE
ElementT ype.annotation_TYPE (on another annotation)
ElementT ype.P ACKAGE
@Inherited → Should subclasses get the annotation? This controls if an annotation should af fect subclasses. If you look at the earlier examples MyClass base
class and @T oDo annotation, the @T oDo annotation is annotated with @Inherited.

---

## 🔹 Q4: What are the different annotation types?

**Answer:**

1) Marker annotation.
2) Single element annotation.

3) Full value or multi-value annotation.
Marker type annotations have no elements, except the annotation name itself.
Single Element or single value type annotations provide a single piece of data only .
Usage:@Target(ElementT ype.METHOD )
@Retention (RetentionPolicy .SOURCE )
public @interface Override {
}
@Documented
@Inherited
@Retention (RetentionPolicy .SOURCE )
@Target({ElementT ype.METHOD ,ElementT ype.TYPE })
public @interface ThreadSafe {
 //default makes it optional
 String value () default "";
}
@ThreadSafe ("not using any instance variables" )
public void method1 (){
 //...
}

Full-value or multi-value type annotations have multiple data members.
Usage:
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »package javax .ejb;
mport java.lang.annotation .Retention ;
mport java.lang.annotation .Target ;
@Target (ElementT ype.TYPE )
@Retention (RetentionPolicy .RUNTIME )
public @interface Stateless {
 String name () default "";
 String mappedName () default "";
 String description () default "";
@Stateless (name ="Char ging", description ="Char ging Service" )
@TransactionManagement
 (TransactionManagementT ype.CONT AINER )
@TransactionAttribute (TransactionAttributeT ype.NEVER )
public class Char gingDAOImpl extends MediationDAO implements 
 Char gingDAO {
 // fields and methods
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