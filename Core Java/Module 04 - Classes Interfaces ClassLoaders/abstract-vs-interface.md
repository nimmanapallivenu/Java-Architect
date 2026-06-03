# Abstract Classes vs Interfaces

> **Module**: Classes Interfaces ClassLoaders  
> **Topic**: Abstract Classes vs Interfaces - Deep Dive with Visual Comparisons

---

## 📋 Table of Contents

- [Introduction](#introduction)
- [Abstract Classes](#abstract-classes)
- [Interfaces](#interfaces)
- [Detailed Comparison](#detailed-comparison)
- [When to Use What](#when-to-use-what)
- [Diamond Problem](#diamond-problem)
- [Design Patterns](#design-patterns)
- [Code Reuse Strategies](#code-reuse-strategies)
- [Best Practices](#best-practices)

---

## Introduction

Understanding when to use abstract classes versus interfaces is crucial for effective object-oriented design in Java.

### Quick Comparison

```
┌─────────────────────────────────────────────────────────────┐
│         Abstract Class vs Interface at a Glance              │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Abstract Class              Interface                       │
│  ┌──────────────┐           ┌──────────────┐                │
│  │ "IS-A"       │           │ "CAN-DO"     │                │
│  │ Relationship │           │ Capability   │                │
│  └──────────────┘           └──────────────┘                │
│                                                               │
│  • Partial impl.             • Contract only                 │
│  • Has state                 • No state                      │
│  • Single inherit.           • Multiple inherit.             │
│  • Constructor               • No constructor                │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## Abstract Classes

### Purpose of Abstract Classes

```
┌─────────────────────────────────────────────────────────────┐
│           Purpose of Abstract Classes                        │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  1. Code Reuse                                               │
│     └─► Share common implementation among subclasses        │
│                                                               │
│  2. Partial Implementation                                   │
│     └─► Provide default behavior, force specific impl.      │
│                                                               │
│  3. Template Method Pattern                                  │
│     └─► Define algorithm skeleton, let subclasses fill gaps │
│                                                               │
│  4. Polymorphism                                             │
│     └─► Upcast to abstract type for polymorphic behavior    │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Abstract Class Structure

```java
/**
 * Complete abstract class demonstrating all features
 */
public abstract class Vehicle {
    // 1. Instance variables (state)
    private String brand;
    private int year;
    
    // 2. Static variables
    private static int vehicleCount = 0;
    
    // 3. Constants
    public static final int MAX_SPEED = 200;
    
    // 4. Constructor
    public Vehicle(String brand, int year) {
        this.brand = brand;
        this.year = year;
        vehicleCount++;
    }
    
    // 5. Abstract methods (must be implemented by subclasses)
    public abstract void start();
    public abstract void stop();
    public abstract double calculateFuelEfficiency();
    
    // 6. Concrete methods (inherited by subclasses)
    public void displayInfo() {
        System.out.println("Brand: " + brand + ", Year: " + year);
    }
    
    // 7. Protected methods (accessible to subclasses)
    protected void performMaintenance() {
        System.out.println("Performing maintenance on " + brand);
    }
    
    // 8. Static methods
    public static int getVehicleCount() {
        return vehicleCount;
    }
    
    // 9. Getters
    public String getBrand() { return brand; }
    public int getYear() { return year; }
}

// Concrete implementation
class Car extends Vehicle {
    private int numberOfDoors;
    
    public Car(String brand, int year, int doors) {
        super(brand, year);
        this.numberOfDoors = doors;
    }
    
    @Override
    public void start() {
        System.out.println("Car starting with ignition");
    }
    
    @Override
    public void stop() {
        System.out.println("Car stopping with brake");
    }
    
    @Override
    public double calculateFuelEfficiency() {
        return 25.5; // MPG
    }
}
```

### Template Method Pattern

```java
/**
 * Template Method Design Pattern
 * Defines algorithm skeleton, subclasses fill in details
 */
public abstract class DataProcessor {
    
    // Template method - defines the algorithm structure
    public final void process() {
        readData();
        validateData();
        processData();
        saveData();
        sendNotification();
    }
    
    // Abstract methods - must be implemented
    protected abstract void readData();
    protected abstract void processData();
    
    // Concrete methods with default implementation
    protected void validateData() {
        System.out.println("Validating data with default rules");
    }
    
    protected void saveData() {
        System.out.println("Saving data to default storage");
    }
    
    // Hook method - optional override
    protected void sendNotification() {
        // Default: do nothing
    }
}

// CSV implementation
class CSVDataProcessor extends DataProcessor {
    @Override
    protected void readData() {
        System.out.println("Reading CSV file");
    }
    
    @Override
    protected void processData() {
        System.out.println("Processing CSV data");
    }
    
    @Override
    protected void sendNotification() {
        System.out.println("Sending email notification");
    }
}

// JSON implementation
class JSONDataProcessor extends DataProcessor {
    @Override
    protected void readData() {
        System.out.println("Reading JSON file");
    }
    
    @Override
    protected void processData() {
        System.out.println("Processing JSON data");
    }
    
    @Override
    protected void validateData() {
        System.out.println("Validating JSON schema");
    }
}
```

---

## Interfaces

### Interface Evolution

```
┌─────────────────────────────────────────────────────────────┐
│              Interface Evolution in Java                     │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Java 7              Java 8              Java 9              │
│  ┌──────────┐      ┌──────────┐       ┌──────────┐         │
│  │ Abstract │      │ Abstract │       │ Abstract │         │
│  │ methods  │      │ methods  │       │ methods  │         │
│  │          │      │ Default  │       │ Default  │         │
│  │ Constants│      │ methods  │       │ methods  │         │
│  │          │      │ Static   │       │ Static   │         │
│  │          │      │ methods  │       │ methods  │         │
│  │          │      │ Constants│       │ Private  │         │
│  │          │      │          │       │ methods  │         │
│  │          │      │          │       │ Constants│         │
│  └──────────┘      └──────────┘       └──────────┘         │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Modern Interface Structure

```java
public interface PaymentProcessor {
    
    // 1. Constants (implicitly public static final)
    String PAYMENT_GATEWAY = "Stripe";
    int MAX_RETRY_ATTEMPTS = 3;
    
    // 2. Abstract methods (implicitly public abstract)
    boolean processPayment(double amount);
    void refund(String transactionId);
    
    // 3. Default methods (Java 8+)
    default void logTransaction(String message) {
        System.out.println("[" + getCurrentTimestamp() + "] " + message);
    }
    
    default boolean validateAmount(double amount) {
        return amount > 0 && amount < 10000;
    }
    
    // 4. Static methods (Java 8+)
    static String generateTransactionId() {
        return "TXN-" + System.currentTimeMillis();
    }
    
    // 5. Private helper methods (Java 9+)
    private String getCurrentTimestamp() {
        return java.time.LocalDateTime.now().toString();
    }
}
```

### Multiple Interface Implementation

```java
// Multiple capabilities through interfaces
interface Flyable {
    void fly();
    default void takeOff() {
        System.out.println("Taking off...");
    }
}

interface Swimmable {
    void swim();
    default void dive() {
        System.out.println("Diving...");
    }
}

interface Walkable {
    void walk();
}

// Duck can do all three
class Duck implements Flyable, Swimmable, Walkable {
    private String name;
    
    public Duck(String name) {
        this.name = name;
    }
    
    @Override
    public void fly() {
        System.out.println(name + " is flying");
    }
    
    @Override
    public void swim() {
        System.out.println(name + " is swimming");
    }
    
    @Override
    public void walk() {
        System.out.println(name + " is walking");
    }
}
```

---

## Detailed Comparison

### Feature Comparison Table

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Abstract Class vs Interface                              │
├──────────────────────┬──────────────────────┬───────────────────────────────┤
│ Feature              │ Abstract Class       │ Interface                     │
├──────────────────────┼──────────────────────┼───────────────────────────────┤
│ Keyword              │ abstract class       │ interface                     │
│ Instance Variables   │ ✓ Yes                │ ✗ No (only constants)         │
│ Constructors         │ ✓ Yes                │ ✗ No                          │
│ Abstract Methods     │ ✓ Yes                │ ✓ Yes                         │
│ Concrete Methods     │ ✓ Yes                │ ✓ Yes (default, Java 8+)      │
│ Static Methods       │ ✓ Yes                │ ✓ Yes (Java 8+)               │
│ Private Methods      │ ✓ Yes                │ ✓ Yes (Java 9+)               │
│ Access Modifiers     │ All types            │ public only (implicitly)      │
│ Multiple Inheritance │ ✗ No (single)        │ ✓ Yes (multiple)              │
│ Instantiation        │ ✗ No                 │ ✗ No                          │
│ Inheritance Keyword  │ extends              │ implements                    │
│ Purpose              │ Code reuse + state   │ Define contract/capability    │
│ Relationship         │ "IS-A"               │ "CAN-DO"                      │
└──────────────────────┴──────────────────────┴───────────────────────────────┘
```

### Key Differences

```
┌─────────────────────────────────────────────────────────────┐
│              Key Differences Summary                         │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  1. STATE                                                    │
│     Abstract Class: ✓ Can maintain state                    │
│     Interface:      ✗ Cannot maintain state                 │
│                                                               │
│  2. INHERITANCE                                              │
│     Abstract Class: Single inheritance only                  │
│     Interface:      Multiple inheritance allowed             │
│                                                               │
│  3. CONSTRUCTOR                                              │
│     Abstract Class: ✓ Can have constructors                 │
│     Interface:      ✗ Cannot have constructors              │
│                                                               │
│  4. ACCESS CONTROL                                           │
│     Abstract Class: Full access control                      │
│     Interface:      Public only (implicitly)                 │
│                                                               │
│  5. WHEN TO USE                                              │
│     Abstract Class: Related classes, shared state/behavior   │
│     Interface:      Unrelated classes, define capabilities   │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## When to Use What

### Decision Flow Chart

```
┌─────────────────────────────────────────────────────────────┐
│           When to Use Abstract Class vs Interface            │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│                    Start Here                                │
│                        │                                     │
│                        ▼                                     │
│         ┌──────────────────────────┐                        │
│         │ Need to maintain state?  │                        │
│         └──────┬───────────┬───────┘                        │
│                │ Yes       │ No                              │
│                ▼           ▼                                 │
│         ┌──────────┐  ┌──────────────────┐                 │
│         │ Abstract │  │ Multiple inherit?│                 │
│         │  Class   │  └────┬────────┬────┘                 │
│         └──────────┘       │ Yes    │ No                   │
│                            ▼        ▼                       │
│                      ┌──────────┐ ┌──────────┐             │
│                      │Interface │ │ Either   │             │
│                      └──────────┘ │ works    │             │
│                                   └──────────┘             │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Use Abstract Class When

```java
/**
 * Use Abstract Class When:
 * 1. Share code among closely related classes
 * 2. Classes have common state (instance variables)
 * 3. Need non-public members (protected, private)
 * 4. Want to provide default behavior with state
 */

abstract class Employee {
    // Shared state
    private String name;
    private String employeeId;
    private double baseSalary;
    
    public Employee(String name, String id, double salary) {
        this.name = name;
        this.employeeId = id;
        this.baseSalary = salary;
    }
    
    // Abstract method - each type calculates differently
    public abstract double calculateSalary();
    
    // Concrete method - shared behavior
    public void displayInfo() {
        System.out.println("Name: " + name);
        System.out.println("ID: " + employeeId);
        System.out.println("Salary: $" + calculateSalary());
    }
    
    protected double getBaseSalary() {
        return baseSalary;
    }
}

class FullTimeEmployee extends Employee {
    private double bonus;
    
    public FullTimeEmployee(String name, String id, double salary, double bonus) {
        super(name, id, salary);
        this.bonus = bonus;
    }
    
    @Override
    public double calculateSalary() {
        return getBaseSalary() + bonus;
    }
}
```

### Use Interface When

```java
/**
 * Use Interface When:
 * 1. Define contract for unrelated classes
 * 2. Need multiple inheritance of type
 * 3. Specify behavior without implementation
 * 4. Design for flexibility and loose coupling
 */

interface Sortable {
    int compareTo(Sortable other);
    
    default void printSortInfo() {
        System.out.println("This object is sortable");
    }
}

class Product implements Sortable {
    private String name;
    private double price;
    
    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
    
    @Override
    public int compareTo(Sortable other) {
        if (other instanceof Product) {
            return Double.compare(this.price, ((Product) other).price);
        }
        return 0;
    }
}

class Student implements Sortable {
    private String name;
    private double gpa;
    
    public Student(String name, double gpa) {
        this.name = name;
        this.gpa = gpa;
    }
    
    @Override
    public int compareTo(Sortable other) {
        if (other instanceof Student) {
            return Double.compare(this.gpa, ((Student) other).gpa);
        }
        return 0;
    }
}
```

---

## Diamond Problem

### The Classic Diamond Problem

```
┌─────────────────────────────────────────────────────────────┐
│              The Diamond Problem                             │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Multiple Implementation Inheritance (NOT in Java)           │
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
│                    │   draw()?   │ ← AMBIGUITY!             │
│                    └─────────────┘                           │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Java's Solution

```java
// ❌ NOT allowed in Java
// class CircleSquare extends Circle, Square { }

// ✓ Allowed - multiple interface inheritance
class Document implements Drawable, Printable {
    // Must resolve conflicts
}
```

### Diamond Problem with Default Methods

```java
interface Flyable {
    default void move() {
        System.out.println("Flying through air");
    }
}

interface Swimmable {
    default void move() {
        System.out.println("Swimming through water");
    }
}

// Must resolve the conflict
class FlyingFish implements Flyable, Swimmable {
    @Override
    public void move() {
        System.out.println("FlyingFish can do both:");
        Flyable.super.move();
        Swimmable.super.move();
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

---

## Design Patterns

### Template Method Pattern (Abstract Class)

```java
abstract class ReportGenerator {
    // Template method - final to prevent override
    public final void generateReport() {
        fetchData();
        formatData();
        if (shouldIncludeCharts()) {
            generateCharts();
        }
        exportReport();
    }
    
    protected abstract void fetchData();
    protected abstract void formatData();
    protected abstract void exportReport();
    
    protected boolean shouldIncludeCharts() {
        return true;
    }
    
    protected void generateCharts() {
        System.out.println("Generating charts");
    }
}

class PDFReportGenerator extends ReportGenerator {
    @Override
    protected void fetchData() {
        System.out.println("Fetching from database");
    }
    
    @Override
    protected void formatData() {
        System.out.println("Formatting for PDF");
    }
    
    @Override
    protected void exportReport() {
        System.out.println("Exporting as PDF");
    }
}
```

### Strategy Pattern (Interface)

```java
interface PaymentStrategy {
    boolean pay(double amount);
    
    default void printReceipt(double amount) {
        System.out.println("Payment of $" + amount + " processed");
    }
}

class CreditCardStrategy implements PaymentStrategy {
    private String cardNumber;
    
    public CreditCardStrategy(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public boolean pay(double amount) {
        System.out.println("Paying $" + amount + " with credit card");
        return true;
    }
}

class PayPalStrategy implements PaymentStrategy {
    private String email;
    
    public PayPalStrategy(String email) {
        this.email = email;
    }
    
    @Override
    public boolean pay(double amount) {
        System.out.println("Paying $" + amount + " with PayPal");
        return true;
    }
}

// Context
class ShoppingCart {
    private PaymentStrategy paymentStrategy;
    
    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.paymentStrategy = strategy;
    }
    
    public void checkout(double amount) {
        if (paymentStrategy.pay(amount)) {
            paymentStrategy.printReceipt(amount);
        }
    }
}
```

---

## Code Reuse Strategies

### Three Approaches

```
┌─────────────────────────────────────────────────────────────┐
│              Code Reuse Strategies                           │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  1. Implementation Inheritance (Abstract Classes)            │
│     ┌──────────────┐                                         │
│     │ Abstract     │                                         │
│     │ Base Class   │                                         │
│     └──────┬───────┘                                         │
│            │ extends                                         │
│     ┌──────▼───────┐                                         │
│     │ Concrete     │                                         │
│     │ Subclass     │                                         │
│     └──────────────┘                                         │
│                                                               │
│  2. Composition + Interface Inheritance                      │
│     ┌──────────────┐      ┌──────────────┐                  │
│     │  Interface   │      │   Helper     │                  │
│     └──────┬───────┘      │   Class      │                  │
│            │ implements   └──────────────┘                  │
│     ┌──────▼───────┐             ▲                          │
│     │   Class      │─────────────┘ has-a                    │
│     └──────────────┘                                         │
│                                                               │
│  3. Delegation                                               │
│     ┌──────────────┐      ┌──────────────┐                  │
│     │   Class A    │─────►│   Class B    │                  │
│     │              │ uses │              │                  │
│     └──────────────┘      └──────────────┘                  │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Why Favor Composition?

```
┌─────────────────────────────────────────────────────────────┐
│      Composition vs Inheritance Trade-offs                   │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Inheritance                    Composition                  │
│  ┌──────────────────┐          ┌──────────────────┐         │
│  │ ✓ Simple syntax  │          │ ✓ More flexible  │         │
│  │ ✓ Polymorphism   │          │ ✓ Runtime change │         │
│  │ ✗ Tight coupling │          │ ✓ Loose coupling │         │
│  │ ✗ Fragile base   │          │ ✓ Better testing │         │
│  │ ✗ Compile-time   │          │ ✓ Easier to mock │         │
│  └──────────────────┘          └──────────────────┘         │
│                                                               │
│  GoF Design Patterns favor: Composition + Interface          │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## Best Practices

### 1. Choose Based on Relationship

```java
// ✓ Good: "IS-A" relationship → Abstract Class
abstract class Animal {
    protected String name;
    public abstract void makeSound();
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
}

// ✓ Good: "CAN-DO" capability → Interface
interface Swimmable {
    void swim();
}

class Fish implements Swimmable {
    @Override
    public void swim() {
        System.out.println("Swimming");
    }
}
```

### 2. Prefer Interfaces for Flexibility

```java
// ✓ Good: Program to interface
List<String> list = new ArrayList<>();

// ❌ Poor: Program to implementation
ArrayList<String> list = new ArrayList<>();
```

### 3. Use Abstract Classes for Shared State

```java
// ✓ Good: Abstract class when state is needed
abstract class Connection {
    protected boolean isOpen;
    protected String url;
    
    public abstract void connect();
    
    public boolean isConnected() {
        return isOpen;
    }
}
```

### 4. Keep Interfaces Focused

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
```

### 5. Document Design Decisions

```java
/**
 * Abstract class used for Template Method pattern.
 * Provides common workflow for all report generators.
 * Subclasses must implement data fetching and formatting.
 */
abstract class ReportGenerator {
    // Implementation
}

/**
 * Interface defining payment processing contract.
 * Implementations can use different payment gateways.
 * Supports Strategy pattern for flexible payment methods.
 */
interface PaymentProcessor {
    // Contract
}
```

---

## Summary

### Key Takeaways

- ✓ Abstract classes provide partial implementation with state
- ✓ Interfaces define contracts without state
- ✓ Use abstract classes for "IS-A" relationships
- ✓ Use interfaces for "CAN-DO" capabilities
- ✓ Java 8+ allows default methods in interfaces
- ✓ Diamond problem partially solved with resolution rules
- ✓ Favor composition over inheritance
- ✓ Choose based on your specific design needs

---

## 📚 Related Topics

- [Classes & Interfaces](classes-interfaces.md)
- [Class Loading Mechanism](class-loading.md)
- [OOP Principles](../Module%2006%20-%20OOP%20and%20FP/oop-principles.md)
- [Design Patterns](../Module%2015%20-%20Design%20Patterns/)

---

**[⬆ Back to Top](#)**

**Last Updated**: 2026-06-03