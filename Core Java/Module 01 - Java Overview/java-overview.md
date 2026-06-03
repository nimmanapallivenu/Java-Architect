# Java Overview & Fundamentals

> **Module**: Java Overview  
> **Difficulty**: Beginner to Intermediate  
> **Estimated Time**: 2-3 hours

---

## 📋 Table of Contents

1. [Introduction to Java](#introduction-to-java)
2. [Why Choose Java?](#why-choose-java)
3. [Java Platform Architecture](#java-platform-architecture)
4. [JDK, JRE, JVM & JIT Explained](#jdk-jre-jvm--jit-explained)
5. [Is Java 100% Object-Oriented?](#is-java-100-object-oriented)
6. [Java vs C++](#java-vs-c)
7. [Java Compilation & Execution](#java-compilation--execution)
8. [JVM Modes & Configuration](#jvm-modes--configuration)
9. [Java Packages & JAR Files](#java-packages--jar-files)
10. [Best Practices](#best-practices)

---

## Introduction to Java

Java is a **high-level, class-based, object-oriented programming language** designed to have as few implementation dependencies as possible. It follows the principle of **"Write Once, Run Anywhere" (WORA)**, meaning compiled Java code can run on all platforms that support Java without recompilation.

### Key Characteristics

```
┌─────────────────────────────────────────────────────────┐
│              JAVA KEY CHARACTERISTICS                    │
├─────────────────────────────────────────────────────────┤
│  ✓ Platform Independent    ✓ Object-Oriented            │
│  ✓ Secure                  ✓ Robust                     │
│  ✓ Multi-threaded          ✓ High Performance           │
│  ✓ Distributed             ✓ Dynamic                    │
└─────────────────────────────────────────────────────────┘
```

---

## Why Choose Java?

### Business & Technical Considerations

When choosing Java for your project, consider these factors:

#### ✅ Advantages

1. **Mature Ecosystem**
   - Millions of developers worldwide
   - Thousands of frameworks and libraries
   - Extensive documentation and community support

2. **Enterprise Ready**
   - Proven in mission-critical applications
   - Strong security features
   - Excellent scalability

3. **Rich API**
   - Comprehensive standard library
   - Built-in support for networking, I/O, data structures
   - Reduces development time

4. **Automatic Memory Management**
   - Garbage collection handles memory cleanup
   - Reduces memory leaks
   - Simplifies development

5. **Multi-threading Support**
   - Built-in concurrency utilities
   - Thread-safe collections
   - Executor framework for task management

### Real-World Use Cases

```
┌──────────────────────────────────────────────────────────┐
│                 JAVA USE CASES                           │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  🌐 Web Applications                                     │
│     • E-commerce platforms (Amazon, eBay)               │
│     • Banking systems                                    │
│     • Enterprise portals                                 │
│                                                          │
│  📱 Mobile Applications                                  │
│     • Android apps (primary language)                   │
│     • Cross-platform mobile solutions                   │
│                                                          │
│  🖥️  Desktop Applications                               │
│     • IDEs (IntelliJ IDEA, Eclipse)                     │
│     • Trading platforms                                  │
│                                                          │
│  ☁️  Cloud & Microservices                              │
│     • Spring Boot microservices                         │
│     • Cloud-native applications                         │
│                                                          │
│  🎮 Big Data & Analytics                                │
│     • Hadoop ecosystem                                   │
│     • Apache Spark                                       │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## Java Platform Architecture

### The Java Platform Components

```
┌─────────────────────────────────────────────────────────────┐
│                    JAVA PLATFORM                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌───────────────────────────────────────────────────┐    │
│  │         Java Application Layer                     │    │
│  │  (Your Java Programs: .java source files)         │    │
│  └───────────────────────────────────────────────────┘    │
│                         ↓                                   │
│  ┌───────────────────────────────────────────────────┐    │
│  │         Java API (Standard Library)                │    │
│  │  • java.lang  • java.util  • java.io              │    │
│  │  • java.net   • java.sql   • javax.*              │    │
│  └───────────────────────────────────────────────────┘    │
│                         ↓                                   │
│  ┌───────────────────────────────────────────────────┐    │
│  │    Java Virtual Machine (JVM)                      │    │
│  │  • Class Loader  • Bytecode Verifier              │    │
│  │  • Execution Engine (Interpreter + JIT)           │    │
│  │  • Garbage Collector                               │    │
│  └───────────────────────────────────────────────────┘    │
│                         ↓                                   │
│  ┌───────────────────────────────────────────────────┐    │
│  │    Operating System (Windows, Linux, macOS)        │    │
│  └───────────────────────────────────────────────────┘    │
│                         ↓                                   │
│  ┌───────────────────────────────────────────────────┐    │
│  │         Hardware (CPU, Memory, Disk)               │    │
│  └───────────────────────────────────────────────────┘    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### Platform Independence

Unlike traditional compiled languages, Java achieves platform independence through bytecode:

```
Traditional Compilation (C/C++):
┌──────────┐    ┌──────────┐    ┌──────────────┐
│ Source   │ -> │ Compiler │ -> │ Native Code  │
│ (.c/.cpp)│    │          │    │ (OS-specific)│
└──────────┘    └──────────┘    └──────────────┘

Java Compilation & Execution:
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌─────────┐
│ Source   │ -> │  javac   │ -> │ Bytecode │ -> │   JVM   │
│ (.java)  │    │ Compiler │    │ (.class) │    │ (Any OS)│
└──────────┘    └──────────┘    └──────────┘    └─────────┘
                                                       ↓
                                              ┌────────────────┐
                                              │ Native Machine │
                                              │     Code       │
                                              └────────────────┘
```

---

## JDK, JRE, JVM & JIT Explained

### Component Hierarchy

```
┌─────────────────────────────────────────────────────────────┐
│                    JDK (Java Development Kit)               │
│  ┌───────────────────────────────────────────────────────┐ │
│  │  Development Tools:                                    │ │
│  │  • javac (compiler)    • javadoc (documentation)      │ │
│  │  • jar (archiver)      • jdb (debugger)               │ │
│  │  • javap (disassembler)                               │ │
│  └───────────────────────────────────────────────────────┘ │
│                                                             │
│  ┌───────────────────────────────────────────────────────┐ │
│  │           JRE (Java Runtime Environment)               │ │
│  │  ┌─────────────────────────────────────────────────┐  │ │
│  │  │  Java Class Libraries (Java API)                 │  │ │
│  │  │  • Core libraries (java.*, javax.*)              │  │ │
│  │  │  • Extension libraries                           │  │ │
│  │  └─────────────────────────────────────────────────┘  │ │
│  │                                                         │ │
│  │  ┌─────────────────────────────────────────────────┐  │ │
│  │  │        JVM (Java Virtual Machine)                │  │ │
│  │  │  ┌───────────────────────────────────────────┐  │  │ │
│  │  │  │  Class Loader Subsystem                   │  │  │ │
│  │  │  │  • Bootstrap  • Extension  • Application  │  │  │ │
│  │  │  └───────────────────────────────────────────┘  │  │ │
│  │  │  ┌───────────────────────────────────────────┐  │  │ │
│  │  │  │  Runtime Data Areas                       │  │  │ │
│  │  │  │  • Heap  • Stack  • Method Area           │  │  │ │
│  │  │  └───────────────────────────────────────────┘  │  │ │
│  │  │  ┌───────────────────────────────────────────┐  │  │ │
│  │  │  │  Execution Engine                         │  │  │ │
│  │  │  │  • Interpreter                            │  │  │ │
│  │  │  │  • JIT Compiler (Just-In-Time)           │  │  │ │
│  │  │  │  • Garbage Collector                      │  │  │ │
│  │  │  └───────────────────────────────────────────┘  │  │ │
│  │  └─────────────────────────────────────────────────┘  │ │
│  └───────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

### Detailed Explanations

#### 1. **JDK (Java Development Kit)**

The complete development environment for Java applications.

**Components:**
- **javac**: Compiles `.java` source files to `.class` bytecode
- **java**: Launches Java applications
- **javadoc**: Generates API documentation
- **jar**: Creates and manages JAR archives
- **jdb**: Java debugger
- **javap**: Class file disassembler

**Example Usage:**
```bash
# Compile Java source
javac HelloWorld.java

# Run Java application
java HelloWorld

# Create JAR file
jar cvf myapp.jar *.class

# View bytecode
javap -c HelloWorld.class
```

#### 2. **JRE (Java Runtime Environment)**

The runtime environment needed to run Java applications.

**Includes:**
- JVM (Java Virtual Machine)
- Core libraries (Java API)
- Supporting files

**Use Case:** End users who only need to run Java applications (not develop them) only need the JRE.

#### 3. **JVM (Java Virtual Machine)**

The engine that executes Java bytecode.

**Key Responsibilities:**
- Load bytecode
- Verify bytecode
- Execute bytecode
- Manage memory (Garbage Collection)
- Provide runtime environment

#### 4. **JIT (Just-In-Time Compiler)**

Improves performance by compiling bytecode to native machine code at runtime.

**How JIT Works:**

```
┌─────────────────────────────────────────────────────────┐
│              JIT COMPILATION PROCESS                     │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  1. Initial Execution (Interpreted)                     │
│     ┌──────────┐         ┌─────────────┐              │
│     │ Bytecode │ ──────> │ Interpreter │              │
│     └──────────┘         └─────────────┘              │
│                                 ↓                       │
│                          Slow Execution                 │
│                                                          │
│  2. Hotspot Detection                                   │
│     JVM identifies frequently executed code             │
│     (methods called multiple times)                     │
│                                                          │
│  3. JIT Compilation                                     │
│     ┌──────────┐         ┌──────────┐                 │
│     │ Bytecode │ ──────> │   JIT    │                 │
│     │ (Hotspot)│         │ Compiler │                 │
│     └──────────┘         └──────────┘                 │
│                                 ↓                       │
│                          ┌─────────────┐               │
│                          │ Native Code │               │
│                          │  (Cached)   │               │
│                          └─────────────┘               │
│                                 ↓                       │
│                          Fast Execution                 │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

**JIT Optimization Levels:**
- **C1 (Client Compiler)**: Fast compilation, moderate optimization
- **C2 (Server Compiler)**: Slower compilation, aggressive optimization

**Disable JIT (for debugging):**
```bash
java -Djava.compiler=NONE MyApplication
```

---

## Is Java 100% Object-Oriented?

### The Verdict: **No, but it's pragmatically OO**

### Six Criteria for Pure OOP

```
┌─────────────────────────────────────────────────────────┐
│         PURE OBJECT-ORIENTED LANGUAGE CRITERIA          │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  1. ✅ Encapsulation                                    │
│     Data hiding and modularity                          │
│                                                          │
│  2. ✅ Inheritance                                      │
│     Code reuse through class hierarchies                │
│                                                          │
│  3. ✅ Polymorphism                                     │
│     Same interface, different implementations           │
│                                                          │
│  4. ❌ All predefined types are objects                │
│     Java has primitive types (int, char, etc.)          │
│                                                          │
│  5. ❌ All operations via message passing               │
│     Java has static methods                             │
│                                                          │
│  6. ✅ All user-defined types are objects               │
│     All classes extend Object                           │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### Why Java Isn't 100% OO

#### Reason 1: Primitive Types

Java has 8 primitive types that are not objects:

```java
// Primitives (NOT objects)
byte    b = 127;
short   s = 32767;
int     i = 2147483647;
long    l = 9223372036854775807L;
float   f = 3.14f;
double  d = 3.14159265359;
char    c = 'A';
boolean bool = true;

// Wrapper Classes (Objects)
Byte    bObj = Byte.valueOf(b);
Short   sObj = Short.valueOf(s);
Integer iObj = Integer.valueOf(i);
Long    lObj = Long.valueOf(l);
Float   fObj = Float.valueOf(f);
Double  dObj = Double.valueOf(d);
Character cObj = Character.valueOf(c);
Boolean boolObj = Boolean.valueOf(bool);
```

**Why Primitives Exist:**
- **Performance**: Primitives are faster and use less memory
- **Simplicity**: Easier to use for basic operations

**Memory Comparison:**
```
┌──────────────────────────────────────────────────────┐
│         MEMORY USAGE: PRIMITIVE VS OBJECT            │
├──────────────────────────────────────────────────────┤
│                                                       │
│  int primitive:        4 bytes                       │
│  Integer object:       16 bytes (object overhead)    │
│                                                       │
│  Array of 1000 ints:   4,000 bytes                  │
│  Array of 1000 Integer: 16,000+ bytes               │
│                                                       │
└──────────────────────────────────────────────────────┘
```

#### Reason 2: Static Methods

Static methods can be called without creating an object:

```java
public class MathUtils {
    // Static method - no object needed
    public static int add(int a, int b) {
        return a + b;
    }
    
    // Instance method - object required
    public int multiply(int a, int b) {
        return a * b;
    }
}

// Usage
int sum = MathUtils.add(5, 3);  // No object creation

MathUtils utils = new MathUtils();
int product = utils.multiply(5, 3);  // Object required
```

#### Reason 3: No Multiple Inheritance

Java doesn't support multiple class inheritance to avoid the **Diamond Problem**:

```
         Animal
        /      \
       /        \
    Mammal    Bird
       \        /
        \      /
         Bat
         
Problem: Which eat() method does Bat inherit?
```

**Java's Solution: Interfaces**

```java
// Multiple interface implementation (allowed)
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck flying");
    }
    
    @Override
    public void swim() {
        System.out.println("Duck swimming");
    }
}
```

**Java 8+: Default Methods in Interfaces**

```java
interface Vehicle {
    default void start() {
        System.out.println("Vehicle starting");
    }
}

interface Electric {
    default void charge() {
        System.out.println("Charging battery");
    }
}

// Multiple behavior inheritance (Java 8+)
class Tesla implements Vehicle, Electric {
    // Inherits both default methods
}
```

#### Reason 4: No Operator Overloading

Java doesn't support operator overloading (except for `+` with Strings):

```java
// String concatenation (built-in)
String result = "Hello" + " " + "World";  // OK

// Numeric addition
int sum = 5 + 3;  // OK

// BigDecimal - verbose without operator overloading
BigDecimal a = new BigDecimal("10.5");
BigDecimal b = new BigDecimal("20.3");
BigDecimal c = new BigDecimal("5.2");

// Without operator overloading (current Java)
BigDecimal result = a.add(b).multiply(c);  // Verbose

// With operator overloading (not supported)
// BigDecimal result = (a + b) * c;  // Would be cleaner
```

---

## Java vs C++

### Key Differences

```
┌────────────────────────────────────────────────────────────┐
│              JAVA VS C++ COMPARISON                        │
├────────────────┬───────────────────┬───────────────────────┤
│   Feature      │      Java         │        C++            │
├────────────────┼───────────────────┼───────────────────────┤
│ Pointers       │ ❌ No pointers    │ ✅ Full pointer       │
│                │ (references only) │    support            │
├────────────────┼───────────────────┼───────────────────────┤
│ Multiple       │ ❌ No (interfaces │ ✅ Yes                │
│ Inheritance    │    instead)       │                       │
├────────────────┼───────────────────┼───────────────────────┤
│ Memory         │ ✅ Automatic GC   │ ❌ Manual (new/delete)│
│ Management     │                   │                       │
├────────────────┼───────────────────┼───────────────────────┤
│ Platform       │ ✅ Platform       │ ❌ Platform           │
│ Independence   │    independent    │    dependent          │
├────────────────┼───────────────────┼───────────────────────┤
│ Operator       │ ❌ No (except +)  │ ✅ Yes                │
│ Overloading    │                   │                       │
├────────────────┼───────────────────┼───────────────────────┤
│ Destructors    │ ❌ finalize()     │ ✅ Destructors        │
│                │    (deprecated)   │                       │
├────────────────┼───────────────────┼───────────────────────┤
│ Global         │ ❌ No             │ ✅ Yes                │
│ Variables      │                   │                       │
├────────────────┼───────────────────┼───────────────────────┤
│ Structures/    │ ❌ No (classes    │ ✅ Yes                │
│ Unions         │    only)          │                       │
└────────────────┴───────────────────┴───────────────────────┘
```

### Example: Memory Management

**C++ (Manual):**
```cpp
// C++ - Manual memory management
class Person {
    string name;
public:
    Person(string n) : name(n) {}
    ~Person() {  // Destructor
        cout << "Destroying " << name << endl;
    }
};

void createPerson() {
    Person* p = new Person("John");  // Allocate
    // ... use p ...
    delete p;  // Must manually free memory
}
```

**Java (Automatic):**
```java
// Java - Automatic garbage collection
class Person {
    private String name;
    
    public Person(String name) {
        this.name = name;
    }
    
    // No destructor needed
    // finalize() is deprecated - don't use it
}

void createPerson() {
    Person p = new Person("John");  // Allocate
    // ... use p ...
    // No need to free - GC handles it
}
```

---

## Java Compilation & Execution

### Complete Workflow

```
┌─────────────────────────────────────────────────────────────┐
│         JAVA COMPILATION & EXECUTION WORKFLOW               │
└─────────────────────────────────────────────────────────────┘

Step 1: Write Source Code
┌──────────────────────┐
│   HelloWorld.java    │
│                      │
│ public class Hello { │
│   public static void │
│   main(String[] args)│
│   {                  │
│     System.out.print │
│     ln("Hello!");    │
│   }                  │
│ }                    │
└──────────────────────┘
          ↓
          
Step 2: Compile with javac
┌──────────────────────┐
│   javac compiler     │
│                      │
│ • Syntax checking    │
│ • Type checking      │
│ • Code generation    │
└──────────────────────┘
          ↓
          
Step 3: Bytecode Generated
┌──────────────────────┐
│  HelloWorld.class    │
│                      │
│ CA FE BA BE 00 00... │
│ (Platform-independent│
│  bytecode)           │
└──────────────────────┘
          ↓
          
Step 4: JVM Execution
┌──────────────────────────────────────┐
│           JVM Process                │
│                                      │
│  1. Class Loading                    │
│     ┌────────────────────┐          │
│     │  Class Loader      │          │
│     │  • Bootstrap       │          │
│     │  • Extension       │          │
│     │  • Application     │          │
│     └────────────────────┘          │
│              ↓                       │
│  2. Bytecode Verification            │
│     ┌────────────────────┐          │
│     │  Verifier          │          │
│     │  • Type safety     │          │
│     │  • Access control  │          │
│     └────────────────────┘          │
│              ↓                       │
│  3. Execution                        │
│     ┌────────────────────┐          │
│     │  Interpreter       │          │
│     │  (Initial run)     │          │
│     └────────────────────┘          │
│              ↓                       │
│     ┌────────────────────┐          │
│     │  JIT Compiler      │          │
│     │  (Hot code)        │          │
│     └────────────────────┘          │
│              ↓                       │
│  4. Memory Management                │
│     ┌────────────────────┐          │
│     │  Garbage Collector │          │
│     └────────────────────┘          │
└──────────────────────────────────────┘
          ↓
          
Step 5: Output
┌──────────────────────┐
│   Console Output     │
│                      │
│   Hello!             │
└──────────────────────┘
```

### Bytecode Example

**Source Code:**
```java
public class Simple {
    public int add(int a, int b) {
        return a + b;
    }
}
```

**Compiled Bytecode (javap -c Simple.class):**
```
public int add(int, int);
  Code:
     0: iload_1        // Load first parameter
     1: iload_2        // Load second parameter
     2: iadd           // Add integers
     3: ireturn        // Return result
```

---

## JVM Modes & Configuration

### JVM Execution Modes

```
┌─────────────────────────────────────────────────────────┐
│              JVM EXECUTION MODES                        │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  1. Interpreted Mode                                    │
│     • Bytecode executed line by line                    │
│     • Slower execution                                  │
│     • Faster startup                                    │
│     • Lower memory usage                                │
│                                                          │
│  2. Compiled Mode (JIT)                                 │
│     • Bytecode compiled to native code                  │
│     • Faster execution                                  │
│     • Slower startup                                    │
│     • Higher memory usage                               │
│                                                          │
│  3. Mixed Mode (Default)                                │
│     • Combination of both                               │
│     • Interprets initially                              │
│     • Compiles hot spots                                │
│     • Best overall performance                          │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### JVM Flavors

#### 1. **Client VM (-client)**
- Optimized for desktop applications
- Faster startup time
- Lower memory footprint
- Less aggressive optimization

#### 2. **Server VM (-server)**
- Optimized for server applications
- Slower startup time
- Higher memory usage
- Aggressive optimization for long-running processes

**Check Current Mode:**
```bash
java -version

# Output example:
# Java HotSpot(TM) 64-Bit Server VM
```

### 32-bit vs 64-bit JVM

```
┌─────────────────────────────────────────────────────────┐
│           32-BIT VS 64-BIT JVM                          │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  32-bit JVM:                                            │
│  • Maximum heap size: ~1.5-2 GB                         │
│  • Lower memory overhead                                │
│  • Faster for small applications                        │
│  • Limited address space                                │
│                                                          │
│  64-bit JVM:                                            │
│  • Maximum heap size: Theoretically unlimited           │
│  • Higher memory overhead (~30-50% more)                │
│  • Required for large applications                      │
│  • Can address more memory                              │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### Common JVM Arguments

```bash
# Memory Settings
java -Xms512m -Xmx2g MyApp          # Initial/Max heap
java -Xss1m MyApp                    # Thread stack size
java -XX:MaxMetaspaceSize=256m MyApp # Metaspace limit

# Garbage Collection
java -XX:+UseG1GC MyApp              # Use G1 collector
java -XX:+UseParallelGC MyApp        # Parallel collector
java -XX:+UseConcMarkSweepGC MyApp   # CMS collector

# Performance Tuning
java -server MyApp                   # Server mode
java -XX:+AggressiveOpts MyApp       # Aggressive optimization
java -XX:+TieredCompilation MyApp    # Tiered compilation

# Debugging & Monitoring
java -verbose:gc MyApp               # GC logging
java -XX:+PrintGCDetails MyApp       # Detailed GC info
java -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5005 MyApp

# System Properties
java -Dfile.encoding=UTF-8 MyApp
java -Duser.timezone=UTC MyApp
```

### Monitoring JVM

**Tools:**
1. **jvisualvm** - Visual monitoring tool
2. **jconsole** - JMX-based monitoring
3. **jstat** - Command-line statistics
4. **jmap** - Heap dumps
5. **jstack** - Thread dumps

**Example:**
```bash
# Monitor GC activity
jstat -gc <pid> 1000

# Generate heap dump
jmap -dump:format=b,file=heap.bin <pid>

# Thread dump
jstack <pid> > threads.txt
```

---

## Java Packages & JAR Files

### Package Structure

```
┌─────────────────────────────────────────────────────────┐
│              JAVA PACKAGE HIERARCHY                     │
└─────────────────────────────────────────────────────────┘

com.company.project
├── model
│   ├── User.java
│   ├── Product.java
│   └── Order.java
├── service
│   ├── UserService.java
│   └── OrderService.java
├── repository
│   ├── UserRepository.java
│   └── OrderRepository.java
└── util
    ├── DateUtil.java
    └── StringUtil.java
```

**Creating a Package:**
```java
// File: com/company/project/model/User.java
package com.company.project.model;

public class User {
    private String name;
    private String email;
    
    // Constructor, getters, setters
}
```

**Using a Package:**
```java
// Import specific class
import com.company.project.model.User;

// Import all classes from package
import com.company.project.model.*;

// Use fully qualified name (no import)
com.company.project.model.User user = new com.company.project.model.User();
```

### JAR Files

**JAR (Java Archive)** is a package file format used to aggregate many Java class files and associated metadata into one file.

```
┌─────────────────────────────────────────────────────────┐
│              JAR FILE STRUCTURE                         │
└─────────────────────────────────────────────────────────┘

myapp.jar
├── META-INF/
│   ├── MANIFEST.MF          # Manifest file
│   └── services/            # Service provider configs
├── com/
│   └── company/
│       └── project/
│           ├── Main.class
│           ├── model/
│           │   ├── User.class
│           │   └── Product.class
│           └── service/
│               └── UserService.class
└── resources/
    ├── config.properties
    └── images/
        └── logo.png
```

**Creating a JAR:**
```bash
# Create JAR
jar cvf myapp.jar *.class

# Create JAR with manifest
jar cvfm myapp.jar manifest.txt *.class

# Create executable JAR
jar cvfe myapp.jar com.company.Main *.class

# Extract JAR
jar xvf myapp.jar

# List JAR contents
jar tvf myapp.jar
```

**Manifest File (META-INF/MANIFEST.MF):**
```
Manifest-Version: 1.0
Main-Class: com.company.project.Main
Class-Path: lib/dependency1.jar lib/dependency2.jar
```

**Running a JAR:**
```bash
# Run executable JAR
java -jar myapp.jar

# Run specific class from JAR
java -cp myapp.jar com.company.project.Main
```

### JAR vs ZIP

```
┌─────────────────────────────────────────────────────────┐
│              JAR VS ZIP                                 │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Similarities:                                          │
│  • Both use ZIP compression                             │
│  • Both can contain multiple files                      │
│  • Both can be extracted with ZIP tools                 │
│                                                          │
│  Differences:                                           │
│  JAR:                                                   │
│  • Contains META-INF/MANIFEST.MF                        │
│  • Can be executed by JVM                               │
│  • Designed for Java classes                            │
│  • Can be added to classpath                            │
│                                                          │
│  ZIP:                                                   │
│  • Generic archive format                               │
│  • No manifest file                                     │
│  • Cannot be executed directly                          │
│  • General purpose compression                          │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

---

## Best Practices

### 1. **Development Environment Setup**

```bash
# Set JAVA_HOME
export JAVA_HOME=/path/to/jdk
export PATH=$JAVA_HOME/bin:$PATH

# Verify installation
java -version
javac -version
```

### 2. **Package Naming Conventions**

```java
// Use reverse domain name
com.company.project.module

// Examples:
com.google.common.collect
org.apache.commons.lang3
io.github.username.project
```

### 3. **Class Organization**

```java
// One public class per file
// File name must match class name

// File: User.java
package com.company.model;

public class User {
    // Class implementation
}
```

### 4. **Compilation Best Practices**

```bash
# Compile with specific Java version
javac -source 11 -target 11 MyClass.java

# Compile with classpath
javac -cp lib/*:. MyClass.java

# Compile with output directory
javac -d bin src/**/*.java
```

### 5. **Running Applications**

```bash
# Run with classpath
java -cp bin:lib/* com.company.Main

# Run with system properties
java -Dconfig.file=app.properties com.company.Main

# Run with JVM options
java -Xms512m -Xmx2g -XX:+UseG1GC com.company.Main
```

---

## Summary

### Key Takeaways

✅ **Java is platform-independent** through bytecode and JVM  
✅ **JDK contains JRE, which contains JVM**  
✅ **JIT compiler improves performance** by compiling hot code  
✅ **Java is pragmatically OO** but not 100% pure  
✅ **Automatic memory management** via garbage collection  
✅ **Rich ecosystem** with extensive libraries and frameworks  
✅ **Enterprise-ready** with proven scalability and security  

### Next Steps

1. ✅ Understand Java compilation process
2. ✅ Learn about JVM architecture
3. ➡️ Study [Compile-time vs Runtime](compile-vs-runtime.md)
4. ➡️ Explore [Java Data Types](../Module%2002%20-%20Java%20Data%20Types/)
5. ➡️ Master [Object-Oriented Programming](../Module%2006%20-%20OOP%20and%20FP/)

---

## Additional Resources

- [Official Java Documentation](https://docs.oracle.com/en/java/)
- [Java Language Specification](https://docs.oracle.com/javase/specs/)
- [OpenJDK](https://openjdk.org/)
- [Java Community Process](https://jcp.org/)

---

**[← Back to Module Index](README.md)** | **[Next: Compile-time vs Runtime →](compile-vs-runtime.md)**