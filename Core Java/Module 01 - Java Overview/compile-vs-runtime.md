# Compile-time vs Runtime in Java

> **Module**: Java Overview  
> **Difficulty**: Intermediate  
> **Estimated Time**: 1-2 hours

---

## 📋 Table of Contents

1. [Introduction](#introduction)
2. [Compile-time Concepts](#compile-time-concepts)
3. [Runtime Concepts](#runtime-concepts)
4. [Constant Folding](#constant-folding)
5. [Method Overloading vs Overriding](#method-overloading-vs-overriding)
6. [Generics & Type Erasure](#generics--type-erasure)
7. [Annotations](#annotations)
8. [Practical Examples](#practical-examples)
9. [Best Practices](#best-practices)

---

## Introduction

Understanding the distinction between **compile-time** and **runtime** is crucial for Java developers. Many language features and optimizations occur at different phases of program execution.

### The Two Phases

```
┌─────────────────────────────────────────────────────────────┐
│              JAVA PROGRAM LIFECYCLE                         │
└─────────────────────────────────────────────────────────────┘

COMPILE-TIME (Static)                RUNTIME (Dynamic)
─────────────────────                ──────────────────

┌──────────────────┐                ┌──────────────────┐
│  Source Code     │                │  Bytecode        │
│  (.java files)   │                │  (.class files)  │
└──────────────────┘                └──────────────────┘
        ↓                                    ↓
┌──────────────────┐                ┌──────────────────┐
│  javac Compiler  │                │   JVM Loader     │
└──────────────────┘                └──────────────────┘
        ↓                                    ↓
┌──────────────────┐                ┌──────────────────┐
│ • Syntax Check   │                │ • Class Loading  │
│ • Type Check     │                │ • Verification   │
│ • Optimization   │                │ • Execution      │
│ • Code Gen       │                │ • GC             │
└──────────────────┘                └──────────────────┘
        ↓                                    ↓
┌──────────────────┐                ┌──────────────────┐
│  Bytecode        │                │  Program Output  │
│  Generated       │                │  & Side Effects  │
└──────────────────┘                └──────────────────┘

Happens ONCE                        Happens EVERY RUN
Known at compile                    Determined at runtime
Static binding                      Dynamic binding
```

---

## Compile-time Concepts

### What Happens at Compile-time?

```
┌─────────────────────────────────────────────────────────────┐
│           COMPILE-TIME ACTIVITIES                           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. ✅ Syntax Validation                                    │
│     • Check for proper Java syntax                          │
│     • Verify brackets, semicolons, keywords                 │
│                                                              │
│  2. ✅ Type Checking                                        │
│     • Verify type compatibility                             │
│     • Check method signatures                               │
│     • Validate generic types                                │
│                                                              │
│  3. ✅ Method Resolution (Overloading)                      │
│     • Determine which overloaded method to call             │
│     • Based on parameter types                              │
│                                                              │
│  4. ✅ Constant Folding                                     │
│     • Evaluate constant expressions                         │
│     • Optimize final variable usage                         │
│                                                              │
│  5. ✅ Generic Type Erasure                                 │
│     • Remove generic type information                       │
│     • Replace with raw types or bounds                      │
│                                                              │
│  6. ✅ Annotation Processing                                │
│     • Process compile-time annotations                      │
│     • Generate additional code if needed                    │
│                                                              │
│  7. ✅ Bytecode Generation                                  │
│     • Convert source to bytecode                            │
│     • Create .class files                                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Compile-time Errors

```java
// Syntax Error - Detected at compile-time
public class SyntaxError {
    public static void main(String[] args) {
        System.out.println("Hello"  // Missing semicolon
    }
}
// Error: ';' expected

// Type Error - Detected at compile-time
public class TypeError {
    public static void main(String[] args) {
        String text = 123;  // Cannot assign int to String
    }
}
// Error: incompatible types: int cannot be converted to String

// Method Not Found - Detected at compile-time
public class MethodError {
    public static void main(String[] args) {
        String text = "Hello";
        text.nonExistentMethod();  // Method doesn't exist
    }
}
// Error: cannot find symbol: method nonExistentMethod()
```

---

## Runtime Concepts

### What Happens at Runtime?

```
┌─────────────────────────────────────────────────────────────┐
│              RUNTIME ACTIVITIES                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. ✅ Class Loading                                        │
│     • Load classes as needed                                │
│     • Resolve dependencies                                  │
│                                                              │
│  2. ✅ Memory Allocation                                    │
│     • Allocate heap memory for objects                      │
│     • Allocate stack memory for methods                     │
│                                                              │
│  3. ✅ Method Resolution (Overriding)                       │
│     • Determine which overridden method to call             │
│     • Based on actual object type                           │
│                                                              │
│  4. ✅ Dynamic Binding                                      │
│     • Resolve method calls at runtime                       │
│     • Support polymorphism                                  │
│                                                              │
│  5. ✅ Exception Handling                                   │
│     • Catch and handle exceptions                           │
│     • Stack unwinding                                       │
│                                                              │
│  6. ✅ Garbage Collection                                   │
│     • Reclaim unused memory                                 │
│     • Prevent memory leaks                                  │
│                                                              │
│  7. ✅ Reflection                                           │
│     • Inspect classes at runtime                            │
│     • Invoke methods dynamically                            │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Runtime Errors

```java
// NullPointerException - Detected at runtime
public class RuntimeError1 {
    public static void main(String[] args) {
        String text = null;
        System.out.println(text.length());  // NPE at runtime
    }
}
// Exception in thread "main" java.lang.NullPointerException

// ArrayIndexOutOfBoundsException - Detected at runtime
public class RuntimeError2 {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3};
        System.out.println(numbers[5]);  // Index out of bounds
    }
}
// Exception: ArrayIndexOutOfBoundsException: Index 5 out of bounds

// ClassCastException - Detected at runtime
public class RuntimeError3 {
    public static void main(String[] args) {
        Object obj = "Hello";
        Integer num = (Integer) obj;  // Invalid cast
    }
}
// Exception: ClassCastException: String cannot be cast to Integer
```

---

## Constant Folding

### What is Constant Folding?

**Constant folding** is a compile-time optimization where the compiler evaluates constant expressions and replaces them with their computed values.

### Example: Constant Folding in Action

```java
public class ConstantFolding {
    // Final variables - known at compile-time
    static final int NUMBER1 = 5;
    static final int NUMBER2 = 6;
    
    // Non-final variables - evaluated at runtime
    static int number3 = 5;
    static int number4 = 6;
    
    public static void main(String[] args) {
        // Line A - Compile-time evaluation
        int product1 = NUMBER1 * NUMBER2;
        
        // Line B - Runtime evaluation
        int product2 = number3 * number4;
        
        System.out.println("Product1: " + product1);
        System.out.println("Product2: " + product2);
    }
}
```

### Decompiled Bytecode Analysis

**Original Source:**
```java
int product1 = NUMBER1 * NUMBER2;  // Line A
int product2 = number3 * number4;  // Line B
```

**Decompiled Code (using jd-gui or javap):**
```java
int product1 = 30;                 // Computed at compile-time!
int product2 = number3 * number4;  // Computed at runtime
```

### Visualization

```
┌─────────────────────────────────────────────────────────────┐
│           CONSTANT FOLDING PROCESS                          │
└─────────────────────────────────────────────────────────────┘

COMPILE-TIME:
─────────────
Source Code:
    final int A = 5;
    final int B = 6;
    int result = A * B;
           ↓
    Compiler sees: 5 * 6
           ↓
    Compiler computes: 30
           ↓
Bytecode:
    int result = 30;  ← Optimized!


RUNTIME:
────────
Source Code:
    int a = 5;
    int b = 6;
    int result = a * b;
           ↓
Bytecode:
    iload_1        // Load 'a'
    iload_2        // Load 'b'
    imul           // Multiply
    istore_3       // Store result
           ↓
    Computed at runtime
```

### Benefits of Constant Folding

```
┌─────────────────────────────────────────────────────────────┐
│         BENEFITS OF CONSTANT FOLDING                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ✅ Performance                                             │
│     • No runtime computation needed                         │
│     • Faster execution                                      │
│                                                              │
│  ✅ Smaller Bytecode                                        │
│     • Fewer instructions                                    │
│     • Reduced class file size                               │
│                                                              │
│  ✅ Optimization                                            │
│     • Compiler can make better decisions                    │
│     • Enables further optimizations                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Practical Example

```java
public class ConfigConstants {
    // These will be folded at compile-time
    public static final int MAX_CONNECTIONS = 100;
    public static final int TIMEOUT_SECONDS = 30;
    public static final int BUFFER_SIZE = MAX_CONNECTIONS * 1024;
    
    // This will be computed at runtime
    public static int currentConnections = 0;
    
    public void checkLimit() {
        // Compile-time: if (currentConnections > 100)
        if (currentConnections > MAX_CONNECTIONS) {
            throw new IllegalStateException("Too many connections");
        }
    }
}
```

---

## Method Overloading vs Overriding

### Compile-time: Method Overloading

**Method overloading** is resolved at **compile-time** based on the method signature (parameter types).

```
┌─────────────────────────────────────────────────────────────┐
│           METHOD OVERLOADING (COMPILE-TIME)                 │
└─────────────────────────────────────────────────────────────┘

Decision made by: COMPILER
Based on: PARAMETER TYPES (at compile-time)
Also called: STATIC POLYMORPHISM / EARLY BINDING

┌──────────────────────────────────────────────────────────┐
│  public class Calculator {                               │
│                                                          │
│      // Method #1                                        │
│      public int add(int a, int b) {                     │
│          return a + b;                                   │
│      }                                                   │
│                                                          │
│      // Method #2 - Overloaded                          │
│      public double add(double a, double b) {            │
│          return a + b;                                   │
│      }                                                   │
│                                                          │
│      // Method #3 - Overloaded                          │
│      public String add(String a, String b) {            │
│          return a + b;                                   │
│      }                                                   │
│  }                                                       │
└──────────────────────────────────────────────────────────┘
                         ↓
              COMPILER DETERMINES
                         ↓
┌──────────────────────────────────────────────────────────┐
│  Calculator calc = new Calculator();                     │
│                                                          │
│  calc.add(5, 3);        → Calls Method #1 (int)        │
│  calc.add(5.5, 3.2);    → Calls Method #2 (double)     │
│  calc.add("Hello", "!"); → Calls Method #3 (String)     │
└──────────────────────────────────────────────────────────┘
```

**Example:**

```java
public class OverloadingExample {
    // Overloaded methods
    public static void print(int value) {
        System.out.println("Integer: " + value);
    }
    
    public static void print(String value) {
        System.out.println("String: " + value);
    }
    
    public static void print(double value) {
        System.out.println("Double: " + value);
    }
    
    public static void main(String[] args) {
        // Compiler determines which method to call
        print(42);          // Calls print(int)
        print("Hello");     // Calls print(String)
        print(3.14);        // Calls print(double)
        
        // Compile-time error if no matching method
        // print(true);     // Error: no method print(boolean)
    }
}
```

### Runtime: Method Overriding

**Method overriding** is resolved at **runtime** based on the actual object type.

```
┌─────────────────────────────────────────────────────────────┐
│           METHOD OVERRIDING (RUNTIME)                       │
└─────────────────────────────────────────────────────────────┘

Decision made by: JVM
Based on: ACTUAL OBJECT TYPE (at runtime)
Also called: DYNAMIC POLYMORPHISM / LATE BINDING

┌──────────────────────────────────────────────────────────┐
│  class Animal {                                          │
│      public void makeSound() {                           │
│          System.out.println("Some sound");               │
│      }                                                   │
│  }                                                       │
│                                                          │
│  class Dog extends Animal {                             │
│      @Override                                          │
│      public void makeSound() {                          │
│          System.out.println("Woof!");                   │
│      }                                                   │
│  }                                                       │
│                                                          │
│  class Cat extends Animal {                             │
│      @Override                                          │
│      public void makeSound() {                          │
│          System.out.println("Meow!");                   │
│      }                                                   │
│  }                                                       │
└──────────────────────────────────────────────────────────┘
                         ↓
                JVM DETERMINES AT RUNTIME
                         ↓
┌──────────────────────────────────────────────────────────┐
│  Animal animal1 = new Dog();                             │
│  Animal animal2 = new Cat();                             │
│  Animal animal3 = new Animal();                          │
│                                                          │
│  animal1.makeSound();  → "Woof!" (Dog's method)         │
│  animal2.makeSound();  → "Meow!" (Cat's method)         │
│  animal3.makeSound();  → "Some sound" (Animal's method) │
└──────────────────────────────────────────────────────────┘
```

**Detailed Example:**

```java
public class OverridingExample {
    static class Shape {
        public double calculateArea() {
            return 0.0;
        }
        
        public void display() {
            System.out.println("Shape area: " + calculateArea());
        }
    }
    
    static class Circle extends Shape {
        private double radius;
        
        public Circle(double radius) {
            this.radius = radius;
        }
        
        @Override
        public double calculateArea() {
            return Math.PI * radius * radius;
        }
    }
    
    static class Rectangle extends Shape {
        private double width, height;
        
        public Rectangle(double width, double height) {
            this.width = width;
            this.height = height;
        }
        
        @Override
        public double calculateArea() {
            return width * height;
        }
    }
    
    public static void main(String[] args) {
        // Compile-time type: Shape
        // Runtime type: determined by actual object
        
        Shape shape1 = new Circle(5.0);      // Runtime: Circle
        Shape shape2 = new Rectangle(4, 6);  // Runtime: Rectangle
        Shape shape3 = new Shape();          // Runtime: Shape
        
        // JVM determines which calculateArea() to call at runtime
        shape1.display();  // Uses Circle's calculateArea()
        shape2.display();  // Uses Rectangle's calculateArea()
        shape3.display();  // Uses Shape's calculateArea()
    }
}
```

### Comparison Table

```
┌────────────────────────────────────────────────────────────────┐
│        OVERLOADING VS OVERRIDING                               │
├──────────────────┬─────────────────┬──────────────────────────┤
│   Aspect         │  Overloading    │     Overriding           │
├──────────────────┼─────────────────┼──────────────────────────┤
│ When Resolved    │ Compile-time    │ Runtime                  │
├──────────────────┼─────────────────┼──────────────────────────┤
│ Polymorphism     │ Static          │ Dynamic                  │
├──────────────────┼─────────────────┼──────────────────────────┤
│ Binding          │ Early           │ Late                     │
├──────────────────┼─────────────────┼──────────────────────────┤
│ Method Signature │ Different       │ Same                     │
├──────────────────┼─────────────────┼──────────────────────────┤
│ Return Type      │ Can differ      │ Same or covariant        │
├──────────────────┼─────────────────┼──────────────────────────┤
│ Access Modifier  │ Can differ      │ Same or less restrictive │
├──────────────────┼─────────────────┼──────────────────────────┤
│ Inheritance      │ Not required    │ Required                 │
├──────────────────┼─────────────────┼──────────────────────────┤
│ @Override        │ Not applicable  │ Recommended              │
├──────────────────┼─────────────────┼──────────────────────────┤
│ Example          │ print(int)      │ Animal.makeSound()       │
│                  │ print(String)   │ Dog.makeSound()          │
└──────────────────┴─────────────────┴──────────────────────────┘
```

---

## Generics & Type Erasure

### Type Erasure: Compile-time Feature

**Generics** in Java are a compile-time feature. The compiler performs **type erasure** to maintain backward compatibility with older Java versions.

```
┌─────────────────────────────────────────────────────────────┐
│              TYPE ERASURE PROCESS                           │
└─────────────────────────────────────────────────────────────┘

COMPILE-TIME:
─────────────
Source Code with Generics:
    List<String> names = new ArrayList<String>();
    names.add("Alice");
    String name = names.get(0);
           ↓
    Compiler checks type safety
           ↓
    Compiler performs type erasure
           ↓
Bytecode (No Generic Information):
    List names = new ArrayList();
    names.add("Alice");
    String name = (String) names.get(0);  ← Cast added


RUNTIME:
────────
    JVM sees only raw types
    No generic type information available
    Casts are inserted by compiler
```

### Example: Before and After Type Erasure

**Source Code:**
```java
public class GenericExample<T> {
    private T value;
    
    public void setValue(T value) {
        this.value = value;
    }
    
    public T getValue() {
        return value;
    }
    
    public static void main(String[] args) {
        GenericExample<String> example = new GenericExample<>();
        example.setValue("Hello");
        String result = example.getValue();
    }
}
```

**After Type Erasure (Decompiled):**
```java
public class GenericExample {
    private Object value;  // T → Object
    
    public void setValue(Object value) {  // T → Object
        this.value = value;
    }
    
    public Object getValue() {  // T → Object
        return value;
    }
    
    public static void main(String[] args) {
        GenericExample example = new GenericExample();  // No <String>
        example.setValue("Hello");
        String result = (String) example.getValue();  // Cast added
    }
}
```

### Bounded Type Parameters

```java
// Source with bounded type
public class BoundedExample<T extends Number> {
    private T value;
    
    public double doubleValue() {
        return value.doubleValue();
    }
}

// After type erasure
public class BoundedExample {
    private Number value;  // T → Number (not Object!)
    
    public double doubleValue() {
        return value.doubleValue();
    }
}
```

### Why Type Erasure?

```
┌─────────────────────────────────────────────────────────────┐
│           REASONS FOR TYPE ERASURE                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. ✅ Backward Compatibility                               │
│     • Works with pre-Java 5 code                            │
│     • No changes to JVM required                            │
│                                                              │
│  2. ✅ Single Bytecode                                      │
│     • One class file for all type parameters                │
│     • Smaller bytecode size                                 │
│                                                              │
│  3. ❌ Limitations                                          │
│     • Cannot create generic arrays                          │
│     • Cannot use instanceof with generics                   │
│     • Type information lost at runtime                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Limitations Due to Type Erasure

```java
public class TypeErasureLimitations<T> {
    // ❌ Cannot create generic array
    // T[] array = new T[10];  // Compile error
    
    // ❌ Cannot use instanceof with type parameter
    public boolean check(Object obj) {
        // return obj instanceof T;  // Compile error
        return false;
    }
    
    // ❌ Cannot create instance of type parameter
    public T createInstance() {
        // return new T();  // Compile error
        return null;
    }
    
    // ✅ Workaround: Use Class<T>
    private Class<T> type;
    
    public TypeErasureLimitations(Class<T> type) {
        this.type = type;
    }
    
    public T createInstanceWorkaround() throws Exception {
        return type.getDeclaredConstructor().newInstance();
    }
}
```

---

## Annotations

### Compile-time vs Runtime Annotations

Annotations can be processed at **compile-time**, **runtime**, or both.

```
┌─────────────────────────────────────────────────────────────┐
│              ANNOTATION RETENTION POLICIES                  │
└─────────────────────────────────────────────────────────────┘

@Retention(RetentionPolicy.SOURCE)
    ↓
Discarded by compiler
Used for: Code generation, compile-time checks
Examples: @Override, @SuppressWarnings
    ↓
Not in bytecode


@Retention(RetentionPolicy.CLASS)
    ↓
Retained in bytecode
Not available at runtime
Used for: Bytecode processing
Default retention policy
    ↓
In bytecode, not accessible via reflection


@Retention(RetentionPolicy.RUNTIME)
    ↓
Retained in bytecode
Available at runtime
Used for: Reflection, frameworks
Examples: @Autowired, @Entity, @Test
    ↓
Accessible via reflection
```

### Examples

#### 1. Compile-time Annotation: @Override

```java
public class Parent {
    public void display() {
        System.out.println("Parent");
    }
}

public class Child extends Parent {
    @Override  // Compile-time check
    public void display() {  // Correct spelling
        System.out.println("Child");
    }
    
    // @Override  // Compile error!
    // public void displya() {  // Typo - not overriding
    //     System.out.println("Child");
    // }
}
```

#### 2. Runtime Annotation: Custom Example

```java
import java.lang.annotation.*;
import java.lang.reflect.*;

// Define runtime annotation
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Test {
    String value() default "";
}

// Use annotation
public class TestRunner {
    @Test("Addition test")
    public void testAdd() {
        System.out.println("Testing addition");
    }
    
    @Test("Subtraction test")
    public void testSubtract() {
        System.out.println("Testing subtraction");
    }
    
    public void notATest() {
        System.out.println("Not a test");
    }
    
    // Runtime processing
    public static void main(String[] args) throws Exception {
        TestRunner runner = new TestRunner();
        Class<?> clazz = runner.getClass();
        
        // Reflection - available at runtime
        for (Method method : clazz.getDeclaredMethods()) {
            if (method.isAnnotationPresent(Test.class)) {
                Test test = method.getAnnotation(Test.class);
                System.out.println("Running: " + test.value());
                method.invoke(runner);
            }
        }
    }
}
```

---

## Practical Examples

### Example 1: Compile-time Constant Optimization

```java
public class PerformanceExample {
    // Compile-time constants
    private static final int ITERATIONS = 1000000;
    private static final double PI = 3.14159;
    
    public static void main(String[] args) {
        long start, end;
        
        // Test 1: Compile-time constant
        start = System.nanoTime();
        for (int i = 0; i < ITERATIONS; i++) {
            double area = PI * 5 * 5;  // Computed at compile-time
        }
        end = System.nanoTime();
        System.out.println("Constant: " + (end - start) + " ns");
        
        // Test 2: Runtime variable
        double pi = 3.14159;
        start = System.nanoTime();
        for (int i = 0; i < ITERATIONS; i++) {
            double area = pi * 5 * 5;  // Computed at runtime
        }
        end = System.nanoTime();
        System.out.println("Variable: " + (end - start) + " ns");
    }
}
```

### Example 2: Polymorphism Timing

```java
public class PolymorphismExample {
    static class Animal {
        public int compute(int x) {
            return x * 2;
        }
    }
    
    static class Dog extends Animal {
        @Override
        public int compute(int x) {
            return x * 3;
        }
    }
    
    public static void main(String[] args) {
        // Compile-time type: Animal
        // Runtime type: determined by object
        Animal animal = Math.random() > 0.5 ? new Dog() : new Animal();
        
        // JVM determines at runtime which compute() to call
        int result = animal.compute(10);
        System.out.println("Result: " + result);
        // Output: 20 or 30 (depends on runtime condition)
    }
}
```

### Example 3: Generic Type Safety

```java
public class GenericSafetyExample {
    public static void main(String[] args) {
        // Compile-time type safety
        List<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        // names.add(123);  // Compile error: type mismatch
        
        // Runtime: no generic information
        List rawList = names;  // Raw type
        rawList.add(123);  // No compile error (warning only)
        
        // Runtime error when retrieving
        try {
            String name = names.get(2);  // ClassCastException
        } catch (ClassCastException e) {
            System.out.println("Runtime error: " + e.getMessage());
        }
    }
}
```

---

## Best Practices

### 1. Use Final for Constants

```java
// ✅ Good: Compile-time constant
public static final int MAX_SIZE = 100;

// ❌ Avoid: Runtime variable
public static int maxSize = 100;
```

### 2. Leverage @Override

```java
// ✅ Good: Catch typos at compile-time
@Override
public String toString() {
    return "MyClass";
}

// ❌ Risky: Typo not caught
public String tostring() {  // Wrong method name
    return "MyClass";
}
```

### 3. Use Generics for Type Safety

```java
// ✅ Good: Type-safe at compile-time
List<String> names = new ArrayList<>();
names.add("Alice");
String name = names.get(0);  // No cast needed

// ❌ Avoid: Raw types
List names = new ArrayList();
names.add("Alice");
String name = (String) names.get(0);  // Cast required, error-prone
```

### 4. Understand Polymorphism Costs

```java
// Compile-time binding (faster)
public final class FastClass {
    public final void compute() {
        // Method cannot be overridden
        // JVM can inline this
    }
}

// Runtime binding (slower)
public class SlowClass {
    public void compute() {
        // Method can be overridden
        // JVM must check at runtime
    }
}
```

---

## Summary

### Key Takeaways

```
┌─────────────────────────────────────────────────────────────┐
│              COMPILE-TIME VS RUNTIME                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  COMPILE-TIME:                                              │
│  ✅ Syntax checking                                         │
│  ✅ Type checking                                           │
│  ✅ Method overloading resolution                           │
│  ✅ Constant folding                                        │
│  ✅ Generic type erasure                                    │
│  ✅ Compile-time annotations                                │
│                                                              │
│  RUNTIME:                                                   │
│  ✅ Class loading                                           │
│  ✅ Memory allocation                                       │
│  ✅ Method overriding resolution                            │
│  ✅ Dynamic binding                                         │
│  ✅ Exception handling                                      │
│  ✅ Garbage collection                                      │
│  ✅ Runtime annotations                                     │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Decision Matrix

| Feature | Compile-time | Runtime |
|---------|-------------|---------|
| Method Overloading | ✅ | ❌ |
| Method Overriding | ❌ | ✅ |
| Constant Folding | ✅ | ❌ |
| Generics | ✅ | ❌ (erased) |
| Polymorphism | Static | Dynamic |
| Type Checking | ✅ | Partial |
| Performance | Optimized | Variable |

### Next Steps

1. ✅ Understand compile-time optimizations
2. ✅ Master polymorphism concepts
3. ➡️ Study [Java Data Types](../Module%2002%20-%20Java%20Data%20Types/)
4. ➡️ Learn [Generics in Detail](../Module%2007%20-%20Generics%20and%20Collections/)
5. ➡️ Explore [Annotations](../Module%2003%20-%20Modifiers%20Annotations%20Initializers/)

---

**[← Back to Java Overview](java-overview.md)** | **[Module Index](README.md)** | **[Next: Java Data Types →](../Module%2002%20-%20Java%20Data%20Types/)**