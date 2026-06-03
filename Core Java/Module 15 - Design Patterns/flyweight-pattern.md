# Flyweight Pattern

> **Module**: Design Patterns  
> **Topic**: Flyweight Pattern

---

## 📋 Table of Contents



- [Q1: Can you give some examples of the usage of the flyweight design pattern in Java?](#q1)
- [Q2: How will you apply this pattern in the following scenario?
You have a scheduling](#q2)
- [Q3: Are there any alternatives to using an object or resource pool to conserve memor](#q3)

---

## 🔹 Q1: Can you give some examples of the usage of the flyweight design pattern in Java?

**Answer:**

Example 1:
In Java, String objects are managed as flyweight. Java puts all fixed String literals into a literal pool. For redundant literals, Java keeps only one copy in the
pool.
The above code snippet will print “referencing the same object”. Even though the two String objects are created separately, under the covers Java is storing them
in the same location, to save space by applying the flyweight design pattern.
Example 2:
The Wrapper classes like Integer, Float, Decimal, Boolean, and many other classes having the valueOf static factory method applies the flyweight design
pattern to conserve memory by reusing the objects.String author = "Little brown fox" ;
String authorCopy = "Little brown fox" ;
if(author == authorCopy ) {
 System.out.println ("referencing the same object" );
}

The above code snippet will print “ referencing the same object “.

---

## 🔹 Q2: How will you apply this pattern in the following scenario?
You have a scheduling application where a number of different time slots are used in 15 minute intervals in various online reporting. It contains hour and minute
slots as in 10:15, 10:30, etc. The objects need to be immutable and reusable.

**Answer:**

Here is the sample code that makes use of the getInstance(…) static factory method to create objects.public class FlyWeightW rapper {
public static void main (String [] args) {
 Integer value1 = Integer .valueOf (5);
 Integer value2 = Integer .valueOf (5);
 if (value1 == value2 ) {
 System.out.println ("referencing the same object" );
 }
}
mport java.util.Map;
mport java.util.concurrent .ConcurrentHashMap ;
/no setters, immutable object
/final class -- don't let outsiders extend
public final class Schedule {
 private byte hour;
 private byte minute ;
 private static Map<String ,Schedule > schedules = new ConcurrentHashMap <String ,Schedule >();
 // Don't let outsiders create new factories directly
 // can be invoked only via getInstance (byte hour, byte minute) method
 private Schedule (byte hour, byte minute ) {
 this.hour = hour;
 this.minute = minute ;

---

## 🔹 Q3: Are there any alternatives to using an object or resource pool to conserve memory in Java?

**Answer:**

Yes, you can use a ThreadLocal object to create an object per thread. This approach is useful when creation of a particular object is not trivial and the
objects cannot be shared between threads. For example, java.util.Calendar and java.text.SimpleDateFormat. Because these are heavy objects that often need to
be set up with a format or locale, it’ s very tempting to create it with a static initializer and stick the instance in a static field. Both of these classes use internal
mutable state when doing date calculations or formatting/parsing dates. If they are called from multiple threads at the same time, the internal mutable state will
most likely do unexpected things and give you wrong answers. In simple terms, this will cause thread-safety issues that can be very hard to debug.
Here is an example with the ThreadLocal class to create per thread heavy object applying the abstract factory and singleton design patterns. }
 public byte getHour () {
 return hour;
 }
 public byte getMinute () {
 return minute ;
 }
 public static Schedule getInstance (byte hour, byte minute ) {
 String key = hour + ":" + minute ;
 //check the object pool first
 Schedule schedule = schedules .get(key);
 //if not found in the pool, create a new instance
 if (schedule == null) {
 schedule = new Schedule (hour, minute );
 // add it to the pool, for later reuse
 schedules .put(key, schedule );
 }
 return schedule ;
 }
/final class -- don't let outsiders extend
public final class CalendarFactory {

Another alternative is to create immutable objects as they are inherently thread-safe, and can be shared with multiple threads.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit //one object per thread storage 
 private ThreadLocal <calendar > calendarRef = new ThreadLocal <calendar >() {
 protected Calendar initialV alue() {
 return new GregorianCalendar ();
 }
 };
 //singleton. one instance per "JVM class loader"
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