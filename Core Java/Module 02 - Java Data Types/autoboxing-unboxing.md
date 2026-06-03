# Java Autoboxing & Unboxing - Complete Guide

> **Module**: Java Data Types  
> **Difficulty**: Intermediate  
> **Estimated Time**: 1.5 hours

---

## 📋 Table of Contents

1. [Introduction](#introduction)
2. [What is Autoboxing?](#what-is-autoboxing)
3. [What is Unboxing?](#what-is-unboxing)
4. [When Does It Happen?](#when-does-it-happen)
5. [Benefits of Autoboxing](#benefits-of-autoboxing)
6. [Performance Implications](#performance-implications)
7. [Common Pitfalls](#common-pitfalls)
8. [Best Practices](#best-practices)
9. [Debugging Autoboxing Issues](#debugging-autoboxing-issues)

---

## Introduction

**Autoboxing** and **unboxing** are automatic conversions between primitive types and their corresponding wrapper classes, introduced in **Java 5** (2004).

### The Problem Before Java 5

```java
// Before Java 5 - Manual boxing required
Integer num = new Integer(10);  // Manual boxing
int primitive = num.intValue(); // Manual unboxing

List<Integer> list = new ArrayList<>();
list.add(new Integer(5));  // Had to manually wrap
int value = list.get(0).intValue();  // Had to manually unwrap

// Tedious and error-prone!
```

### The Solution: Autoboxing/Unboxing

```java
// Java 5+ - Automatic conversion
Integer num = 10;  // Autoboxing (automatic)
int primitive = num;  // Unboxing (automatic)

List<Integer> list = new ArrayList<>();
list.add(5);  // Autoboxing
int value = list.get(0);  // Unboxing

// Clean and simple!
```

---

## What is Autoboxing?

**Autoboxing** is the automatic conversion of a primitive type to its corresponding wrapper class object.

### Conversion Flow

```
┌─────────────────────────────────────────────────────────────┐
│                  AUTOBOXING PROCESS                         │
└─────────────────────────────────────────────────────────────┘

Source Code:
    int value = 10;
    Integer obj = value;  // Autoboxing
         │
         ▼
Compiler Transformation:
    Integer obj = Integer.valueOf(10);
         │
         ▼
Runtime Execution:
    ┌─────────────────┐
    │  Integer Object │
    │   value = 10    │
    │  (on heap)      │
    └─────────────────┘
```

### Complete Autoboxing Table

```
┌──────────────────────────────────────────────────────────────┐
│            AUTOBOXING CONVERSIONS                            │
├─────────────┬────────────────────────────────────────────────┤
│  Primitive  │  Wrapper (Autoboxing Method)                   │
├─────────────┼────────────────────────────────────────────────┤
│  boolean    │  Boolean.valueOf(boolean)                      │
│  byte       │  Byte.valueOf(byte)                            │
│  char       │  Character.valueOf(char)                       │
│  short      │  Short.valueOf(short)                          │
│  int        │  Integer.valueOf(int)                          │
│  long       │  Long.valueOf(long)                            │
│  float      │  Float.valueOf(float)                          │
│  double     │  Double.valueOf(double)                        │
└─────────────┴────────────────────────────────────────────────┘
```

### Autoboxing Examples

```java
// Example 1: Simple assignment
Integer num = 100;
// Compiler: Integer num = Integer.valueOf(100);

// Example 2: Method parameters
public void printNumber(Integer num) {
    System.out.println(num);
}
printNumber(42);  // Autoboxing
// Compiler: printNumber(Integer.valueOf(42));

// Example 3: Collections
List<Integer> numbers = new ArrayList<>();
numbers.add(10);  // Autoboxing
// Compiler: numbers.add(Integer.valueOf(10));

// Example 4: Return values
public Integer getAge() {
    return 25;  // Autoboxing
    // Compiler: return Integer.valueOf(25);
}

// Example 5: Expressions
Integer sum = 10 + 20;  // Autoboxing
// Compiler: Integer sum = Integer.valueOf(10 + 20);
```

### Data Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│         AUTOBOXING DATA FLOW EXAMPLE                        │
└─────────────────────────────────────────────────────────────┘

Code: List<Integer> list = new ArrayList<>();
      list.add(5);

Step 1: Primitive value
   5 (int primitive)
   │
   ▼
Step 2: Compiler inserts valueOf()
   Integer.valueOf(5)
   │
   ▼
Step 3: Check cache (-128 to 127)
   ┌─────────────────────┐
   │  Is 5 in cache?     │
   │  Yes! Return cached │
   └─────────────────────┘
   │
   ▼
Step 4: Return cached Integer object
   ┌─────────────────┐
   │ Integer(5)      │
   │ (cached object) │
   └─────────────────┘
   │
   ▼
Step 5: Add to list
   list.add(Integer(5))
```

---

## What is Unboxing?

**Unboxing** is the automatic conversion of a wrapper object to its corresponding primitive type.

### Conversion Flow

```
┌─────────────────────────────────────────────────────────────┐
│                   UNBOXING PROCESS                          │
└─────────────────────────────────────────────────────────────┘

Source Code:
    Integer obj = 100;
    int value = obj;  // Unboxing
         │
         ▼
Compiler Transformation:
    int value = obj.intValue();
         │
         ▼
Runtime Execution:
    ┌──────────┐
    │ int = 100│
    │ (on stack)│
    └──────────┘
```

### Complete Unboxing Table

```
┌──────────────────────────────────────────────────────────────┐
│             UNBOXING CONVERSIONS                             │
├─────────────┬────────────────────────────────────────────────┤
│  Wrapper    │  Primitive (Unboxing Method)                   │
├─────────────┼────────────────────────────────────────────────┤
│  Boolean    │  booleanValue()                                │
│  Byte       │  byteValue()                                   │
│  Character  │  charValue()                                   │
│  Short      │  shortValue()                                  │
│  Integer    │  intValue()                                    │
│  Long       │  longValue()                                   │
│  Float      │  floatValue()                                  │
│  Double     │  doubleValue()                                 │
└─────────────┴────────────────────────────────────────────────┘
```

### Unboxing Examples

```java
// Example 1: Simple assignment
Integer obj = 100;
int num = obj;  // Unboxing
// Compiler: int num = obj.intValue();

// Example 2: Method parameters
public void calculate(int num) {
    System.out.println(num * 2);
}
Integer value = 50;
calculate(value);  // Unboxing
// Compiler: calculate(value.intValue());

// Example 3: Arithmetic operations
Integer a = 10;
Integer b = 20;
int sum = a + b;  // Unboxing both
// Compiler: int sum = a.intValue() + b.intValue();

// Example 4: Comparisons
Integer x = 100;
if (x > 50) {  // Unboxing
    // Compiler: if (x.intValue() > 50)
}

// Example 5: Array access
Integer[] array = {10, 20, 30};
int value = array[0];  // Unboxing
// Compiler: int value = array[0].intValue();
```

---

## When Does It Happen?

### Complete Scenarios

```
┌─────────────────────────────────────────────────────────────┐
│        AUTOBOXING/UNBOXING SCENARIOS                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  AUTOBOXING occurs when:                                    │
│  1. ✓ Assigning primitive to wrapper variable              │
│  2. ✓ Passing primitive to method expecting wrapper         │
│  3. ✓ Adding primitive to collection                        │
│  4. ✓ Returning primitive from method with wrapper return   │
│  5. ✓ Using primitive in wrapper context                    │
│                                                              │
│  UNBOXING occurs when:                                      │
│  1. ✓ Assigning wrapper to primitive variable              │
│  2. ✓ Passing wrapper to method expecting primitive         │
│  3. ✓ Using wrapper in arithmetic operations                │
│  4. ✓ Using wrapper in comparisons                          │
│  5. ✓ Returning wrapper from method with primitive return   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Mixed Operations Example

```java
// Complex example with both autoboxing and unboxing
Integer a = 10;        // Autoboxing: Integer.valueOf(10)
Integer b = 20;        // Autoboxing: Integer.valueOf(20)
int sum = a + b;       // Unboxing both: a.intValue() + b.intValue()
Integer result = sum;  // Autoboxing: Integer.valueOf(sum)

// What compiler actually sees:
Integer a = Integer.valueOf(10);
Integer b = Integer.valueOf(20);
int sum = a.intValue() + b.intValue();
Integer result = Integer.valueOf(sum);
```

### Complete Data Flow

```
┌─────────────────────────────────────────────────────────────┐
│         AUTOBOXING/UNBOXING DATA FLOW                       │
└─────────────────────────────────────────────────────────────┘

Example: Integer result = (Integer)10 + (Integer)20;

Step 1: Autoboxing primitives
   10 ──────────────────────> Integer.valueOf(10)
   20 ──────────────────────> Integer.valueOf(20)

Step 2: Unboxing for addition
   Integer(10) ──────────────> 10 (intValue())
   Integer(20) ──────────────> 20 (intValue())

Step 3: Primitive addition
   10 + 20 = 30

Step 4: Autoboxing result
   30 ──────────────────────> Integer.valueOf(30)

Final: Integer result = Integer(30)

Total operations: 3 autoboxing + 2 unboxing = 5 conversions!
```

---

## Benefits of Autoboxing

### Q2: What are the benefits of autoboxing?

**Answer:** Cleaner, more readable code with less boilerplate.

```
┌─────────────────────────────────────────────────────────────┐
│           BENEFITS OF AUTOBOXING                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. ✅ Less Code to Write                                   │
│     • No manual valueOf() calls                             │
│     • No manual xxxValue() calls                            │
│                                                              │
│  2. ✅ More Readable Code                                   │
│     • Cleaner syntax                                        │
│     • Focus on logic, not conversion                        │
│                                                              │
│  3. ✅ Easier Collection Usage                              │
│     • Direct primitive addition                             │
│     • Seamless retrieval                                    │
│                                                              │
│  4. ✅ Reduced Errors                                       │
│     • Compiler handles conversions                          │
│     • Less manual code = fewer bugs                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Code Comparison

```java
// ❌ WITHOUT AUTOBOXING (Java 1.4 and earlier)
List<Integer> list = new ArrayList<>();
list.add(Integer.valueOf(6));
list.add(Integer.valueOf(7));
list.add(Integer.valueOf(8));

int sum = 0;
for (int i = 0; i < list.size(); i++) {
    sum += list.get(i).intValue();
}

Map<Long, Double> map = new HashMap<>();
map.put(Long.valueOf(5L), Double.valueOf(12.50));
double value = map.get(Long.valueOf(5L)).doubleValue();

// ✅ WITH AUTOBOXING (Java 5+)
List<Integer> list = new ArrayList<>();
list.add(6);  // Clean!
list.add(7);
list.add(8);

int sum = 0;
for (int num : list) {
    sum += num;  // Clean!
}

Map<Long, Double> map = new HashMap<>();
map.put(5L, 12.50);  // Clean!
double value = map.get(5L);  // Clean!
```

---

## Performance Implications

### Q3: What are some of the pitfalls of autoboxing?

**Answer:** Performance overhead, unnecessary object creation, and potential NullPointerExceptions.

### 1. Unnecessary Object Creation

```java
// ❌ SLOW: Autoboxing in loop creates 1 million objects!
Integer sum = 0;
for (int i = 0; i < 1_000_000; i++) {
    sum += i;  // Unbox sum, add i, autobox result
}
// Each iteration: unbox + autobox = 2 million operations!
// Time: ~100ms

// ✅ FAST: Use primitives
int sum = 0;
for (int i = 0; i < 1_000_000; i++) {
    sum += i;  // Pure primitive operation
}
// No object creation
// Time: ~2ms

// Performance difference: 50x faster!
```

### Performance Visualization

```
┌─────────────────────────────────────────────────────────────┐
│        PERFORMANCE: PRIMITIVE vs AUTOBOXING                 │
└─────────────────────────────────────────────────────────────┘

Operation: Sum 1 million integers

Primitive (int):
Time:  ██ 2ms
Memory: ▪ 4 bytes
Objects: 0

Autoboxing (Integer):
Time:  ████████████████████████████████████████████████ 100ms
Memory: ████████████████ 16 MB (temporary objects)
Objects: 1,000,000 created and discarded

Ratio: 1 : 50 (time)
       1 : 4,000,000 (memory)
```

### Detecting Object Creation with jmap

```bash
# Run the inefficient code
java AutoBoxUnbox &

# Get process ID
jps
# Output: 8896 AutoBoxUnbox

# Monitor heap
jmap -histo:live 8896 > mem.txt
```

**Initial Output:**

```
num  #instances    #bytes  class name
───────────────────────────────────────
 7:      1318     21088  java.lang.Integer
```

**After some time:**

```
num  #instances    #bytes  class name
───────────────────────────────────────
 7:      1704     27264  java.lang.Integer
```

**Growing instances = Memory leak pattern!**

### 2. Garbage Collection Overhead

```
┌─────────────────────────────────────────────────────────────┐
│         GARBAGE COLLECTION IMPACT                           │
└─────────────────────────────────────────────────────────────┘

Inefficient Code (Integer sum):
┌────────────────────────────────────┐
│  Loop iterations: 1,000,000        │
│  Objects created: 1,000,000        │
│  GC collections: 15                │
│  GC pause time: 150ms              │
│  Total time: 250ms                 │
└────────────────────────────────────┘

Optimized Code (int sum):
┌────────────────────────────────────┐
│  Loop iterations: 1,000,000        │
│  Objects created: 0                │
│  GC collections: 0                 │
│  GC pause time: 0ms                │
│  Total time: 2ms                   │
└────────────────────────────────────┘

Result: 125x faster with primitives!
```

---

## Common Pitfalls

### Pitfall 1: NullPointerException

```java
// ❌ DANGER: Unboxing null throws NPE
Integer num = null;
int value = num;  // NullPointerException!
// Compiler: int value = num.intValue(); // NPE!

// ✅ SAFE: Check for null
Integer num = null;
int value = (num != null) ? num : 0;

// Real-world example
public int calculateTotal(Integer quantity, Integer price) {
    // ❌ DANGER: NPE if either is null
    return quantity * price;
    
    // ✅ SAFE: Null checks
    if (quantity == null || price == null) {
        return 0;
    }
    return quantity * price;
}
```

**NPE Flow Diagram:**

```
┌─────────────────────────────────────────────────────────────┐
│         NULL POINTER EXCEPTION FLOW                         │
└─────────────────────────────────────────────────────────────┘

Code: Integer num = null;
      int value = num;

Step 1: num is null
   num ──> null

Step 2: Compiler inserts intValue()
   int value = num.intValue();
                │
                ▼
Step 3: Call method on null
   null.intValue()
        │
        ▼
Step 4: NullPointerException!
   ┌─────────────────────────────┐
   │ java.lang.NullPointerException │
   │ at line X                   │
   └─────────────────────────────┘
```

### Pitfall 2: Conditional Operator Surprise

```java
// ❌ DANGER: Type mismatch causes NPE
Integer num = null;
int result = (num != null) ? num : 0;  // NPE!
// Why? Ternary returns Integer, unboxing null!

// Explanation:
// 1. Ternary evaluates to Integer type (not int)
// 2. Result: Integer temp = (num != null) ? num : Integer.valueOf(0);
// 3. Then: int result = temp.intValue(); // NPE if temp is null!

// ✅ SAFE: Ensure same type
Integer num = null;
int result = (num != null) ? num.intValue() : 0;

// ✅ BETTER: Use Integer
Integer num = null;
Integer result = (num != null) ? num : 0;
```

### Pitfall 3: Comparison Confusion

```java
// ❌ WRONG: Using == with wrappers
Integer a = 200;
Integer b = 200;
System.out.println(a == b);  // false! (different objects)

Integer c = 100;
Integer d = 100;
System.out.println(c == d);  // true! (cached)

// Why the difference? Integer caching!

// ✅ CORRECT: Use equals()
System.out.println(a.equals(b));  // true

// ✅ ALTERNATIVE: Unbox first
System.out.println(a.intValue() == b.intValue());  // true
```

**Integer Caching Explained:**

```
┌─────────────────────────────────────────────────────────────┐
│              INTEGER CACHING MECHANISM                      │
└─────────────────────────────────────────────────────────────┘

valueOf() Method Behavior:

For values -128 to 127:
┌──────────────────────────────────────┐
│     Integer Cache Pool               │
│  ┌────┬────┬────┬───┬────┬────┐    │
│  │-128│-127│... │ 0 │... │127 │    │
│  └────┴────┴────┴───┴────┴────┘    │
│         Reused Objects               │
└──────────────────────────────────────┘
        ↓
Integer.valueOf(50) → Returns cached object
Integer.valueOf(50) → Returns SAME cached object
Result: a == b is TRUE

For values outside -128 to 127:
┌──────────────────────────────────────┐
│     Heap Memory                      │
│  ┌────────┐  ┌────────┐            │
│  │ New    │  │ New    │            │
│  │ Object │  │ Object │            │
│  └────────┘  └────────┘            │
└──────────────────────────────────────┘
        ↓
Integer.valueOf(200) → Creates new object
Integer.valueOf(200) → Creates ANOTHER new object
Result: a == b is FALSE
```

### Pitfall 4: Overloading Confusion

```java
// Q: What will be the output?
public class AutoBoxUnbox {
    public static void main(String[] args) {
        Integer value = 0;
        new AutoBoxUnbox().eval(value);
    }
    
    void eval(long val) {
        System.out.println(1);
    }
    
    void eval(Long value) {
        System.out.println(2);
    }
}

// Answer: 1
// Why? No direct conversion from Integer to Long
// So Integer → long (widening) is used instead of Integer → Long
```

**Overloading Resolution:**

```
┌─────────────────────────────────────────────────────────────┐
│         METHOD OVERLOADING RESOLUTION                       │
└─────────────────────────────────────────────────────────────┘

Call: eval(Integer(0))

Option 1: eval(Long value)
   Integer → Long?
   ❌ No direct conversion

Option 2: eval(long val)
   Integer → int (unbox) → long (widen)
   ✅ Valid conversion

Result: Calls eval(long val)
Output: 1
```

### Pitfall 5: Collection Performance

```java
// ❌ SLOW: Autoboxing in collection operations
List<Integer> numbers = new ArrayList<>();
for (int i = 0; i < 1_000_000; i++) {
    numbers.add(i);  // 1 million autoboxing operations
}

int sum = 0;
for (Integer num : numbers) {
    sum += num;  // 1 million unboxing operations
}
// Total: 2 million boxing/unboxing operations!

// ✅ FASTER: Use primitive array when possible
int[] numbers = new int[1_000_000];
for (int i = 0; i < 1_000_000; i++) {
    numbers[i] = i;  // No boxing
}

int sum = 0;
for (int num : numbers) {
    sum += num;  // No unboxing
}
// Total: 0 boxing/unboxing operations!
```

---

## Best Practices

### 1. Prefer Primitives for Local Variables

```java
// ✅ GOOD: Use primitives for local variables
public void calculate() {
    int count = 0;
    double total = 0.0;
    boolean isValid = true;
}

// ❌ BAD: Unnecessary wrappers
public void calculate() {
    Integer count = 0;
    Double total = 0.0;
    Boolean isValid = true;
}
```

### 2. Use Wrappers Only When Needed

```java
// ✅ GOOD: Wrapper needed for null
public class User {
    private Integer age;  // Can be null (unknown)
    
    public boolean hasAge() {
        return age != null;
    }
}

// ✅ GOOD: Wrapper needed for collections
List<Integer> scores = new ArrayList<>();
Map<String, Double> prices = new HashMap<>();
```

### 3. Avoid Autoboxing in Loops

```java
// ❌ BAD: Autoboxing in loop
Integer sum = 0;
for (int i = 0; i < 1000; i++) {
    sum += i;  // Creates 1000 Integer objects!
}

// ✅ GOOD: Primitive in loop
int sum = 0;
for (int i = 0; i < 1000; i++) {
    sum += i;  // No object creation
}
Integer result = sum;  // Single autoboxing at end
```

### 4. Be Explicit with Null Checks

```java
// ❌ RISKY: No null check
public int multiply(Integer a, Integer b) {
    return a * b;  // NPE if either is null
}

// ✅ SAFE: Explicit null handling
public int multiply(Integer a, Integer b) {
    if (a == null || b == null) {
        throw new IllegalArgumentException("Arguments cannot be null");
    }
    return a * b;
}

// ✅ ALTERNATIVE: Use primitives
public int multiply(int a, int b) {
    return a * b;  // No null possible
}
```

### 5. Use equals() for Wrapper Comparison

```java
// ❌ WRONG: Using ==
Integer a = 200;
Integer b = 200;
if (a == b) {  // false!
    System.out.println("Equal");
}

// ✅ CORRECT: Using equals()
if (a.equals(b)) {  // true
    System.out.println("Equal");
}

// ✅ ALTERNATIVE: Unbox and compare
if (a.intValue() == b.intValue()) {  // true
    System.out.println("Equal");
}
```

---

## Debugging Autoboxing Issues

### Q4: How will you go about debugging autoboxing and unboxing errors?

**Answer:** Use IDE warnings and profiling tools.

### 1. Configure IDE Warnings

**Eclipse Configuration:**

```
Java → Compiler → Errors/Warnings
  → Potential programming problems
    → Boxing and unboxing conversions: Warning
```

**IntelliJ IDEA Configuration:**

```
Settings → Editor → Inspections
  → Java → Performance issues
    ☑ Auto-boxing
    ☑ Auto-unboxing
```

### 2. Use Static Analysis Tools

```java
// FindBugs/SpotBugs will warn about:

// Warning: Boxed value is unboxed and then immediately re-boxed
Integer value = 10;
Integer result = value + 5;  // Unbox, add, rebox

// Warning: Boxing of primitive in loop
for (int i = 0; i < 1000; i++) {
    Integer boxed = i;  // Boxing in loop!
}

// Warning: Unboxing of possibly null value
Integer num = getNullableValue();
int value = num;  // Potential NPE!
```

### 3. Enable Compiler Warnings

```bash
# Compile with warnings
javac -Xlint:boxing MyClass.java

# Output:
# MyClass.java:10: warning: [boxing] boxing conversion
#     Integer num = 5;
#                   ^
# MyClass.java:11: warning: [boxing] unboxing conversion
#     int value = num;
#                 ^
```

### 4. Use Profiling Tools

```bash
# Monitor object creation
jmap -histo:live <pid> | grep Integer

# Track allocations
java -XX:+PrintGCDetails -XX:+PrintGCTimeStamps MyApp

# Use VisualVM for detailed analysis
jvisualvm
```

---

## Real-World Example

### E-Commerce Order Processing

```java
public class OrderProcessor {
    
    // ❌ INEFFICIENT VERSION
    public Integer calculateTotalBad(List<Integer> prices) {
        Integer total = 0;  // Wrapper
        for (Integer price : prices) {
            total += price;  // Unbox total, unbox price, add, autobox result
        }
        return total;
        // For 1000 items: 3000 boxing/unboxing operations!
        // Time: ~30ms
    }
    
    // ✅ OPTIMIZED VERSION
    public int calculateTotalGood(List<Integer> prices) {
        int total = 0;  // Primitive
        for (Integer price : prices) {
            total += price;  // Only unbox price
        }
        return total;
        // For 1000 items: 1000 unboxing operations
        // Time: ~10ms (3x faster!)
    }
    
    // ✅ BEST VERSION (if possible)
    public int calculateTotalBest(int[] prices) {
        int total = 0;  // Primitive
        for (int price : prices) {
            total += price;  // No boxing/unboxing at all!
        }
        return total;
        // For 1000 items: 0 boxing/unboxing operations
        // Time: ~3ms (10x faster than first version!)
    }
}
```

### Performance Comparison

```
┌─────────────────────────────────────────────────────────────┐
│      ORDER PROCESSING PERFORMANCE (1000 items)              │
└─────────────────────────────────────────────────────────────┘

calculateTotalBad (Integer total):
Time: ████████████████████████████████ 30ms
Boxing ops: 3000
Memory: High GC pressure

calculateTotalGood (int total):
Time: ██████████ 10ms
Boxing ops: 1000
Memory: Medium GC pressure

calculateTotalBest (int[] array):
Time: ███ 3ms
Boxing ops: 0
Memory: No GC pressure

Recommendation: Use primitives whenever possible!
```

---

## Summary

### Key Takeaways

```
┌─────────────────────────────────────────────────────────────┐
│        AUTOBOXING/UNBOXING BEST PRACTICES                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ✅ Understand what autoboxing/unboxing is                  │
│  ✅ Know when it happens automatically                      │
│  ✅ Use primitives for performance-critical code            │
│  ✅ Use wrappers only when null is needed                   │
│  ✅ Avoid autoboxing in loops                               │
│  ✅ Always check for null before unboxing                   │
│  ✅ Use equals() for wrapper comparison                     │
│  ✅ Be aware of Integer caching (-128 to 127)               │
│  ✅ Enable IDE warnings for boxing/unboxing                 │
│  ✅ Profile to detect unnecessary object creation           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Quick Reference

| Scenario | Autoboxing | Unboxing | Performance |
|----------|------------|----------|-------------|
| Assignment | ✓ | ✓ | Fast |
| Collections | ✓ | ✓ | Medium |
| Loops | ✓ | ✓ | Slow (avoid!) |
| Arithmetic | - | ✓ | Fast |
| Comparisons | - | ✓ | Fast |

### Common Mistakes to Avoid

```
┌─────────────────────────────────────────────────────────────┐
│           COMMON AUTOBOXING MISTAKES                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ❌ Using == to compare wrapper objects                     │
│  ❌ Autoboxing in loops                                     │
│  ❌ Unboxing null values                                    │
│  ❌ Unnecessary wrapper usage                               │
│  ❌ Ignoring performance implications                       │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

**[← Back to Primitives & Memory](primitives-memory.md)** | **[Next: String Class →](string-class.md)**

**[↑ Back to Module Index](README.md)**