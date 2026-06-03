# Classes & Interfaces

> **Module**: Classes Interfaces ClassLoaders  
> **Topic**: Classes & Interfaces - Comprehensive Guide with Diagrams and Examples

---

## 📋 Table of Contents

- [Introduction](#introduction)
- [Classes in Java](#classes-in-java)
- [Interfaces in Java](#interfaces-in-java)
- [Class vs Interface Comparison](#class-vs-interface-comparison)
- [Inheritance and Implementation](#inheritance-and-implementation)
- [Variable Shadowing](#variable-shadowing)
- [Method Overriding](#method-overriding)
- [Static Method Hiding](#static-method-hiding)
- [Marker Interfaces](#marker-interfaces)
- [Functional Interfaces](#functional-interfaces)
- [Diamond Problem](#diamond-problem)
- [Interview Questions](#interview-questions)
- [Best Practices](#best-practices)

---

## Introduction

Classes and interfaces are fundamental building blocks of Java's Object-Oriented Programming (OOP) paradigm. Understanding their differences, use cases, and relationships is crucial for designing robust Java applications.

### Key Concepts Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Java Type System                          │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────┐              ┌──────────────┐            │
│  │   Classes    │              │  Interfaces  │            │
│  ├──────────────┤              ├──────────────┤            │
│  │ • State      │              │ • Contract   │            │
│  │ • Behavior   │              │ • No State   │            │
│  │ • Single     │              │ • Multiple   │            │
│  │   Inheritance│              │   Inheritance│            │
│  └──────────────┘              └──────────────┘            │
│         │                              │                     │
│         └──────────┬───────────────────┘                     │
│                    │                                         │
│              ┌─────▼─────┐                                  │
│              │ Concrete  │                                  │
│              │   Class   │                                  │
│              └───────────┘                                  │
└─────────────────────────────────────────────────────────────┘
```

---

## Classes in Java

### What is a Class?

A class is a blueprint or template that defines the structure and behavior of objects. It encapsulates data (fields) and methods that operate on that data.

### Class Structure

```java
// Complete class structure example
public class Employee {
    // 1. Instance Variables (State)
    private String name;
    private int employeeId;
    private double salary;
    
    // 2. Static Variables (Class-level state)
    private static int employeeCount = 0;
    
    // 3. Constants
    public static final String COMPANY_NAME = "TechCorp";
    
    // 4. Constructor
    public Employee(String name, int employeeId, double salary) {
        this.name = name;
        this.employeeId = employeeId;
        this.salary = salary;
        employeeCount++;
    }
    
    // 5. Instance Methods (Behavior)
    public void displayInfo() {
        System.out.println("Name: " + name);
        System.out.println("ID: " + employeeId);
        System.out.println("Salary: $" + salary);
    }
    
    public void raiseSalary(double percentage) {
        salary += salary * (percentage / 100);
    }
    
    // 6. Static Methods
    public static int getEmployeeCount() {
        return employeeCount;
    }
    
    // 7. Getters and Setters
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
}
```

### Class Memory Layout

```
┌─────────────────────────────────────────────────────────────┐
│                    Heap Memory                               │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Employee Object 1                Employee Object 2          │
│  ┌──────────────────┐            ┌──────────────────┐       │
│  │ name: "John"     │            │ name: "Jane"     │       │
│  │ employeeId: 101  │            │ employeeId: 102  │       │
│  │ salary: 50000.0  │            │ salary: 60000.0  │       │
│  └──────────────────┘            └──────────────────┘       │
│                                                               │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                    Method Area (Metaspace)                   │
├─────────────────────────────────────────────────────────────┤
│  Employee Class Metadata                                     │
│  ┌────────────────────────────────────────────────┐         │
│  │ Static Variables:                              │         │
│  │   employeeCount: 2                             │         │
│  │   COMPANY_NAME: "TechCorp"                     │         │
│  │                                                 │         │
│  │ Method Definitions:                            │         │
│  │   - displayInfo()                              │         │
│  │   - raiseSalary(double)                        │         │
│  │   - getEmployeeCount()                         │         │
│  └────────────────────────────────────────────────┘         │
└─────────────────────────────────────────────────────────────┘
```

---

## Interfaces in Java

### What is an Interface?

An interface is a contract that defines a set of methods that implementing classes must provide. It represents "what" a class can do, not "how" it does it.

### Interface Evolution

```
┌─────────────────────────────────────────────────────────────┐
│              Interface Evolution in Java                     │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Java 7 and Earlier          Java 8+           Java 9+       │
│  ┌──────────────┐         ┌──────────────┐  ┌─────────────┐│
│  │ • Abstract   │         │ • Abstract   │  │ • Abstract  ││
│  │   methods    │         │   methods    │  │   methods   ││
│  │              │         │ • Default    │  │ • Default   ││
│  │ • Constants  │         │   methods    │  │   methods   ││
│  │              │         │ • Static     │  │ • Static    ││
│  │              │         │   methods    │  │   methods   ││
│  │              │         │ • Constants  │  │ • Private   ││
│  │              │         │              │  │   methods   ││
│  │              │         │              │  │ • Constants ││
│  └──────────────┘         └──────────────┘  └─────────────┘│
└─────────────────────────────────────────────────────────────┘
```

### Interface Examples

#### Traditional Interface (Java 7)

```java
public interface Drawable {
    // Abstract method (implicitly public abstract)
    void draw();
    
    // Constant (implicitly public static final)
    String TYPE = "2D";
}

class Circle implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }
}
```

#### Modern Interface (Java 8+)

```java
@FunctionalInterface
public interface Calculator {
    // Abstract method
    int calculate(int a, int b);
    
    // Default method (provides default implementation)
    default void printResult(int result) {
        System.out.println("Result: " + result);
    }
    
    // Static helper method
    static boolean isPositive(int number) {
        return number > 0;
    }
    
    // Private helper method (Java 9+)
    private void logOperation(String operation) {
        System.out.println("Performing: " + operation);
    }
}

// Implementation
class Addition implements Calculator {
    @Override
    public int calculate(int a, int b) {
        return a + b;
    }
}

// Usage
public class CalculatorDemo {
    public static void main(String[] args) {
        Calculator calc = new Addition();
        int result = calc.calculate(5, 3);
        calc.printResult(result);  // Uses default method
        
        boolean positive = Calculator.isPositive(result);  // Static method
        System.out.println("Is positive? " + positive);
    }
}
```

---

## Class vs Interface Comparison

### Detailed Comparison Table

| Feature | Class | Interface |
|---------|-------|-----------|
| **Keyword** | `class` | `interface` |
| **State** | Can have instance variables | No instance variables (only constants) |
| **Constructors** | Can have constructors | Cannot have constructors |
| **Method Implementation** | Can have concrete methods | Abstract methods (Java 7), default/static methods (Java 8+) |
| **Inheritance** | Single inheritance (`extends`) | Multiple inheritance (`implements`) |
| **Access Modifiers** | public, protected, private, default | public (implicitly) |
| **Instantiation** | Can be instantiated (if not abstract) | Cannot be instantiated |
| **Purpose** | Define objects with state and behavior | Define contracts/capabilities |

### Visual Comparison

```
┌─────────────────────────────────────────────────────────────┐
│                  Class vs Interface                          │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  CLASS                          INTERFACE                    │
│  ┌──────────────────┐          ┌──────────────────┐         │
│  │ State (Fields)   │          │ Constants Only   │         │
│  │ ✓ Instance vars  │          │ ✗ No instance    │         │
│  │ ✓ Static vars    │          │ ✓ Static final   │         │
│  ├──────────────────┤          ├──────────────────┤         │
│  │ Behavior         │          │ Behavior         │         │
│  │ ✓ Concrete       │          │ ✓ Abstract       │         │
│  │ ✓ Abstract       │          │ ✓ Default (J8+)  │         │
│  │                  │          │ ✓ Static (J8+)   │         │
│  ├──────────────────┤          ├──────────────────┤         │
│  │ Inheritance      │          │ Inheritance      │         │
│  │ Single (extends) │          │ Multiple         │         │
│  │                  │          │ (implements)     │         │
│  └──────────────────┘          └──────────────────┘         │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## Inheritance and Implementation

### Q1: Which class declaration is correct if A and B are classes and C and D are interfaces?

```java
// Options:
// a) class Z extends A implements C, D {}      ✓ CORRECT
// b) class Z extends A, B implements D {}      ✗ WRONG
// c) class Z extends C implements A, B {}      ✗ WRONG
// d) class Z extends C, D implements B {}      ✗ WRONG
```

**Answer: Option (a)** is correct.

### Inheritance Rules Diagram

```
┌─────────────────────────────────────────────────────────────┐
│              Java Inheritance Rules                          │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Single Class Inheritance        Multiple Interface          │
│                                  Inheritance                  │
│  ┌─────────┐                    ┌──────────┐                │
│  │ Class A │                    │Interface │                │
│  └────┬────┘                    │    C     │                │
│       │ extends                 └────┬─────┘                │
│       │ (only one)                   │                       │
│  ┌────▼────┐                         │ implements           │
│  │ Class Z │◄────────────────────────┤ (multiple)           │
│  └─────────┘                         │                       │
│                                  ┌───▼──────┐               │
│                                  │Interface │               │
│                                  │    D     │               │
│                                  └──────────┘               │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Complete Example

```java
// Parent class
class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    public void eat() {
        System.out.println(name + " is eating");
    }
}

// Interfaces
interface Swimmable {
    void swim();
    
    default void dive() {
        System.out.println("Diving into water");
    }
}

interface Flyable {
    void fly();
    
    default void land() {
        System.out.println("Landing on ground");
    }
}

// Child class with single inheritance and multiple interface implementation
class Duck extends Animal implements Swimmable, Flyable {
    
    public Duck(String name) {
        super(name);
    }
    
    @Override
    public void swim() {
        System.out.println(name + " is swimming");
    }
    
    @Override
    public void fly() {
        System.out.println(name + " is flying");
    }
}

// Usage
public class InheritanceDemo {
    public static void main(String[] args) {
        Duck duck = new Duck("Donald");
        
        // From Animal class
        duck.eat();
        
        // From Swimmable interface
        duck.swim();
        duck.dive();
        
        // From Flyable interface
        duck.fly();
        duck.land();
    }
}
```

**Output:**
```
Donald is eating
Donald is swimming
Diving into water
Donald is flying
Landing on ground
```

---

## Variable Shadowing

### Q2: What happens when a parent and a child class has the same variable name?

When both parent and child classes have fields with the same name, this is called **variable shadowing** or **variable hiding**.

### Variable Shadowing Diagram

```
┌─────────────────────────────────────────────────────────────┐
│              Variable Shadowing Behavior                     │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Compile Time (Static Type)                                  │
│  ┌──────────────────────────────────────────┐               │
│  │ Parent parent = new Child();             │               │
│  │ parent.value → Parent's value            │               │
│  └──────────────────────────────────────────┘               │
│                                                               │
│  ┌──────────────────────────────────────────┐               │
│  │ Child child = new Child();               │               │
│  │ child.value → Child's value              │               │
│  └──────────────────────────────────────────┘               │
│                                                               │
│  Key Point: Depends on REFERENCE TYPE, not OBJECT TYPE      │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Example Code

```java
class Parent {
    public String value = "Parent Value";
    
    public void display() {
        System.out.println("Parent display: " + value);
    }
}

class Child extends Parent {
    public String value = "Child Value";  // Shadows parent's value
    
    @Override
    public void display() {
        System.out.println("Child display: " + value);
        System.out.println("Parent's value via super: " + super.value);
    }
}

public class VariableShadowingDemo {
    public static void main(String[] args) {
        // Case 1: Parent reference, Child object
        Parent p = new Child();
        System.out.println("p.value = " + p.value);  // Parent Value (static type)
        p.display();  // Child display (dynamic dispatch)
        
        System.out.println("\n---\n");
        
        // Case 2: Child reference, Child object
        Child c = new Child();
        System.out.println("c.value = " + c.value);  // Child Value
        c.display();  // Child display
    }
}
```

**Output:**
```
p.value = Parent Value
Child display: Child Value
Parent's value via super: Parent Value

---

c.value = Child Value
Child display: Child Value
Parent's value via super: Parent Value
```

### Key Takeaway

```
┌─────────────────────────────────────────────────────────────┐
│  Variable Shadowing: Determined at COMPILE TIME             │
│  - Based on REFERENCE TYPE (static type)                    │
│  - NOT based on OBJECT TYPE (dynamic type)                  │
│                                                               │
│  Method Overriding: Determined at RUNTIME                   │
│  - Based on OBJECT TYPE (dynamic type)                      │
│  - NOT based on REFERENCE TYPE (static type)                │
└─────────────────────────────────────────────────────────────┘
```

---

## Method Overriding

### Q3: What happens when the parent and the child class has the same non-static method with the same signature?

This is **method overriding**, which enables **polymorphism**.

### Method Overriding Flow

```
┌─────────────────────────────────────────────────────────────┐
│              Method Overriding (Runtime Polymorphism)        │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Compile Time              Runtime                           │
│  ┌──────────────┐         ┌──────────────┐                  │
│  │ Check method │         │ Determine    │                  │
│  │ exists in    │────────►│ actual object│                  │
│  │ Parent class │         │ type         │                  │
│  └──────────────┘         └──────┬───────┘                  │
│                                   │                          │
│                                   ▼                          │
│                          ┌─────────────────┐                │
│                          │ Call overridden │                │
│                          │ method in Child │                │
│                          └─────────────────┘                │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Example Code

```java
class Shape {
    public void draw() {
        System.out.println("Drawing a shape");
    }
    
    public double area() {
        return 0.0;
    }
}

class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a circle with radius: " + radius);
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double width, height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a rectangle: " + width + "x" + height);
    }
    
    @Override
    public double area() {
        return width * height;
    }
}

public class PolymorphismDemo {
    public static void main(String[] args) {
        // Polymorphic array
        Shape[] shapes = {
            new Circle(5.0),
            new Rectangle(4.0, 6.0),
            new Circle(3.0)
        };
        
        // Polymorphic behavior
        for (Shape shape : shapes) {
            shape.draw();  // Calls overridden method based on actual object
            System.out.println("Area: " + shape.area());
            System.out.println();
        }
    }
}
```

**Output:**
```
Drawing a circle with radius: 5.0
Area: 78.53981633974483

Drawing a rectangle: 4.0x6.0
Area: 24.0

Drawing a circle with radius: 3.0
Area: 28.274333882308138
```

### Overriding Rules

```java
class OverridingRules {
    /*
     * Method Overriding Rules:
     * 
     * 1. Same method signature (name + parameters)
     * 2. Return type: Same or covariant (subtype)
     * 3. Access modifier: Same or more accessible
     * 4. Cannot override final methods
     * 5. Cannot override static methods (hiding instead)
     * 6. Can throw fewer or narrower checked exceptions
     */
}

// Example of covariant return type
class Animal {
    public Animal reproduce() {
        return new Animal();
    }
}

class Dog extends Animal {
    @Override
    public Dog reproduce() {  // Covariant return type
        return new Dog();
    }
}
```

---

## Static Method Hiding

### Q4: What happens when the parent and the child class has the same static method with the same signature?

Static methods are **hidden**, not overridden. The behavior is similar to variable shadowing.

### Static Method Hiding Diagram

```
┌─────────────────────────────────────────────────────────────┐
│              Static Method Hiding                            │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Parent parent = new Child();                                │
│  parent.staticMethod() → Calls Parent's static method        │
│                          (Based on REFERENCE TYPE)           │
│                                                               │
│  Child child = new Child();                                  │
│  child.staticMethod() → Calls Child's static method          │
│                         (Based on REFERENCE TYPE)            │
│                                                               │
│  ⚠️  NOT POLYMORPHIC - Determined at COMPILE TIME           │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Example Code

```java
class Parent {
    public static void staticMethod() {
        System.out.println("Parent static method");
    }
    
    public void instanceMethod() {
        System.out.println("Parent instance method");
    }
}

class Child extends Parent {
    // This HIDES parent's static method (not overrides)
    public static void staticMethod() {
        System.out.println("Child static method");
    }
    
    // This OVERRIDES parent's instance method
    @Override
    public void instanceMethod() {
        System.out.println("Child instance method");
    }
}

public class StaticHidingDemo {
    public static void main(String[] args) {
        Parent p = new Child();
        
        // Static method - based on reference type (compile-time)
        p.staticMethod();  // Parent static method
        
        // Instance method - based on object type (runtime)
        p.instanceMethod();  // Child instance method
        
        System.out.println("\n---\n");
        
        Child c = new Child();
        c.staticMethod();  // Child static method
        c.instanceMethod();  // Child instance method
        
        System.out.println("\n---\n");
        
        // Best practice: Call static methods via class name
        Parent.staticMethod();  // Parent static method
        Child.staticMethod();   // Child static method
    }
}
```

**Output:**
```
Parent static method
Child instance method

---

Child static method
Child instance method

---

Parent static method
Child static method
```

---

## Marker Interfaces

### Q8: What is a marker or tag interface? Why are there some interfaces with no defined methods?

**Marker interfaces** (also called **tag interfaces**) are interfaces with no methods that serve as metadata to indicate that a class has certain properties.

### Common Marker Interfaces

```
┌─────────────────────────────────────────────────────────────┐
│              Common Marker Interfaces                        │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  java.io.Serializable                                        │
│  └─► Indicates class can be serialized                       │
│                                                               │
│  java.lang.Cloneable                                         │
│  └─► Indicates class can be cloned                           │
│                                                               │
│  java.util.EventListener                                     │
│  └─► Marks event listener classes                            │
│                                                               │
│  java.rmi.Remote                                             │
│  └─► Marks remote objects in RMI                             │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Example: Serializable

```java
import java.io.*;

// Marker interface - no methods
class Employee implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private String name;
    private int id;
    private transient String password;  // Won't be serialized
    
    public Employee(String name, int id, String password) {
        this.name = name;
        this.id = id;
        this.password = password;
    }
    
    @Override
    public String toString() {
        return "Employee{name='" + name + "', id=" + id + 
               ", password='" + password + "'}";
    }
}

public class MarkerInterfaceDemo {
    public static void main(String[] args) {
        Employee emp = new Employee("John Doe", 101, "secret123");
        
        // Serialize
        try (ObjectOutputStream oos = new ObjectOutputStream(
                new FileOutputStream("employee.ser"))) {
            oos.writeObject(emp);
            System.out.println("Serialized: " + emp);
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        // Deserialize
        try (ObjectInputStream ois = new ObjectInputStream(
                new FileInputStream("employee.ser"))) {
            Employee deserializedEmp = (Employee) ois.readObject();
            System.out.println("Deserialized: " + deserializedEmp);
            // Note: password is null (transient field)
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

### Marker Interfaces vs Annotations

```
┌─────────────────────────────────────────────────────────────┐
│        Marker Interfaces vs Annotations (Java 5+)            │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Marker Interface              Annotation                    │
│  ┌──────────────────┐         ┌──────────────────┐          │
│  │ • Compile-time   │         │ • Runtime check  │          │
│  │   type checking  │         │   via reflection │          │
│  │ • Inherited by   │         │ • More flexible  │          │
│  │   subclasses     │         │ • Can have       │          │
│  │ • Less flexible  │         │   parameters     │          │
│  │ • Legacy approach│         │ • Modern approach│          │
│  └──────────────────┘         └──────────────────┘          │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Modern Alternative: Annotations

```java
// Custom marker annotation
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@interface Auditable {
    String value() default "";
}

@Auditable("Financial data")
class BankAccount {
    private double balance;
    
    public void deposit(double amount) {
        balance += amount;
    }
}

// Check at runtime
public class AnnotationDemo {
    public static void main(String[] args) {
        Class<BankAccount> clazz = BankAccount.class;
        
        if (clazz.isAnnotationPresent(Auditable.class)) {
            Auditable audit = clazz.getAnnotation(Auditable.class);
            System.out.println("This class is auditable: " + audit.value());
        }
    }
}
```

---

## Functional Interfaces

### Q9: What is a functional interface?

A **functional interface** is an interface with exactly one abstract method. Introduced in Java 8 to support lambda expressions and functional programming.

### Functional Interface Structure

```
┌─────────────────────────────────────────────────────────────┐
│              Functional Interface Components                 │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  @FunctionalInterface (optional but recommended)             │
│  ┌────────────────────────────────────────────┐             │
│  │ • Exactly ONE abstract method              │             │
│  │ • Any number of default methods            │             │
│  │ • Any number of static methods             │             │
│  │ • Methods from Object class don't count    │             │
│  └────────────────────────────────────────────┘             │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Built-in Functional Interfaces

```java
import java.util.function.*;

public class FunctionalInterfacesDemo {
    public static void main(String[] args) {
        
        // 1. Predicate<T> - takes T, returns boolean
        Predicate<Integer> isEven = n -> n % 2 == 0;
        System.out.println("Is 4 even? " + isEven.test(4));
        
        // 2. Function<T, R> - takes T, returns R
        Function<String, Integer> stringLength = s -> s.length();
        System.out.println("Length of 'Hello': " + stringLength.apply("Hello"));
        
        // 3. Consumer<T> - takes T, returns void
        Consumer<String> printer = s -> System.out.println("Printing: " + s);
        printer.accept("Hello World");
        
        // 4. Supplier<T> - takes nothing, returns T
        Supplier<Double> randomSupplier = () -> Math.random();
        System.out.println("Random number: " + randomSupplier.get());
        
        // 5. BiFunction<T, U, R> - takes T and U, returns R
        BiFunction<Integer, Integer, Integer> adder = (a, b) -> a + b;
        System.out.println("5 + 3 = " + adder.apply(5, 3));
        
        // 6. UnaryOperator<T> - takes T, returns T
        UnaryOperator<Integer> square = n -> n * n;
        System.out.println("Square of 5: " + square.apply(5));
    }
}
```

### Custom Functional Interface

```java
@FunctionalInterface
interface MathOperation {
    // Single abstract method
    int operate(int a, int b);
    
    // Default methods are allowed
    default void printResult(int result) {
        System.out.println("Result: " + result);
    }
    
    // Static methods are allowed
    static MathOperation getAddition() {
        return (a, b) -> a + b;
    }
    
    // Methods from Object class don't count
    @Override
    boolean equals(Object obj);
}

public class CustomFunctionalInterfaceDemo {
    public static void main(String[] args) {
        // Lambda expression
        MathOperation addition = (a, b) -> a + b;
        MathOperation multiplication = (a, b) -> a * b;
        
        int sum = addition.operate(5, 3);
        addition.printResult(sum);
        
        int product = multiplication.operate(5, 3);
        multiplication.printResult(product);
        
        // Using static factory method
        MathOperation add = MathOperation.getAddition();
        add.printResult(add.operate(10, 20));
    }
}
```

### @FunctionalInterface Annotation

```java
// Q10: What does @FunctionalInterface do?

@FunctionalInterface
interface ValidFunctionalInterface {
    void singleMethod();  // ✓ Valid - exactly one abstract method
}

@FunctionalInterface
interface InvalidFunctionalInterface {
    void method1();
    void method2();  // ✗ Compile error - more than one abstract method
}

/*
 * @FunctionalInterface annotation:
 * 
 * 1. Documents that interface is intended to be functional
 * 2. Compiler checks that interface has exactly one abstract method
 * 3. Similar to @Override - helps catch errors at compile time
 * 4. Makes code more readable and maintainable
 */
```

---

## Diamond Problem

### Q10: Does Java 8 solve the "diamond problem"? If yes, how?

**Partially yes.** Java 8 allows default methods in interfaces, which introduces a form of multiple behavioral inheritance.

### Diamond Problem Illustration

```
┌─────────────────────────────────────────────────────────────┐
│              The Diamond Problem                             │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Traditional Multiple Inheritance (Not in Java)              │
│                                                               │
│                    ┌─────────┐                               │
│                    │  Shape  │                               │
│                    │ draw()  │                               │
│                    └────┬────┘                               │
│                         │                                    │
│              ┌──────────┴──────────┐                         │
│              │                     │                         │
│         ┌────▼────┐           ┌───▼─────┐                   │
│         │ Circle  │           │ Square  │                   │
│         │ draw()  │           │ draw()  │                   │
│         └────┬────┘           └───┬─────┘                   │
│              │                     │                         │
│              └──────────┬──────────┘                         │
│                         │                                    │
│                    ┌────▼────────┐                           │
│                    │CircleSquare │                           │
│                    │   draw()?   │ ← Which draw() to call?  │
│                    └─────────────┘                           │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Java's Solution with Interfaces

```java
// Java 8+ with default methods
interface Drawable {
    default void draw() {
        System.out.println("Drawing from Drawable");
    }
}

interface Printable {
    default void draw() {
        System.out.println("Drawing from Printable");
    }
}

// Compiler error: class inherits unrelated defaults for draw()
// Must explicitly resolve the conflict
class Document implements Drawable, Printable {
    @Override
    public void draw() {
        // Option 1: Provide own implementation
        System.out.println("Drawing Document");
        
        // Option 2: Call specific interface method
        // Drawable.super.draw();
        
        // Option 3: Call both
        // Drawable.super.draw();
        // Printable.super.draw();
    }
}

public class DiamondProblemDemo {
    public static void main(String[] args) {
        Document doc = new Document();
        doc.draw();
    }
}
```

### Resolution Rules

```
┌─────────────────────────────────────────────────────────────┐
│        Diamond Problem Resolution Rules (Java 8+)            │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Rule 1: Classes win over interfaces                         │
│  └─► If a class has a method, it takes precedence           │
│                                                               │
│  Rule 2: More specific interface wins                        │
│  └─► If one interface extends another, child wins           │
│                                                               │
│  Rule 3: Explicit resolution required                        │
│  └─► If conflict remains, class must override               │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Complete Example

```java
interface A {
    default void hello() {
        System.out.println("Hello from A");
    }
}

interface B extends A {
    @Override
    default void hello() {
        System.out.println("Hello from B");
    }
}

interface C extends A {
    @Override
    default void hello() {
        System.out.println("Hello from C");
    }
}

// Case 1: More specific interface wins (Rule 2)
class D implements A, B {
    // No need to override - B is more specific than A
}

// Case 2: Must explicitly resolve (Rule 3)
class E implements B, C {
    @Override
    public void hello() {
        // Must choose which one to call
        B.super.hello();
        // Or provide own implementation
    }
}

// Case 3: Class wins (Rule 1)
class F implements A {
    @Override
    public void hello() {
        System.out.println("Hello from F");
    }
}

class G extends F implements A {
    // F's hello() wins over A's default hello()
}

public class DiamondResolutionDemo {
    public static void main(String[] args) {
        new D().hello();  // Hello from B
        new E().hello();  // Hello from B
        new G().hello();  // Hello from F
    }
}
```

---

## Interview Questions

### Q1: When should you use an abstract class vs an interface?

**Use Abstract Class when:**
- You want to share code among closely related classes
- Classes have common state (instance variables)
- You need non-public members (protected, private)
- You want to provide default behavior with state

**Use Interface when:**
- You want to define a contract for unrelated classes
- You need multiple inheritance of type
- You want to specify behavior without implementation details
- You're designing for flexibility and loose coupling

### Q2: Can an interface extend multiple interfaces?

**Yes!** An interface can extend multiple interfaces.

```java
interface A {
    void methodA();
}

interface B {
    void methodB();
}

interface C extends A, B {
    void methodC();
}

class Implementation implements C {
    @Override
    public void methodA() { }
    
    @Override
    public void methodB() { }
    
    @Override
    public void methodC() { }
}
```

### Q3: What are the differences between Java 7 and Java 8 interfaces?

```
┌─────────────────────────────────────────────────────────────┐
│              Java 7 vs Java 8 Interfaces                     │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Java 7                        Java 8+                       │
│  ┌──────────────────┐         ┌──────────────────┐          │
│  │ • Abstract       │         │ • Abstract       │          │
│  │   methods only   │         │   methods        │          │
│  │                  │         │ • Default methods│          │
│  │ • Constants      │         │ • Static methods │          │
│  │                  │         │ • Constants      │          │
│  │                  │         │                  │          │
│  │ • No method body │         │ • Method bodies  │          │
│  │                  │         │   allowed        │          │
│  └──────────────────┘         └──────────────────┘          │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## Best Practices

### 1. Favor Composition Over Inheritance

```java
// ❌ Poor: Using inheritance for code reuse
class ArrayList extends Vector {
    // Inherits unnecessary synchronized methods
}

// ✓ Good: Using composition
class MyList {
    private List<String> items = new ArrayList<>();
    
    public void add(String item) {
        items.add(item);
    }
}
```

### 2. Program to Interfaces

```java
// ❌ Poor: Programming to implementation
ArrayList<String> list = new ArrayList<>();

// ✓ Good: Programming to interface
List<String> list = new ArrayList<>();
```

### 3. Use @Override Annotation

```java
class Parent {
    public void process() { }
}

class Child extends Parent {
    @Override  // ✓ Helps catch errors
    public void process() { }
}
```

### 4. Keep Interfaces Focused (Interface Segregation Principle)

```java
// ❌ Poor: Fat interface
interface Worker {
    void work();
    void eat();
    void sleep();
}

// ✓ Good: Segregated interfaces
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}
```

### 5. Use Functional Interfaces for Single Method Contracts

```java
// ✓ Good: Functional interface for callbacks
@FunctionalInterface
interface Callback {
    void onComplete(String result);
}

class AsyncTask {
    public void execute(Callback callback) {
        // Do work...
        callback.onComplete("Done");
    }
}

// Usage with lambda
new AsyncTask().execute(result -> System.out.println(result));
```

---

## Summary

### Key Concepts Checklist

- ✓ Classes define objects with state and behavior
- ✓ Interfaces define contracts without state
- ✓ Java supports single class inheritance
- ✓ Java supports multiple interface inheritance
- ✓ Variable shadowing is compile-time (static type)
- ✓ Method overriding is runtime (dynamic type)
- ✓ Static methods are hidden, not overridden
- ✓ Marker interfaces indicate metadata
- ✓ Functional interfaces enable lambda expressions
- ✓ Java 8+ partially solves diamond problem with default methods

---

## 📚 Related Topics

- [Abstract Classes vs Interfaces](abstract-vs-interface.md)
- [Class Loading Mechanism](class-loading.md)
- [OOP Principles](../Module%2006%20-%20OOP%20and%20FP/oop-principles.md)
- [Polymorphism](../Module%2006%20-%20OOP%20and%20FP/polymorphism.md)

---

## 💡 Practice Exercises

1. Create a class hierarchy with proper inheritance
2. Implement multiple interfaces in a single class
3. Demonstrate variable shadowing vs method overriding
4. Create custom functional interfaces with lambda expressions
5. Resolve diamond problem conflicts with default methods

---

**[⬆ Back to Top](#)**

**Last Updated**: 2026-06-03