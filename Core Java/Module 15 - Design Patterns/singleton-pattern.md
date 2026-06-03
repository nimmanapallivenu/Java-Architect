# Singleton Pattern

> **Module**: Design Patterns  
> **Topic**: Singleton Pattern

---

## 📋 Table of Contents



- [Q1: What is a singleton design pattern?](#q1)
- [Q2: What are the different ways to create a singleton?](#q2)
- [Q3: What is a per thread singleton? How will you create one?](#q3)
- [Q4: What are the downsides of not prperly creating a singleton?](#q4)
- [Q5: Can you explain the statement that “Spring” or “DI” reduces the number of Single](#q5)

---

## 🔹 Q1: What is a singleton design pattern?

**Answer:**

The Singleton pattern is simple, but it can be deceptive as you need to understand all it nuances. Singleton means single instance of an object per JVM per
class loader. The window managers, print spoolers, filesystems, the factory classes to create objects, the cache managers, and the objects you create via the
Spring framework by dafault are all singleton objects.

---

## 🔹 Q2: What are the different ways to create a singleton?

**Answer:**

1. Eager intitialization
The “instance” is created as part of the “MySingleton” class initialization even before the “instance” is required by invoking the method “getInstance()”.
Every time you call “getInstance()”, same instance “instance” is retuned.public class MySingleton {
 private static volatile MySingleton instance = new MySingleton ();
 // private constructor
 private MySingleton () {
 public static MySingleton getInstance () {
 return instance ;
 }

2. Lazy initialization
Create it only when it is required for the first time.
On first invocation of “getInstance”, it will check if instance is already created. If there is no instance, it will create an instance and will return its reference. If
instance is already created, it will simply return the reference of instance.
Q. Why are we checking “instance == null” twice?
A. What if two threads “thread1” and “thread2” enter concurrently the getInstance(), and execute “instance==null”, now both threads have identified instance
variable to null thus end up creating two instances. T wo ways to overcome this issie.
1) Mark the whole method as “synchronized”. This is known as a fat lovk or a coarse gained locking.
2) Have a fine grained block level synchronization, but you need to have “ double checking ” as showen above and the “instance” varaible must be marked
“volatile” to guarantee visibility and ordering. This pattern is known as the “ Double checked locking ” pattern.public class MySingleton {
 private volatile MySingleton instance = null; //volatile modifier is used
 // private constructor
 private MySingleton () {
 }
 public static MySingleton getInstance () {
 if (instance == null) {
 synchronized (MySingleton .class ) {
 // Double check
 if (instance == null) {
 instance = new MySingleton ();
 }
 }
 }
 return instance ;
 }

Q. Why are we using the volatile modifier?
A. The volatile keyword only guarantees visibility and ordering, but not atomicity, whereas the synchronized keyword can guarantee both visibility and
atomicity if done properly. So, the volatile variable has a limited use, and cannot be used in compound operations like incrementing a variable. Learn more
about Atomicity, Visibility, and Ordering in Java multithreading .
3. Static block singleton
Initialized when the class is loaded by the class loader. Even before a constructor is loaded.
4. Enum singleton
As per JavaDoc for Enum: implicit support for thread safety and only one instance is guaranteed.public class MySingleton {
 private static final MySingleton INST ANCE ;
 //initialized when the class is loaded by the class loader
 //even before a constructor is loaded
 static {
 try {
 INST ANCE = new MySingleton ();
 } catch (Exception e) {
 throw new RuntimeException ("Error loading!", e);
 }
 }
 
 private MySingleton () {
 // ...
 }
 public static MySingleton getInstance () {
 return INST ANCE ;
 }

5. Via Spring IoC – default is a singleton
Note that the scope is “singleton”. If you don’ t specify the scope attribute attribut, by default it is “singleton.”
Q. What if “CourseServiceImpl” is not thread-safe? Say it has a shared instance variable.
A. You need to define the scope as “prototype as shown below to be thread-safe. Each thread will work on its own instances of “CourseServiceImpl”, but the
“CourseDaoImpl” will be using the same single instance.public enum MySingleton {
 INST ANCE ;
} 
<?xml version ="1.0" encoding ="UTF-8" ?>
<beans xmlns ="http://www .springframework.or g/schema/beans"
 xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://www .springframework.or g/schema/beans
 http://www .springframework.or g/schema/beans/spring-beans-3.0.xsd" >
 <bean id="courseDao" class ="com.mytutorial.dao.CourseDaoImpl" scope ="singleton" />
 
 <bean id="courseService" class ="com.mytutorial.service.CourseServiceImpl" scope ="singleton" >
 <constructor -arg name ="dao" ref="courseDao" />
 </bean >
</beans >
<?xml version ="1.0" encoding ="UTF-8" ?>

---

## 🔹 Q3: What is a per thread singleton? How will you create one?

**Answer:**

Here is an example with the ThreadLocal class to create per thread heavy object applying the abstract factory and singleton design patterns.<beans xmlns ="http://www .springframework.or g/schema/beans"
 xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://www .springframework.or g/schema/beans
 http://www .springframework.or g/schema/beans/spring-beans-3.0.xsd" >
 <bean id="courseDao" class ="com.mytutorial.dao.CourseDaoImpl" scope ="singleton" />
 
 <bean id="courseService" class ="com.mytutorial.service.CourseServiceImpl" scope ="prototype" >
 <constructor -arg name ="dao" ref="courseDao" />
 </bean >
</beans >
/final class -- don't let outsiders extend
public final class CalendarFactory {
private ThreadLocal <calendar > calendarRef = new ThreadLocal <calendar >() {
 protected Calendar initialV alue() {
 return new GregorianCalendar ();
 }
};
private static CalendarFactory instance = new CalendarFactory ();
public static CalendarFactory getFactory () {
 return instance ;
}
public Calendar getCalendar () {
 return calendarRef .get();
}
// Don't let outsiders create new factories directly
private CalendarFactory () {}

---

## 🔹 Q4: What are the downsides of not prperly creating a singleton?

**Answer:**

Thread safety and memory leak issues.
Q. How do you get a memory leak issue in singletons?
A. Long living objects having reference to short living objects, causing the memory to slowly grow. For example, singleton classes referring to short lived
objects. This prevents short-lived objects being garbage collected.

---

## 🔹 Q5: Can you explain the statement that “Spring” or “DI” reduces the number of Singletons needed to be created?

**Answer:**

Spring is an IoC container for “Dependency Injection” (i.e. DI). If you are not using DI, you will end up creating factory classes as singletons to loosely
couple your caller and the callee. The caller will get the callee via a factory class. The factory class is responsible for creating an object and returning it to the
callee. This will create the necessity for lots of singletons. But, if you use DI, you don’ t have to create singletons or factory classes for this purpose of loose
coupling.
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