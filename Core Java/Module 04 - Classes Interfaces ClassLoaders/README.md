# Module 04: Classes, Interfaces & ClassLoaders

> **Core Java Learning Path** | Advanced OOP Concepts and Runtime Mechanisms

---

## 📚 Module Overview

This module provides comprehensive coverage of Java's fundamental building blocks: Classes, Interfaces, and the ClassLoader mechanism. Understanding these concepts is essential for designing robust, maintainable Java applications and troubleshooting runtime issues.

### What You'll Learn

```
┌─────────────────────────────────────────────────────────────┐
│              Module 04 Learning Path                         │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  1. Classes & Interfaces                                     │
│     ├─► Class structure and components                      │
│     ├─► Interface evolution (Java 7 to Java 9+)             │
│     ├─► Multiple inheritance with interfaces                │
│     ├─► Variable shadowing vs method overriding             │
│     └─► Functional interfaces and lambda expressions        │
│                                                               │
│  2. Abstract Classes vs Interfaces                           │
│     ├─► When to use abstract classes                        │
│     ├─► When to use interfaces                              │
│     ├─► Diamond problem and resolution                      │
│     ├─► Design patterns (Template Method, Strategy)         │
│     └─► Code reuse strategies                               │
│                                                               │
│  3. Class Loading Mechanism                                  │
│     ├─► Class loader hierarchy                              │
│     ├─► Parent delegation model                             │
│     ├─► Static vs dynamic loading                           │
│     ├─► Custom class loaders                                │
│     └─► Troubleshooting class loading issues                │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## 📖 Topics Covered

### 1. [Classes & Interfaces](classes-interfaces.md)

Comprehensive guide to Java classes and interfaces with visual diagrams and practical examples.

**Key Topics:**
- ✓ Class structure and memory layout
- ✓ Interface evolution (Java 7, 8, 9+)
- ✓ Multiple interface implementation
- ✓ Variable shadowing vs method overriding
- ✓ Static method hiding
- ✓ Marker interfaces
- ✓ Functional interfaces
- ✓ Diamond problem with default methods

**Highlights:**
```java
// Modern interface with default and static methods
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
    
    default void printResult(int result) {
        System.out.println("Result: " + result);
    }
    
    static boolean isPositive(int number) {
        return number > 0;
    }
}
```

---

### 2. [Abstract Classes vs Interfaces](abstract-vs-interface.md)

Deep dive into the differences, use cases, and design patterns for abstract classes and interfaces.

**Key Topics:**
- ✓ Detailed feature comparison
- ✓ When to use abstract classes
- ✓ When to use interfaces
- ✓ Diamond problem resolution
- ✓ Template Method pattern
- ✓ Strategy pattern
- ✓ Composition vs inheritance
- ✓ Java 8+ changes

**Decision Flow:**
```
Need to maintain state? ──► Yes ──► Abstract Class
                      │
                      └─► No ──► Multiple inheritance needed?
                                 │
                                 ├─► Yes ──► Interface
                                 └─► No ──► Either works
```

---

### 3. [Class Loading Mechanism](class-loading.md)

Complete guide to Java's class loading mechanism with hierarchy diagrams and troubleshooting tips.

**Key Topics:**
- ✓ Class loading phases (Loading, Linking, Initialization)
- ✓ Class loader hierarchy (Bootstrap, Extension, System, Custom)
- ✓ Parent delegation model
- ✓ Static vs dynamic class loading
- ✓ Class.forName() vs ClassLoader.loadClass()
- ✓ Custom class loaders
- ✓ Common exceptions and errors
- ✓ Debugging techniques

**Class Loader Hierarchy:**
```
Bootstrap ClassLoader (Native)
    │
    └─► Extension/Platform ClassLoader
            │
            └─► System/Application ClassLoader
                    │
                    └─► Custom ClassLoader
```

---

## 🎯 Learning Objectives

After completing this module, you will be able to:

### Classes & Interfaces
- [ ] Design classes with proper encapsulation
- [ ] Create and implement interfaces effectively
- [ ] Understand variable shadowing vs method overriding
- [ ] Use functional interfaces with lambda expressions
- [ ] Resolve diamond problem conflicts
- [ ] Choose between classes and interfaces appropriately

### Abstract Classes vs Interfaces
- [ ] Decide when to use abstract classes vs interfaces
- [ ] Apply Template Method design pattern
- [ ] Implement Strategy design pattern
- [ ] Understand composition vs inheritance trade-offs
- [ ] Leverage Java 8+ default methods
- [ ] Design flexible and maintainable APIs

### Class Loading
- [ ] Understand class loading phases
- [ ] Explain parent delegation model
- [ ] Differentiate static and dynamic loading
- [ ] Create custom class loaders
- [ ] Debug ClassNotFoundException and NoClassDefFoundError
- [ ] Optimize class loading performance
- [ ] Load resources from classpath

---

## 🔑 Key Concepts

### Classes vs Interfaces

| Aspect | Class | Interface |
|--------|-------|-----------|
| **State** | Can have instance variables | Only constants |
| **Constructor** | Yes | No |
| **Inheritance** | Single (extends) | Multiple (implements) |
| **Methods** | Concrete + abstract | Abstract + default + static |
| **Purpose** | Define objects | Define contracts |
| **Relationship** | "IS-A" | "CAN-DO" |

### Abstract Class vs Interface

```
┌─────────────────────────────────────────────────────────────┐
│         When to Use Abstract Class vs Interface              │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  Abstract Class                Interface                     │
│  ┌──────────────────┐         ┌──────────────────┐          │
│  │ • Related classes│         │ • Unrelated      │          │
│  │ • Shared state   │         │   classes        │          │
│  │ • Code reuse     │         │ • Define contract│          │
│  │ • Template Method│         │ • Multiple       │          │
│  │   pattern        │         │   inheritance    │          │
│  │ • "IS-A"         │         │ • "CAN-DO"       │          │
│  └──────────────────┘         └──────────────────┘          │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

### Class Loading Phases

```
Loading ──► Linking ──► Initialization
            │
            ├─► Verification
            ├─► Preparation
            └─► Resolution
```

---

## 💡 Practical Examples

### Example 1: Multiple Interface Implementation

```java
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

class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }
    
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}
```

### Example 2: Template Method Pattern

```java
abstract class DataProcessor {
    // Template method
    public final void process() {
        readData();
        validateData();
        processData();
        saveData();
    }
    
    protected abstract void readData();
    protected abstract void processData();
    
    protected void validateData() {
        System.out.println("Default validation");
    }
    
    protected void saveData() {
        System.out.println("Default save");
    }
}
```

### Example 3: Dynamic Class Loading

```java
public class DynamicLoader {
    public static void main(String[] args) {
        try {
            // Load class at runtime
            Class<?> clazz = Class.forName("com.example.MyClass");
            
            // Create instance
            Object obj = clazz.getDeclaredConstructor().newInstance();
            
            // Invoke method
            Method method = clazz.getMethod("execute");
            method.invoke(obj);
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

## 🎓 Interview Questions

### Beginner Level

1. What is the difference between a class and an interface?
2. Can a class extend multiple classes in Java?
3. What is a functional interface?
4. What is the purpose of the @Override annotation?
5. What is a class loader?

### Intermediate Level

1. Explain variable shadowing vs method overriding
2. When should you use an abstract class vs an interface?
3. What is the diamond problem and how does Java solve it?
4. Explain the parent delegation model in class loading
5. What's the difference between ClassNotFoundException and NoClassDefFoundError?

### Advanced Level

1. How do default methods in interfaces affect the diamond problem?
2. Explain the three phases of class loading in detail
3. How would you implement a custom class loader?
4. What are the benefits and drawbacks of composition vs inheritance?
5. How can you debug class loading issues in production?

---

## 🛠️ Hands-On Exercises

### Exercise 1: Interface Design
Create a payment processing system using interfaces:
- Define `PaymentProcessor` interface
- Implement `CreditCardProcessor`, `PayPalProcessor`, `BitcoinProcessor`
- Use Strategy pattern for flexible payment methods

### Exercise 2: Abstract Class Implementation
Implement a report generation system:
- Create abstract `ReportGenerator` class
- Use Template Method pattern
- Implement `PDFReportGenerator` and `ExcelReportGenerator`

### Exercise 3: Custom Class Loader
Build a plugin system:
- Create custom class loader for plugins
- Load plugin classes from a directory
- Implement plugin isolation

### Exercise 4: Diamond Problem Resolution
Create a scenario with diamond problem:
- Define interfaces with conflicting default methods
- Implement resolution strategies
- Test all three resolution rules

---

## 📊 Visual Learning Aids

### Class Structure Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                    Java Class                                │
├─────────────────────────────────────────────────────────────┤
│  • Instance Variables (State)                                │
│  • Static Variables (Class-level state)                      │
│  • Constants                                                 │
│  • Constructors                                              │
│  • Instance Methods (Behavior)                               │
│  • Static Methods                                            │
│  • Nested Classes                                            │
└─────────────────────────────────────────────────────────────┘
```

### Interface Evolution Timeline

```
Java 7          Java 8          Java 9
  │               │               │
  ├─► Abstract    ├─► Default     ├─► Private
  │   methods     │   methods     │   methods
  │               │               │
  └─► Constants   ├─► Static      └─► Private
                  │   methods         static
                  │
                  └─► Functional
                      interfaces
```

### Class Loader Delegation Flow

```
Request Class
     │
     ▼
Check if loaded ──► Yes ──► Return
     │
     No
     │
     ▼
Delegate to parent ──► Found ──► Return
     │
     Not found
     │
     ▼
Load by self ──► Found ──► Return
     │
     Not found
     │
     ▼
ClassNotFoundException
```

---

## 🔗 Related Modules

- **[Module 01: Java Overview](../Module%2001%20-%20Java%20Overview/)** - Java fundamentals
- **[Module 02: Data Types](../Module%2002%20-%20Java%20Data%20Types/)** - Type system basics
- **[Module 03: Modifiers & Annotations](../Module%2003%20-%20Modifiers%20Annotations%20Initializers/)** - Access control
- **[Module 05: Java Objects](../Module%2005%20-%20Java%20Objects/)** - Object lifecycle
- **[Module 06: OOP & FP](../Module%2006%20-%20OOP%20and%20FP/)** - Design principles
- **[Module 11: JVM](../Module%2011%20-%20JVM/)** - JVM internals
- **[Module 15: Design Patterns](../Module%2015%20-%20Design%20Patterns/)** - Common patterns

---

## 📝 Best Practices Summary

### Classes & Interfaces
- ✓ Program to interfaces, not implementations
- ✓ Keep interfaces focused (Interface Segregation Principle)
- ✓ Use @Override annotation consistently
- ✓ Prefer composition over inheritance
- ✓ Document design decisions

### Abstract Classes
- ✓ Use for "IS-A" relationships
- ✓ Provide partial implementations
- ✓ Use Template Method pattern when appropriate
- ✓ Make template methods final
- ✓ Provide hook methods for flexibility

### Interfaces
- ✓ Use for "CAN-DO" capabilities
- ✓ Design for multiple implementations
- ✓ Use default methods judiciously
- ✓ Keep functional interfaces simple
- ✓ Use @FunctionalInterface annotation

### Class Loading
- ✓ Understand classpath configuration
- ✓ Use appropriate loading method
- ✓ Handle exceptions properly
- ✓ Avoid class loader leaks
- ✓ Use -verbose:class for debugging
- ✓ Document custom class loaders

---

## 🎯 Module Completion Checklist

- [ ] Read all three topic files
- [ ] Understand class vs interface differences
- [ ] Practice multiple interface implementation
- [ ] Implement Template Method pattern
- [ ] Implement Strategy pattern
- [ ] Understand class loading phases
- [ ] Debug class loading issues
- [ ] Create a custom class loader
- [ ] Complete hands-on exercises
- [ ] Review interview questions
- [ ] Apply concepts in real projects

---

## 📚 Additional Resources

### Official Documentation
- [Java Language Specification - Classes](https://docs.oracle.com/javase/specs/jls/se17/html/jls-8.html)
- [Java Language Specification - Interfaces](https://docs.oracle.com/javase/specs/jls/se17/html/jls-9.html)
- [Java Virtual Machine Specification - Class Loading](https://docs.oracle.com/javase/specs/jvms/se17/html/jvms-5.html)

### Recommended Reading
- Effective Java by Joshua Bloch (Items on classes and interfaces)
- Design Patterns: Elements of Reusable Object-Oriented Software (GoF)
- Java Performance: The Definitive Guide (Class loading chapter)

### Tools
- `javap` - Java class file disassembler
- `jvisualvm` - Visual profiling tool
- Eclipse Memory Analyzer (MAT) - Memory analysis
- JConsole - JVM monitoring

---

## 💬 Discussion Topics

1. **When would you choose an abstract class over an interface in a real project?**
   - Consider state management, code reuse, and design flexibility

2. **How do Java 8+ default methods change interface design?**
   - Discuss backward compatibility and API evolution

3. **What are the performance implications of class loading?**
   - Analyze startup time, memory usage, and optimization strategies

4. **How would you design a plugin architecture using class loaders?**
   - Consider isolation, versioning, and hot deployment

5. **What are the trade-offs between composition and inheritance?**
   - Evaluate flexibility, coupling, and testability

---

## 🚀 Next Steps

After completing this module:

1. **Practice** - Implement the hands-on exercises
2. **Apply** - Use these concepts in your projects
3. **Review** - Revisit complex topics as needed
4. **Advance** - Move to Module 05: Java Objects
5. **Integrate** - Combine with OOP principles from Module 06

---

## 📊 Module Statistics

- **Topics**: 3 comprehensive guides
- **Code Examples**: 50+ practical examples
- **Diagrams**: 30+ visual aids
- **Interview Questions**: 15+ common questions
- **Exercises**: 4 hands-on projects
- **Estimated Time**: 8-10 hours

---

## ✨ Key Takeaways

```
┌─────────────────────────────────────────────────────────────┐
│              Module 04 Key Takeaways                         │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  1. Classes define objects with state and behavior           │
│  2. Interfaces define contracts without state                │
│  3. Java supports single class inheritance                   │
│  4. Java supports multiple interface inheritance             │
│  5. Abstract classes provide partial implementations         │
│  6. Interfaces enable flexible, loosely-coupled design       │
│  7. Class loading happens in three phases                    │
│  8. Parent delegation ensures uniqueness and security        │
│  9. Custom class loaders enable advanced scenarios           │
│  10. Proper design choices lead to maintainable code         │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

**Happy Learning! 🎓**

**[⬆ Back to Core Java Index](../README.md)**

---

**Last Updated**: 2026-06-03  
**Module Version**: 2.0  
**Contributors**: Java Architect Team
