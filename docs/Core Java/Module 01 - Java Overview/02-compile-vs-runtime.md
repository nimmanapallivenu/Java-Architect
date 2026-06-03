# 02. compile vs runtime

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents
- [Introduction](#introduction)
- [Compile-time Phase](#compile-time-phase)
- [Runtime Phase](#runtime-phase)
- [Compile-time vs Runtime Comparison](#compile-time-vs-runtime-comparison)
- [Compile-time Errors](#compile-time-errors)
- [Runtime Errors](#runtime-errors)
- [Real-World Examples](#real-world-examples)
- [Best Practices](#best-practices)
- [Interview Questions](#interview-questions)

---

## Introduction

Understanding the distinction between compile-time and runtime is crucial for Java developers. These two phases represent different stages in the Java program lifecycle, each with its own characteristics, error types, and optimization opportunities.

### The Two Phases

```
┌─────────────────────────────────────────────────────────────┐
│              JAVA PROGRAM LIFECYCLE                          │
└─────────────────────────────────────────────────────────────┘

COMPILE-TIME                          RUNTIME
─────────────                         ───────
    │                                     │
    ▼                                     ▼
┌─────────┐                         ┌─────────┐
│ Source  │                         │Bytecode │
│  Code   │                         │ (.class)│
│ (.java) │                         │         │
└─────────┘                         └─────────┘
    │                                     │
    ▼                                     ▼
┌─────────┐                         ┌─────────┐
│  javac  │                         │   JVM   │
│Compiler │                         │         │
└─────────┘                         └─────────┘
    │                                     │
    ▼                                     ▼
┌─────────┐                         ┌─────────┐
│Bytecode │                         │Execution│
│Generated│                         │ Output  │
└─────────┘                         └─────────┘
    │                                     │
    ▼                                     ▼
Syntax Check                        Logic Execution
Type Check                          Memory Allocation
Reference Check                     Exception Handling
```

---

## Compile-time Phase

### What Happens at Compile-time?

The compile-time phase occurs when you run `javac` to compile your Java source code.

```
┌───────────────────────────────────────────────────────────┐
│              COMPILATION PROCESS                           │
├───────────────────────────────────────────────────────────┤
│                                                            │
│  1. LEXICAL ANALYSIS                                       │
│     ┌────────────────────────────────────────┐           │
│     │ Source Code → Tokens                    │           │
│     │ "public class" → [PUBLIC][CLASS]        │           │
│     └────────────────────────────────────────┘           │
│                      │                                     │
│                      ▼                                     │
│  2. SYNTAX ANALYSIS                                        │
│     ┌────────────────────────────────────────┐           │
│     │ Tokens → Parse Tree                     │           │
│     │ Check grammar rules                     │           │
│     └────────────────────────────────────────┘           │
│                      │                                     │
│                      ▼                                     │
│  3. SEMANTIC ANALYSIS                                      │
│     ┌────────────────────────────────────────┐           │
│     │ Type checking                           │           │
│     │ Variable declaration checking           │           │
│     │ Method signature verification           │           │
│     └────────────────────────────────────────┘           │
│                      │                                     │
│                      ▼                                     │
│  4. CODE GENERATION                                        │
│     ┌────────────────────────────────────────┐           │
│     │ Generate bytecode (.class file)        │           │
│     │ Optimize code                           │           │
│     └────────────────────────────────────────┘           │
│                                                            │
└───────────────────────────────────────────────────────────┘
```

### Compile-time Checks

#### 1. Syntax Checking
```java
// Correct syntax
public class MyClass {
    public void myMethod() {
        System.out.println("Hello");
    }
}

// Compile-time error: missing semicolon
public class MyClass {
    public void myMethod() {
        System.out.println("Hello")  // Error: ';' expected
    }
}

// Compile-time error: missing closing brace
public class MyClass {
    public void myMethod() {
        System.out.println("Hello");
    // Error: '}' expected
}
```

#### 2. Type Checking
```java
public class TypeChecking {
    public static void main(String[] args) {
        // Correct: compatible types
        int number = 10;
        double decimal = 10.5;
        String text = "Hello";
        
        // Compile-time error: incompatible types
        // int x = "Hello";  // Error: incompatible types
        
        // Compile-time error: lossy conversion
        // int y = 10.5;  // Error: possible lossy conversion
        
        // Correct: explicit casting
        int z = (int) 10.5;  // OK
        
        // Compile-time error: cannot find symbol
        // System.out.println(undeclaredVariable);  // Error
    }
}
```

#### 3. Access Control Checking
```java
public class AccessControl {
    private int privateField = 10;
    protected int protectedField = 20;
    public int publicField = 30;
    
    private void privateMethod() {
        System.out.println("Private method");
    }
}

class AnotherClass {
    public void test() {
        AccessControl obj = new AccessControl();
        
        // Compile-time error: privateField has private access
        // System.out.println(obj.privateField);  // Error
        
        // OK: publicField is accessible
        System.out.println(obj.publicField);  // OK
        
        // Compile-time error: privateMethod has private access
        // obj.privateMethod();  // Error
    }
}
```

#### 4. Method Signature Checking
```java
public class MethodSignature {
    // Correct: method overloading
    public void print(int x) {
        System.out.println("Integer: " + x);
    }
    
    public void print(String x) {
        System.out.println("String: " + x);
    }
    
    // Compile-time error: duplicate method
    // public int print(int x) {  // Error: already defined
    //     return x;
    // }
    
    // Compile-time error: missing return statement
    // public int calculate() {  // Error: missing return
    //     int x = 10;
    // }
    
    // Correct: return statement present
    public int calculate() {
        return 10;
    }
}
```

---

## Runtime Phase

### What Happens at Runtime?

The runtime phase occurs when you execute the compiled bytecode using `java` command.

```
┌───────────────────────────────────────────────────────────┐
│              RUNTIME EXECUTION PROCESS                     │
├───────────────────────────────────────────────────────────┤
│                                                            │
│  1. CLASS LOADING                                          │
│     ┌────────────────────────────────────────┐           │
│     │ Load .class files into memory          │           │
│     │ Bootstrap → Extension → Application    │           │
│     └────────────────────────────────────────┘           │
│                      │                                     │
│                      ▼                                     │
│  2. LINKING                                                │
│     ┌────────────────────────────────────────┐           │
│     │ Verification: Bytecode verification    │           │
│     │ Preparation: Allocate memory           │           │
│     │ Resolution: Resolve symbolic refs      │           │
│     └────────────────────────────────────────┘           │
│                      │                                     │
│                      ▼                                     │
│  3. INITIALIZATION                                         │
│     ┌────────────────────────────────────────┐           │
│     │ Execute static initializers            │           │
│     │ Initialize static variables            │           │
│     └────────────────────────────────────────┘           │
│                      │                                     │
│                      ▼                                     │
│  4. EXECUTION                                              │
│     ┌────────────────────────────────────────┐           │
│     │ Execute main() method                  │           │
│     │ Create objects                         │           │
│     │ Call methods                           │           │
│     │ Handle exceptions                      │           │
│     └────────────────────────────────────────┘           │
│                                                            │
└───────────────────────────────────────────────────────────┘
```

### Runtime Operations

#### 1. Memory Allocation
```java
public class MemoryAllocation {
    public static void main(String[] args) {
        // Runtime: Memory allocated on heap
        String str = new String("Hello");
        
        // Runtime: Array memory allocated
        int[] numbers = new int[1000];
        
        // Runtime: Object creation
        Person person = new Person("John", 30);
        
        // Runtime: Can cause OutOfMemoryError
        // List<byte[]> list = new ArrayList<>();
        // while(true) {
        //     list.add(new byte[1024 * 1024]);  // 1MB each
        // }
    }
}

class Person {
    String name;
    int age;
    
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

#### 2. Dynamic Method Dispatch (Polymorphism)
```java
public class DynamicDispatch {
    public static void main(String[] args) {
        // Compile-time: Reference type is Animal
        // Runtime: Actual object type determines method called
        Animal animal1 = new Dog();
        Animal animal2 = new Cat();
        
        // Runtime: Dog's makeSound() is called
        animal1.makeSound();  // Output: Woof!
        
        // Runtime: Cat's makeSound() is called
        animal2.makeSound();  // Output: Meow!
    }
}

class Animal {
    void makeSound() {
        System.out.println("Some sound");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Woof!");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Meow!");
    }
}
```

#### 3. Exception Handling
```java
public class ExceptionHandling {
    public static void main(String[] args) {
        // Runtime: Division by zero
        try {
            int result = 10 / 0;  // ArithmeticException at runtime
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero");
        }
        
        // Runtime: Array index out of bounds
        try {
            int[] arr = {1, 2, 3};
            System.out.println(arr[5]);  // ArrayIndexOutOfBoundsException
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Invalid array index");
        }
        
        // Runtime: Null pointer
        try {
            String str = null;
            System.out.println(str.length());  // NullPointerException
        } catch (NullPointerException e) {
            System.out.println("Null reference");
        }
    }
}
```

---

## Compile-time vs Runtime Comparison

### Comprehensive Comparison Table

| Aspect | Compile-time | Runtime |
|--------|--------------|---------|
| **Phase** | Source code → Bytecode | Bytecode → Execution |
| **Tool** | javac compiler | JVM |
| **When** | Before execution | During execution |
| **Checks** | Syntax, types, references | Logic, memory, exceptions |
| **Errors** | Compilation errors | Runtime exceptions |
| **Performance** | One-time cost | Ongoing cost |
| **Optimization** | Code optimization | JIT optimization |
| **Type Checking** | Static type checking | Dynamic type checking |
| **Memory** | No memory allocation | Memory allocation |
| **Polymorphism** | Method binding (static) | Method dispatch (dynamic) |

### Visual Comparison

```
┌─────────────────────────────────────────────────────────────┐
│                    COMPILE-TIME                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ✓ Syntax errors                                            │
│  ✓ Type mismatches                                          │
│  ✓ Missing semicolons                                       │
│  ✓ Undeclared variables                                     │
│  ✓ Access violations                                        │
│  ✓ Method signature errors                                  │
│  ✓ Import errors                                            │
│  ✓ Generic type errors                                      │
│                                                              │
│  ✗ Logic errors                                             │
│  ✗ Division by zero                                         │
│  ✗ Null pointer exceptions                                  │
│  ✗ Array index errors                                       │
│  ✗ Memory issues                                            │
│                                                              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                      RUNTIME                                 │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ✓ Logic errors                                             │
│  ✓ Division by zero                                         │
│  ✓ Null pointer exceptions                                  │
│  ✓ Array index errors                                       │
│  ✓ Memory issues (OutOfMemoryError)                         │
│  ✓ Stack overflow                                           │
│  ✓ Class not found                                          │
│  ✓ IO exceptions                                            │
│  ✓ Network errors                                           │
│                                                              │
│  ✗ Syntax errors (already caught)                           │
│  ✗ Type mismatches (already caught)                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Compile-time Errors

### Common Compile-time Errors

#### 1. Syntax Errors
```java
public class SyntaxErrors {
    public static void main(String[] args) {
        // Error: ';' expected
        // int x = 10
        
        // Error: ')' expected
        // System.out.println("Hello";
        
        // Error: illegal start of expression
        // public int x = 10;  // Cannot use access modifier in method
        
        // Error: '}' expected
        // if (true) {
        //     System.out.println("True");
        // // Missing closing brace
    }
}
```

#### 2. Type Errors
```java
public class TypeErrors {
    public static void main(String[] args) {
        // Error: incompatible types
        // String text = 123;
        
        // Error: incompatible types: possible lossy conversion
        // int number = 10.5;
        
        // Error: incompatible types
        // boolean flag = "true";
        
        // Error: bad operand types
        // String result = "Hello" - "World";
        
        // Correct: explicit casting
        int number = (int) 10.5;
        String text = String.valueOf(123);
    }
}
```

#### 3. Reference Errors
```java
public class ReferenceErrors {
    public static void main(String[] args) {
        // Error: cannot find symbol
        // System.out.println(undeclaredVariable);
        
        // Error: cannot find symbol
        // undeclaredMethod();
        
        // Error: package does not exist
        // import com.nonexistent.package.*;
        
        // Correct: declare before use
        int declaredVariable = 10;
        System.out.println(declaredVariable);
    }
}
```

#### 4. Access Control Errors
```java
class MyClass {
    private int privateField = 10;
    
    private void privateMethod() {
        System.out.println("Private");
    }
}

public class AccessErrors {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        
        // Error: privateField has private access
        // System.out.println(obj.privateField);
        
        // Error: privateMethod() has private access
        // obj.privateMethod();
    }
}
```

---

## Runtime Errors

### Common Runtime Errors

#### 1. ArithmeticException
```java
public class ArithmeticErrors {
    public static void main(String[] args) {
        int a = 10;
        int b = 0;
        
        // Runtime error: Division by zero
        try {
            int result = a / b;  // ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Error: " + e.getMessage());
        }
        
        // Correct: Check before division
        if (b != 0) {
            int result = a / b;
            System.out.println("Result: " + result);
        } else {
            System.out.println("Cannot divide by zero");
        }
    }
}
```

#### 2. NullPointerException
```java
public class NullPointerErrors {
    public static void main(String[] args) {
        String str = null;
        
        // Runtime error: NullPointerException
        try {
            int length = str.length();  // NPE
        } catch (NullPointerException e) {
            System.out.println("Error: Null reference");
        }
        
        // Correct: Null check
        if (str != null) {
            int length = str.length();
            System.out.println("Length: " + length);
        } else {
            System.out.println("String is null");
        }
        
        // Java 8+: Optional
        String result = java.util.Optional.ofNullable(str)
                                          .orElse("Default");
        System.out.println(result);
    }
}
```

#### 3. ArrayIndexOutOfBoundsException
```java
public class ArrayErrors {
    public static void main(String[] args) {
        int[] numbers = {1, 2, 3, 4, 5};
        
        // Runtime error: ArrayIndexOutOfBoundsException
        try {
            System.out.println(numbers[10]);  // Index out of bounds
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Error: Invalid index");
        }
        
        // Correct: Check bounds
        int index = 10;
        if (index >= 0 && index < numbers.length) {
            System.out.println(numbers[index]);
        } else {
            System.out.println("Index out of bounds");
        }
    }
}
```

#### 4. ClassCastException
```java
public class CastErrors {
    public static void main(String[] args) {
        Object obj = "Hello";
        
        // Runtime error: ClassCastException
        try {
            Integer num = (Integer) obj;  // Cannot cast String to Integer
        } catch (ClassCastException e) {
            System.out.println("Error: Invalid cast");
        }
        
        // Correct: instanceof check
        if (obj instanceof Integer) {
            Integer num = (Integer) obj;
            System.out.println("Number: " + num);
        } else {
            System.out.println("Not an Integer");
        }
    }
}
```

---

## Real-World Examples

### Example 1: E-commerce Order Processing

```java
public class OrderProcessor {
    
    // Compile-time: Method signature checked
    public double calculateTotal(List<OrderItem> items) {
        // Compile-time: Type checking
        double total = 0.0;
        
        // Runtime: Null check needed
        if (items == null) {
            throw new IllegalArgumentException("Items cannot be null");
        }
        
        // Runtime: Iteration and calculation
        for (OrderItem item : items) {
            // Runtime: Null check for each item
            if (item != null) {
                // Runtime: Method dispatch
                total += item.getPrice() * item.getQuantity();
            }
        }
        
        return total;
    }
    
    // Compile-time: Return type checked
    public void processPayment(double amount) {
        // Runtime: Validation
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        
        // Runtime: External service call (can fail)
        try {
            PaymentGateway.charge(amount);
        } catch (PaymentException e) {
            // Runtime: Exception handling
            System.err.println("Payment failed: " + e.getMessage());
            throw e;
        }
    }
}

class OrderItem {
    private String productId;
    private double price;
    private int quantity;
    
    // Compile-time: Constructor signature
    public OrderItem(String productId, double price, int quantity) {
        // Runtime: Validation
        if (price < 0 || quantity < 0) {
            throw new IllegalArgumentException("Invalid price or quantity");
        }
        this.productId = productId;
        this.price = price;
        this.quantity = quantity;
    }
    
    // Compile-time: Getter signatures
    public double getPrice() { return price; }
    public int getQuantity() { return quantity; }
}
```

### Example 2: Database Connection

```java
public class DatabaseManager {
    
    // Compile-time: Method signature and exception declaration
    public Connection getConnection() throws SQLException {
        String url = "jdbc:mysql://localhost:3306/mydb";
        String user = "root";
        String password = "password";
        
        // Runtime: Database connection (can fail)
        try {
            // Runtime: Class loading
            Class.forName("com.mysql.jdbc.Driver");
            
            // Runtime: Network operation
            return DriverManager.getConnection(url, user, password);
        } catch (ClassNotFoundException e) {
            // Runtime: Driver not found
            throw new SQLException("MySQL driver not found", e);
        } catch (SQLException e) {
            // Runtime: Connection failed
            System.err.println("Connection failed: " + e.getMessage());
            throw e;
        }
    }
    
    // Compile-time: Generic type checking
    public <T> List<T> executeQuery(String sql, RowMapper<T> mapper) 
            throws SQLException {
        List<T> results = new ArrayList<>();
        
        // Runtime: Try-with-resources
        try (Connection conn = getConnection();
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {
            
            // Runtime: Iterate results
            while (rs.next()) {
                // Runtime: Map row to object
                T obj = mapper.mapRow(rs);
                results.add(obj);
            }
        }
        
        return results;
    }
}

@FunctionalInterface
interface RowMapper<T> {
    T mapRow(ResultSet rs) throws SQLException;
}
```

---

## Best Practices

### 1. Catch Errors Early (Compile-time)

```java
// Good: Use strong typing
public class StrongTyping {
    // Compile-time: Type safety
    public int add(int a, int b) {
        return a + b;
    }
    
    // Compile-time: Generic type safety
    public <T> List<T> createList(T... elements) {
        return Arrays.asList(elements);
    }
}

// Bad: Weak typing with Object
public class WeakTyping {
    // Runtime: Type checking needed
    public Object add(Object a, Object b) {
        if (a instanceof Integer && b instanceof Integer) {
            return (Integer) a + (Integer) b;
        }
        throw new IllegalArgumentException("Invalid types");
    }
}
```

### 2. Defensive Programming (Runtime)

```java
public class DefensiveProgramming {
    
    // Good: Validate inputs
    public void processUser(User user) {
        // Runtime: Null check
        if (user == null) {
            throw new IllegalArgumentException("User cannot be null");
        }
        
        // Runtime: Validate fields
        if (user.getName() == null || user.getName().isEmpty()) {
            throw new IllegalArgumentException("User name is required");
        }
        
        if (user.getAge() < 0 || user.getAge() > 150) {
            throw new IllegalArgumentException("Invalid age");
        }
        
        // Process user...
    }
    
    // Good: Use Optional for nullable values
    public Optional<User> findUser(String id) {
        // Runtime: Database query
        User user = database.findById(id);
        return Optional.ofNullable(user);
    }
}
```

### 3. Use Appropriate Exception Handling

```java
public class ExceptionBestPractices {
    
    // Good: Specific exceptions
    public void readFile(String filename) throws IOException {
        // Compile-time: Checked exception declared
        try (BufferedReader reader = new BufferedReader(
                new FileReader(filename))) {
            String line;
            while ((line = reader.readLine()) != null) {
                processLine(line);
            }
        }
        // Runtime: IOException handled by caller
    }
    
    // Good: Custom exceptions
    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(
                "Insufficient funds: " + balance);
        }
        balance -= amount;
    }
    
    private double balance = 1000.0;
    
    private void processLine(String line) {
        // Process line...
    }
}

class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```

---

## Interview Questions

### 🔹 Q1: What is the difference between compile-time and runtime?

**Answer**:

**Compile-time**:
- Phase when source code is converted to bytecode
- Performed by javac compiler
- Checks: syntax, types, references, access control
- Errors: compilation errors (must fix to run)

**Runtime**:
- Phase when bytecode is executed by JVM
- Performed by java command
- Operations: memory allocation, method dispatch, exception handling
- Errors: runtime exceptions (occur during execution)

### 🔹 Q2: What types of errors are caught at compile-time vs runtime?

**Answer**:

**Compile-time Errors**:
- Syntax errors (missing semicolon, braces)
- Type mismatches
- Undeclared variables/methods
- Access violations
- Method signature errors

**Runtime Errors**:
- NullPointerException
- ArrayIndexOutOfBoundsException
- ArithmeticException (division by zero)
- ClassCastException
- OutOfMemoryError
- StackOverflowError

### 🔹 Q3: Can you give an example of polymorphism at compile-time vs runtime?

**Answer**:

```java
// Compile-time polymorphism (Method Overloading)
class Calculator {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
    // Method resolved at compile-time based on parameters
}

// Runtime polymorphism (Method Overriding)
class Animal {
    void makeSound() { System.out.println("Some sound"); }
}

class Dog extends Animal {
    @Override
    void makeSound() { System.out.println("Woof!"); }
}

// Runtime: Actual method determined by object type
Animal animal = new Dog();
animal.makeSound();  // Calls Dog's makeSound() at runtime
```

### 🔹 Q4: What is early binding vs late binding?

**Answer**:

**Early Binding (Static Binding)**:
- Resolved at compile-time
- Used for: static methods, private methods, final methods
- Faster (no runtime overhead)

**Late Binding (Dynamic Binding)**:
- Resolved at runtime
- Used for: overridden methods (polymorphism)
- Flexible but slightly slower

```java
class Example {
    // Early binding
    static void staticMethod() { }
    private void privateMethod() { }
    final void finalMethod() { }
    
    // Late binding
    void instanceMethod() { }
}
```

### 🔹 Q5: How does Java achieve platform independence?

**Answer**:

Java achieves platform independence through:

1. **Compile-time**: Source code compiled to bytecode (platform-independent)
2. **Runtime**: Bytecode executed by JVM (platform-specific)

```
Source (.java) → Bytecode (.class) → JVM (Windows/Linux/Mac)
```

The bytecode is the same across platforms; only the JVM differs.

---

## Summary

Understanding compile-time vs runtime is essential for:
- **Writing robust code**: Catch errors early
- **Performance optimization**: Minimize runtime overhead
- **Debugging**: Know where to look for issues
- **Design decisions**: Choose appropriate error handling

**Key Takeaways**:
- Compile-time: Syntax, types, structure
- Runtime: Logic, memory, execution
- Use strong typing to catch errors early
- Implement defensive programming for runtime safety

**Next Steps**:
- Explore [Java Data Types](../Module%2002%20-%20Java%20Data%20Types/03-data-types.md)
- Learn about [Java Modifiers](../Module%2003%20-%20Modifiers%20Annotations%20Initializers/07-java-modifiers.md)

---

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

