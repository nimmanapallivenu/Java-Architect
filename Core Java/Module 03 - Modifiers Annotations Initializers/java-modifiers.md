# Java Modifiers

> **Module**: Modifiers Annotations Initializers  
> **Topic**: Java Modifiers - Access, Non-Access, and Special Modifiers

---

## 📋 Table of Contents

- [Access Modifiers](#access-modifiers)
  - [Public](#public)
  - [Protected](#protected)
  - [Default (Package-Private)](#default-package-private)
  - [Private](#private)
  - [Access Modifier Summary](#access-modifier-summary)
- [Non-Access Modifiers](#non-access-modifiers)
  - [Static Modifier](#static-modifier)
  - [Final Modifier](#final-modifier)
  - [Abstract Modifier](#abstract-modifier)
  - [Synchronized Modifier](#synchronized-modifier)
  - [Volatile Modifier](#volatile-modifier)
  - [Transient Modifier](#transient-modifier)
  - [Native Modifier](#native-modifier)
  - [Strictfp Modifier](#strictfp-modifier)
- [Interview Questions](#interview-questions)
  - [Q1: What is the difference between final, finally, and finalize?](#q1-what-is-the-difference-between-final-finally-and-finalize)
  - [Q2: What is the difference between final and const?](#q2-what-is-the-difference-between-final-and-const)
  - [Q3: What is the volatile keyword in Java?](#q3-what-is-the-volatile-keyword-in-java)
  - [Q4: How does volatile differ from synchronized?](#q4-how-does-volatile-differ-from-synchronized)
  - [Q5: What is the transient modifier?](#q5-what-is-the-transient-modifier)
  - [Q6: What value will a finally block return?](#q6-what-value-will-a-finally-block-return)
  - [Q7: What can prevent execution of a finally block?](#q7-what-can-prevent-execution-of-a-finally-block)

---

## Access Modifiers

Access modifiers control the visibility and accessibility of classes, methods, and variables in Java.

### Public

- **Scope**: Accessible from anywhere in the application
- **Usage**: Classes, interfaces, methods, variables
- **Best Practice**: Use for APIs and public interfaces

```java
public class PublicExample {
    public int publicVariable = 10;
    
    public void publicMethod() {
        System.out.println("Accessible from anywhere");
    }
}
```

### Protected

- **Scope**: Accessible within the same package and by subclasses (even in different packages)
- **Usage**: Methods, variables (not top-level classes)
- **Best Practice**: Use for inheritance hierarchies

```java
public class Parent {
    protected int protectedVariable = 20;
    
    protected void protectedMethod() {
        System.out.println("Accessible to subclasses");
    }
}

// In different package
public class Child extends Parent {
    public void accessProtected() {
        System.out.println(protectedVariable); // OK
        protectedMethod(); // OK
    }
}
```

### Default (Package-Private)

- **Scope**: Accessible only within the same package
- **Usage**: Classes, methods, variables (when no modifier is specified)
- **Best Practice**: Use for internal package implementation

```java
class DefaultClass { // package-private
    int defaultVariable = 30;
    
    void defaultMethod() {
        System.out.println("Accessible within package only");
    }
}
```

### Private

- **Scope**: Accessible only within the same class
- **Usage**: Methods, variables, nested classes
- **Best Practice**: Use for encapsulation and information hiding

```java
public class PrivateExample {
    private int privateVariable = 40;
    
    private void privateMethod() {
        System.out.println("Accessible only within this class");
    }
    
    public void publicAccessor() {
        privateMethod(); // OK - same class
    }
}
```

### Access Modifier Summary

| Modifier | Class | Package | Subclass | World |
|----------|-------|---------|----------|-------|
| public | ✓ | ✓ | ✓ | ✓ |
| protected | ✓ | ✓ | ✓ | ✗ |
| default | ✓ | ✓ | ✗ | ✗ |
| private | ✓ | ✗ | ✗ | ✗ |

---

## Non-Access Modifiers

### Static Modifier

The `static` keyword indicates that a member belongs to the class rather than instances of the class.

**Static Variables (Class Variables)**

```java
public class Counter {
    private static int count = 0; // Shared across all instances
    private int instanceId;
    
    public Counter() {
        count++;
        instanceId = count;
    }
    
    public static int getCount() {
        return count;
    }
}

// Usage
Counter c1 = new Counter();
Counter c2 = new Counter();
System.out.println(Counter.getCount()); // 2
```

**Static Methods**

```java
public class MathUtils {
    // Static method - no instance needed
    public static int add(int a, int b) {
        return a + b;
    }
    
    // Cannot access instance variables
    public static void staticMethod() {
        // this.instanceVar; // Compilation error
    }
}

// Usage
int result = MathUtils.add(5, 3); // No object creation needed
```

**Static Blocks**

```java
public class Configuration {
    private static Properties props;
    
    // Static initialization block
    static {
        props = new Properties();
        try {
            props.load(new FileInputStream("config.properties"));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**Key Points:**
- Static members are loaded when the class is loaded
- Cannot access instance variables or methods directly
- Can be accessed without creating an object
- Static methods cannot be overridden (they are hidden, not overridden)

---

### Final Modifier

The `final` keyword prevents modification and has different meanings depending on context.

**Final Variables (Constants)**

```java
public class Constants {
    // Compile-time constant
    public static final double PI = 3.14159;
    
    // Runtime constant
    public static final String APP_NAME = System.getProperty("app.name");
    
    // Instance constant - must be initialized in constructor
    private final String id;
    
    public Constants(String id) {
        this.id = id; // Can only be assigned once
    }
}
```

**Final Methods**

```java
public class Parent {
    // Cannot be overridden by subclasses
    public final void finalMethod() {
        System.out.println("This method cannot be overridden");
    }
}

public class Child extends Parent {
    // Compilation error - cannot override final method
    // public void finalMethod() { }
}
```

**Final Classes**

```java
// Cannot be extended
public final class ImmutableClass {
    private final String value;
    
    public ImmutableClass(String value) {
        this.value = value;
    }
    
    public String getValue() {
        return value;
    }
}

// Compilation error - cannot extend final class
// public class SubClass extends ImmutableClass { }
```

**Final with References**

```java
public class FinalReference {
    public static void main(String[] args) {
        final Employee emp = new Employee("John", "Doe");
        
        // Cannot reassign reference
        // emp = new Employee("Jane", "Smith"); // Compilation error
        
        // But can modify object state (if not immutable)
        emp.setFirstName("Simon"); // OK if setter exists
    }
}
```

---

### Abstract Modifier

The `abstract` keyword is used for classes and methods that must be implemented by subclasses.

```java
public abstract class Shape {
    private String color;
    
    // Abstract method - no implementation
    public abstract double calculateArea();
    
    // Concrete method
    public void setColor(String color) {
        this.color = color;
    }
}

public class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}
```

---

### Synchronized Modifier

The `synchronized` keyword ensures thread-safe access to methods or blocks.

```java
public class Counter {
    private int count = 0;
    
    // Synchronized method
    public synchronized void increment() {
        count++;
    }
    
    // Synchronized block
    public void incrementWithBlock() {
        synchronized(this) {
            count++;
        }
    }
    
    // Static synchronized method - locks on class object
    public static synchronized void staticMethod() {
        // Thread-safe static method
    }
}
```

---

### Volatile Modifier

The `volatile` keyword ensures visibility of changes to variables across threads.

**How Volatile Works:**

```java
public class VolatileExample {
    private volatile boolean flag = false;
    
    // Thread 1
    public void writer() {
        flag = true; // Write to main memory immediately
    }
    
    // Thread 2
    public void reader() {
        while (!flag) { // Always reads from main memory
            // Wait for flag to be true
        }
        System.out.println("Flag is now true");
    }
}
```

**Volatile Guarantees:**
1. **Visibility**: Changes made by one thread are immediately visible to other threads
2. **Ordering**: Prevents instruction reordering around volatile variables
3. **No Atomicity**: Does not guarantee atomicity for compound operations

**Correct Usage:**

```java
public class StatusChecker {
    private volatile boolean status = false;
    
    // Simple read/write - volatile is sufficient
    public void setStatus(boolean status) {
        this.status = status;
    }
    
    public boolean isStatus() {
        return status;
    }
}
```

**Incorrect Usage (Compound Operations):**

```java
public class VolatileCounter {
    private volatile int counter = 0;
    
    // NOT thread-safe despite volatile
    public void increment() {
        counter++; // Read-modify-write is not atomic
    }
}
```

**Double-Checked Locking with Volatile:**

```java
public final class Singleton {
    private static volatile Singleton instance = null;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) { // First check (no locking)
            synchronized (Singleton.class) {
                if (instance == null) { // Second check (with locking)
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

---

### Transient Modifier

The `transient` keyword marks fields that should not be serialized.

```java
public class User implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private String username;
    private transient String password; // Not serialized
    private transient File tempFile;   // Cannot be serialized
    
    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }
}

// Usage
User user = new User("john", "secret123");

// Serialize
ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("user.ser"));
out.writeObject(user);
out.close();

// Deserialize
ObjectInputStream in = new ObjectInputStream(new FileInputStream("user.ser"));
User deserializedUser = (User) in.readObject();
in.close();

System.out.println(deserializedUser.username); // "john"
System.out.println(deserializedUser.password); // null (not serialized)
```

**When to Use Transient:**
- Sensitive data (passwords, credit cards)
- Derived/calculated fields
- Non-serializable objects (File, Socket, Thread)
- Cache or temporary data

---

### Native Modifier

The `native` keyword indicates that a method is implemented in platform-specific code (C/C++).

```java
public class NativeExample {
    // Native method declaration
    public native void nativeMethod();
    
    // Load native library
    static {
        System.loadLibrary("nativelib");
    }
}
```

---

### Strictfp Modifier

The `strictfp` keyword ensures consistent floating-point calculations across platforms.

```java
public strictfp class StrictMath {
    public double calculate(double a, double b) {
        return a * b; // Strict IEEE 754 compliance
    }
}
```

---

## Interview Questions

### Q1: What is the difference between final, finally, and finalize?

**Answer:**

**`final` - Modifier for immutability and prevention of inheritance/overriding:**

```java
// Final variable - cannot be reassigned
final int MAX_SIZE = 100;

// Final method - cannot be overridden
public final void process() {
    // implementation
}

// Final class - cannot be extended
public final class ImmutableString {
    // implementation
}

// Final reference - reference cannot change, but object state can
final Employee emp = new Employee("John", "Doe");
// emp = new Employee(); // Compilation error
emp.setFirstName("Jane"); // OK if not immutable
```

**`finally` - Block that always executes in try-catch-finally:**

**Pre-Java 7 approach:**

```java
BufferedReader br = null;
try {
    File f = new File("c://temp/simple.txt");
    InputStream is = new FileInputStream(f);
    InputStreamReader isr = new InputStreamReader(is);
    br = new BufferedReader(isr);
    String read;
    while ((read = br.readLine()) != null) {
        System.out.println(read);
    }
} catch (IOException ioe) {
    ioe.printStackTrace();
} finally {
    // Always executes - even if exception occurs
    try {
        if (br != null) {
            br.close();
        }
    } catch (IOException ex) {
        ex.printStackTrace();
    }
}
```

**Java 7+ try-with-resources (preferred):**

```java
// AutoCloseable resources automatically closed
try (InputStream is = new FileInputStream(new File("c://temp/simple.txt"));
     InputStreamReader isr = new InputStreamReader(is);
     BufferedReader br = new BufferedReader(isr)) {
    
    String read;
    while ((read = br.readLine()) != null) {
        System.out.println(read);
    }
} catch (IOException ioe) {
    ioe.printStackTrace();
}
// No finally needed - resources auto-closed
```

**`finalize()` - Method called by garbage collector (deprecated in Java 9):**

```java
@Deprecated(since="9")
protected void finalize() throws Throwable {
    try {
        // Cleanup code - but unreliable
        // Don't use for closing resources!
    } finally {
        super.finalize();
    }
}
```

**Key Differences:**

| Aspect | final | finally | finalize() |
|--------|-------|---------|------------|
| Type | Modifier | Block | Method |
| Purpose | Immutability/Prevention | Guaranteed execution | Cleanup before GC |
| When | Compile-time | Runtime (exception handling) | Before garbage collection |
| Reliability | 100% | 100% (with exceptions) | Unreliable - deprecated |
| Use Case | Constants, prevent override | Resource cleanup | Avoid - use try-with-resources |

---

### Q2: What is the difference between final and const?

**Answer:**

**`const` is a reserved keyword in Java but NOT used.** It exists for potential future use but currently has no functionality.

```java
// const int VALUE = 10; // Compilation error - reserved but not implemented
```

**`final` - The Java way to create constants:**

```java
public class Constants {
    // Compile-time constant
    public static final int MAX_USERS = 1000;
    
    // Runtime constant
    public static final String TIMESTAMP = new Date().toString();
    
    // Instance constant
    private final String id;
    
    public Constants(String id) {
        this.id = id; // Must be initialized
    }
}
```

**Important: `final` for references only prevents reassignment, not mutation:**

```java
final Employee emp = new Employee("John", "Doe");

// Cannot reassign reference
// emp = new Employee("Jane", "Smith"); // Compilation error

// But CAN modify object state (unless object is immutable)
emp.setFirstName("Simon"); // OK - object state can change

// To make truly immutable, all fields must be final
public final class ImmutableEmployee {
    private final String firstName;
    private final String lastName;
    
    public ImmutableEmployee(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    
    // Only getters, no setters
    public String getFirstName() { return firstName; }
    public String getLastName() { return lastName; }
}
```

**Comparison with C++:**

| Feature | Java `final` | C++ `const` |
|---------|-------------|-------------|
| Variable | Reference immutable | Value immutable |
| Method | Cannot override | Cannot modify object state |
| Class | Cannot extend | N/A |

---

### Q3: What is the volatile keyword in Java?

**Answer:**

The `volatile` keyword ensures that changes to a variable are immediately visible to all threads by preventing caching and reordering.

**What volatile guarantees:**

1. **Visibility**: All reads/writes go directly to main memory
2. **Ordering**: Prevents instruction reordering (happens-before relationship)
3. **No Caching**: Thread-local caches are bypassed

**Memory Model:**

```
Without volatile:
Thread 1: [Local Cache] → Main Memory
Thread 2: [Local Cache] → Main Memory
(Threads may see stale values)

With volatile:
Thread 1: → Main Memory ←
Thread 2: → Main Memory ←
(All threads see latest value immediately)
```

**Example - Status Flag:**

```java
public class TaskRunner {
    private volatile boolean running = true;
    
    // Thread 1 - Worker
    public void run() {
        while (running) {
            // Do work
            processTask();
        }
        System.out.println("Stopped");
    }
    
    // Thread 2 - Controller
    public void stop() {
        running = false; // Immediately visible to Thread 1
    }
}
```

**Without volatile, Thread 1 might never see the change!**

**Happens-Before Guarantee (Java 5+):**

```java
public class VolatileExample {
    private int x = 0;
    private volatile boolean flag = false;
    
    // Thread 1
    public void writer() {
        x = 42;           // 1. Write to x
        flag = true;      // 2. Write to volatile flag
    }
    
    // Thread 2
    public void reader() {
        if (flag) {       // 3. Read volatile flag
            System.out.println(x); // 4. Guaranteed to see x = 42
        }
    }
}
```

**Guarantee**: All writes before a volatile write are visible to any thread that reads that volatile variable.

---

### Q4: How does volatile differ from synchronized?

**Answer:**

**Key Differences:**

| Aspect | volatile | synchronized |
|--------|----------|--------------|
| Applies to | Variables only | Methods/blocks |
| Guarantees | Visibility + Ordering | Visibility + Ordering + Atomicity |
| Locking | No locking | Acquires lock |
| Performance | Faster | Slower (lock overhead) |
| Compound ops | NOT safe | Safe |
| Use case | Simple flags | Complex operations |

**1. Scope:**

```java
// volatile - variables only
private volatile boolean flag;
private volatile int counter;

// synchronized - methods and blocks
public synchronized void method() { }
synchronized(this) { }
```

**2. Atomicity:**

```java
// volatile - NOT atomic for compound operations
public class VolatileCounter {
    private volatile int counter = 0;
    
    public void increment() {
        counter++; // NOT thread-safe! (read-modify-write)
    }
}

// synchronized - atomic for compound operations
public class SynchronizedCounter {
    private int counter = 0;
    
    public synchronized void increment() {
        counter++; // Thread-safe
    }
}
```

**3. Correct Usage of volatile:**

```java
// Example 1: Simple flag
public class StatusChecker {
    private volatile boolean status = false;
    
    public void setStatus(boolean status) {
        this.status = status; // Simple write - OK
    }
    
    public boolean getStatus() {
        return status; // Simple read - OK
    }
}

// Example 2: Double-checked locking
public class Singleton {
    private static volatile Singleton instance;
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

**4. Wrong Usage of volatile:**

```java
// WRONG - compound operation
private volatile int counter = 0;

public void increment() {
    counter++; // NOT thread-safe
    // Equivalent to:
    // int temp = counter;  // read
    // temp = temp + 1;     // modify
    // counter = temp;      // write
}

// CORRECT - use synchronized or AtomicInteger
private AtomicInteger counter = new AtomicInteger(0);

public void increment() {
    counter.incrementAndGet(); // Thread-safe
}
```

**Decision Guide:**

```
Use volatile when:
✓ Simple read/write operations
✓ Status flags
✓ One writer, multiple readers
✓ No compound operations

Use synchronized when:
✓ Compound operations (++, --, +=)
✓ Multiple operations must be atomic
✓ Complex state changes
✓ Need mutual exclusion
```

---

### Q5: What is the transient modifier?

**Answer:**

The `transient` modifier marks fields that should NOT be serialized when an object is persisted.

**Basic Usage:**

```java
public class User implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private String username;           // Will be serialized
    private transient String password; // Will NOT be serialized
    private transient int loginCount;  // Will NOT be serialized
    
    public User(String username, String password) {
        this.username = username;
        this.password = password;
        this.loginCount = 0;
    }
}
```

**After Deserialization:**

```java
// Serialize
User user = new User("john", "secret123");
ObjectOutputStream out = new ObjectOutputStream(
    new FileOutputStream("user.ser"));
out.writeObject(user);
out.close();

// Deserialize
ObjectInputStream in = new ObjectInputStream(
    new FileInputStream("user.ser"));
User deserializedUser = (User) in.readObject();
in.close();

System.out.println(deserializedUser.username);   // "john"
System.out.println(deserializedUser.password);   // null (transient)
System.out.println(deserializedUser.loginCount); // 0 (default value)
```

**When to Use Transient:**

1. **Sensitive Data:**
```java
public class Account implements Serializable {
    private String accountNumber;
    private transient String pin;           // Security
    private transient String creditCardCVV; // Security
}
```

2. **Non-Serializable Objects:**
```java
public class FileProcessor implements Serializable {
    private String filename;
    private transient File file;           // File is not serializable
    private transient Socket connection;   // Socket is not serializable
    private transient Thread workerThread; // Thread is not serializable
}
```

3. **Derived/Calculated Fields:**
```java
public class Rectangle implements Serializable {
    private double width;
    private double height;
    private transient double area; // Can be recalculated
    
    private void readObject(ObjectInputStream in) 
            throws IOException, ClassNotFoundException {
        in.defaultReadObject();
        area = width * height; // Recalculate after deserialization
    }
}
```

4. **Cache/Temporary Data:**
```java
public class DataCache implements Serializable {
    private Map<String, String> persistentData;
    private transient Map<String, String> cache; // Don't persist cache
}
```

**Important Notes:**

- `transient` cannot be used with `static` variables (static variables belong to class, not object)
- Transient fields are set to default values after deserialization (null for objects, 0 for numbers, false for boolean)
- Use `readObject()` method to initialize transient fields after deserialization

**Transient vs @Transient:**

```java
// Java serialization
private transient String field1;

// JPA/Hibernate - different purpose
@Transient
private String field2; // Not persisted to database
```

---

### Q6: What value will a finally block return?

**Answer:**

**The finally block has the power to override any return value or exception from try/catch blocks.**

**Example 1: Finally overrides return value**

```java
public static int getSomeNumber() {
    try {
        return 2;
    } finally {
        return 1; // Finally wins - returns 1
    }
}

public static void main(String[] args) {
    System.out.println(getSomeNumber()); // Prints: 1
}
```

**Example 2: Finally suppresses exceptions**

```java
public static int getSomeNumber() {
    try {
        throw new RuntimeException("Exception from try");
    } finally {
        return 12; // Suppresses the exception!
    }
}

public static void main(String[] args) {
    System.out.println(getSomeNumber()); // Prints: 12 (no exception thrown)
}
```

**Why This is BAD PRACTICE:**

1. **Suppresses Exceptions:**
```java
public void badExample() {
    try {
        // Critical error occurs
        throw new DatabaseException("Connection lost");
    } finally {
        return; // Exception is lost forever!
    }
}
```

2. **Confusing Control Flow:**
```java
public int confusingExample() {
    try {
        return calculateResult(); // Developer expects this
    } finally {
        return 0; // But this is what actually returns
    }
}
```

**BEST PRACTICES:**

✓ **DO**: Use finally for cleanup only
```java
public String readFile(String path) throws IOException {
    BufferedReader reader = null;
    try {
        reader = new BufferedReader(new FileReader(path));
        return reader.readLine();
    } finally {
        if (reader != null) {
            reader.close(); // Cleanup only
        }
    }
}
```

✓ **DO**: Use try-with-resources (Java 7+)
```java
public String readFile(String path) throws IOException {
    try (BufferedReader reader = new BufferedReader(new FileReader(path))) {
        return reader.readLine();
    }
    // No finally needed - auto-closed
}
```

✗ **DON'T**: Return from finally
```java
// BAD - Don't do this!
public int badExample() {
    try {
        return 1;
    } finally {
        return 2; // Avoid this!
    }
}
```

---

### Q7: What can prevent execution of a finally block?

**Answer:**

While `finally` blocks almost always execute, there are specific scenarios where they won't:

**1. Infinite Loop:**

```java
public static void main(String[] args) {
    try {
        System.out.println("This line is printed");
        while (true) {
            // Infinite loop - finally never reached
        }
    } finally {
        System.out.println("Finally block is reached"); // Never executes
    }
}
```

**2. System.exit():**

```java
public static void main(String[] args) {
    try {
        System.out.println("This line is printed");
        System.exit(1); // JVM terminates
    } finally {
        System.out.println("Finally block is reached"); // Never executes
    }
}
```

**3. JVM Crash or Power Loss:**

```java
public static void main(String[] args) {
    try {
        System.out.println("This line is printed");
        Runtime.getRuntime().halt(1); // Forceful JVM shutdown
    } finally {
        System.out.println("Finally block is reached"); // Never executes
    }
}
```

**4. Thread Death:**

```java
public static void main(String[] args) {
    try {
        System.out.println("This line is printed");
        Thread.currentThread().stop(); // Deprecated - kills thread
    } finally {
        System.out.println("Finally block is reached"); // May not execute
    }
}
```

**5. Exception in Finally Block Itself:**

```java
public static void main(String[] args) {
    try {
        System.out.println("Try block");
    } finally {
        System.out.println("Finally started");
        throw new RuntimeException("Exception in finally");
        // Code after exception won't execute
        System.out.println("This won't print");
    }
}
```

**6. Daemon Thread Termination:**

```java
public static void main(String[] args) {
    Thread daemon = new Thread(() -> {
        try {
            Thread.sleep(5000);
        } finally {
            // May not execute if JVM exits
            System.out.println("Daemon finally");
        }
    });
    daemon.setDaemon(true);
    daemon.start();
    // Main thread ends, JVM may exit before daemon finally executes
}
```

**Summary:**

| Scenario | Finally Executes? | Reason |
|----------|------------------|---------|
| Normal execution | ✓ Yes | Standard flow |
| Exception thrown | ✓ Yes | Exception handling |
| Return in try | ✓ Yes | Before return |
| Infinite loop | ✗ No | Never reaches finally |
| System.exit() | ✗ No | JVM terminates |
| JVM crash | ✗ No | Process killed |
| Thread.stop() | ✗ Maybe | Deprecated, unsafe |
| Exception in finally | ✗ Partial | Stops at exception |

**Best Practice - Use Try-With-Resources:**

```java
// Preferred approach - resources always closed
try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
    return reader.readLine();
} catch (IOException e) {
    e.printStackTrace();
}
// No finally needed - AutoCloseable handles cleanup
```

---

## 📚 Related Topics

- [Annotations](./annotations.md)
- [Annotation Processing](./annotation-processing.md)
- [Classes and Interfaces](../Module%2004%20-%20Classes%20Interfaces%20ClassLoaders/)
- [OOP Principles](../Module%2006%20-%20OOP%20and%20FP/)
- [Multi-threading](../Module%2008%20-%20Multi-threading/)

---

## 💡 Key Takeaways

**Access Modifiers:**
- Use `public` for APIs and public interfaces
- Use `protected` for inheritance hierarchies
- Use default (package-private) for internal implementation
- Use `private` for encapsulation

**Non-Access Modifiers:**
- `static` - belongs to class, not instance
- `final` - prevents modification/inheritance/overriding
- `volatile` - ensures visibility across threads (simple operations only)
- `synchronized` - ensures atomicity and visibility (use for compound operations)
- `transient` - excludes from serialization
- `abstract` - requires implementation by subclasses

**Best Practices:**
- Prefer `final` for immutability
- Use `volatile` for simple flags, `synchronized` for complex operations
- Use try-with-resources instead of finally for resource management
- Never return from finally blocks
- Mark sensitive/non-serializable fields as `transient`

---

**[⬆ Back to Top](#)**