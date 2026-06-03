# Java Data Types - Complete Guide

> **Module**: Java Data Types  
> **Difficulty**: Beginner to Intermediate  
> **Estimated Time**: 2-3 hours

---

## 📋 Table of Contents

1. [Introduction to Java Data Types](#introduction-to-java-data-types)
2. [Primitive Data Types](#primitive-data-types)
3. [Wrapper Classes](#wrapper-classes)
4. [Type Conversion & Casting](#type-conversion--casting)
5. [Floating-Point Considerations](#floating-point-considerations)
6. [Bitwise Operations](#bitwise-operations)
7. [Best Practices](#best-practices)
8. [Common Pitfalls](#common-pitfalls)

---

## Introduction to Java Data Types

Java is a **strongly typed language**, meaning every variable must have a declared type. This provides type safety and helps catch errors at compile-time.

### Type System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                  JAVA TYPE SYSTEM                           │
└─────────────────────────────────────────────────────────────┘

                    Java Types
                        │
        ┌───────────────┴───────────────┐
        │                               │
   Primitive Types              Reference Types
        │                               │
   ┌────┴────┐                    ┌─────┴─────┐
   │         │                    │           │
Numeric  Boolean              Classes    Interfaces
   │                              │
┌──┴──┐                      ┌────┴────┐
│     │                      │         │
Integer Floating          Arrays   Objects
Types   Point
```

### Why Data Types Matter

```
┌─────────────────────────────────────────────────────────────┐
│           IMPORTANCE OF CHOOSING RIGHT DATA TYPES           │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. ✅ Memory Efficiency                                    │
│     • Smaller types use less memory                         │
│     • Important for large datasets                          │
│                                                              │
│  2. ✅ Performance                                          │
│     • Primitive types are faster                            │
│     • Less overhead than objects                            │
│                                                              │
│  3. ✅ Precision                                            │
│     • Choose appropriate precision for calculations         │
│     • Avoid data loss                                       │
│                                                              │
│  4. ✅ Type Safety                                          │
│     • Compile-time error detection                          │
│     • Prevents runtime errors                               │
│                                                              │
│  5. ✅ Code Clarity                                         │
│     • Self-documenting code                                 │
│     • Clear intent                                          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Primitive Data Types

Java has **8 primitive data types** that represent simple values.

### Complete Overview

```
┌──────────────────────────────────────────────────────────────────────┐
│                    JAVA PRIMITIVE DATA TYPES                         │
├────────┬──────────┬─────────────────────┬──────────────────────────┤
│  Type  │   Size   │       Range         │      Default Value       │
├────────┼──────────┼─────────────────────┼──────────────────────────┤
│ byte   │  8 bits  │  -128 to 127        │          0               │
│ short  │ 16 bits  │  -32,768 to 32,767  │          0               │
│ int    │ 32 bits  │  -2³¹ to 2³¹-1      │          0               │
│ long   │ 64 bits  │  -2⁶³ to 2⁶³-1      │          0L              │
│ float  │ 32 bits  │  ±3.4E+38 (7 digits)│          0.0f            │
│ double │ 64 bits  │  ±1.7E+308(15 digit)│          0.0d            │
│ char   │ 16 bits  │  0 to 65,535        │          '\u0000'        │
│ boolean│  1 bit   │  true or false      │          false           │
└────────┴──────────┴─────────────────────┴──────────────────────────┘
```

### Memory Layout

```
┌─────────────────────────────────────────────────────────────┐
│              PRIMITIVE TYPES MEMORY LAYOUT                  │
└─────────────────────────────────────────────────────────────┘

byte (8 bits):
┌───┬───┬───┬───┬───┬───┬───┬───┐
│ 0 │ 1 │ 0 │ 1 │ 1 │ 0 │ 1 │ 0 │  = 90
└───┴───┴───┴───┴───┴───┴───┴───┘

short (16 bits):
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 1 │ 0 │ 1 │ 1 │ 0 │ 1 │ 0 │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
                                    = 90

int (32 bits):
┌────────────────┬────────────────┬────────────────┬────────────────┐
│   00000000     │   00000000     │   00000000     │   01011010     │
└────────────────┴────────────────┴────────────────┴────────────────┘
                                                    = 90

long (64 bits):
┌────────────────────────────────┬────────────────────────────────┐
│         32 bits (0s)           │         32 bits (90)           │
└────────────────────────────────┴────────────────────────────────┘
```

### Detailed Type Descriptions

#### 1. **Integer Types**

```java
// byte: -128 to 127 (8 bits)
byte age = 25;
byte temperature = -10;

// short: -32,768 to 32,767 (16 bits)
short year = 2024;
short altitude = -500;

// int: -2,147,483,648 to 2,147,483,647 (32 bits)
int population = 1000000;
int distance = -50000;

// long: -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 (64 bits)
long worldPopulation = 7800000000L;  // Note the 'L' suffix
long nationalDebt = 28000000000000L;
```

**When to Use Each:**

```
┌─────────────────────────────────────────────────────────────┐
│           INTEGER TYPE SELECTION GUIDE                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  byte:                                                      │
│  • Small numbers (-128 to 127)                              │
│  • Array indices for small arrays                           │
│  • Flags and status codes                                   │
│  • Memory-critical applications                             │
│                                                              │
│  short:                                                     │
│  • Medium-range numbers                                     │
│  • Year values                                              │
│  • Port numbers                                             │
│  • Rarely used in modern Java                               │
│                                                              │
│  int: (MOST COMMON)                                         │
│  • Default choice for integers                              │
│  • Loop counters                                            │
│  • Array sizes                                              │
│  • Most calculations                                        │
│                                                              │
│  long:                                                      │
│  • Very large numbers                                       │
│  • Timestamps (milliseconds since epoch)                    │
│  • File sizes                                               │
│  • Database IDs                                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 2. **Floating-Point Types**

```java
// float: 32-bit IEEE 754 floating point
float price = 19.99f;  // Note the 'f' suffix
float pi = 3.14159f;

// double: 64-bit IEEE 754 floating point (DEFAULT)
double precisePrice = 19.99;
double scientificValue = 1.23e-4;  // 0.000123
```

**Precision Comparison:**

```
┌─────────────────────────────────────────────────────────────┐
│         FLOATING-POINT PRECISION                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  float (32-bit):                                            │
│  • ~7 decimal digits of precision                           │
│  • Range: ±3.4 × 10³⁸                                       │
│  • Example: 3.1415927                                       │
│                                                              │
│  double (64-bit):                                           │
│  • ~15 decimal digits of precision                          │
│  • Range: ±1.7 × 10³⁰⁸                                      │
│  • Example: 3.141592653589793                               │
│                                                              │
│  Recommendation: Use double for most calculations           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 3. **Character Type**

```java
// char: 16-bit Unicode character
char letter = 'A';
char digit = '5';
char symbol = '$';
char unicode = '\u0041';  // 'A' in Unicode
char newline = '\n';
char tab = '\t';
```

**Character Encoding:**

```
┌─────────────────────────────────────────────────────────────┐
│              CHAR TYPE DETAILS                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Size: 16 bits (2 bytes)                                    │
│  Range: 0 to 65,535 (unsigned)                              │
│  Encoding: UTF-16                                           │
│                                                              │
│  Common Escape Sequences:                                   │
│  • \n  - Newline                                            │
│  • \t  - Tab                                                │
│  • \r  - Carriage return                                    │
│  • \\  - Backslash                                          │
│  • \'  - Single quote                                       │
│  • \"  - Double quote                                       │
│  • \uXXXX - Unicode character                               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 4. **Boolean Type**

```java
// boolean: true or false
boolean isActive = true;
boolean hasPermission = false;
boolean isValid = (age >= 18);
```

---

## Wrapper Classes

Every primitive type has a corresponding **wrapper class** that provides object-oriented features.

### Wrapper Class Hierarchy

```
┌─────────────────────────────────────────────────────────────┐
│              WRAPPER CLASS HIERARCHY                        │
└─────────────────────────────────────────────────────────────┘

                    Object
                      │
        ┌─────────────┼─────────────┐
        │             │             │
     Number       Character      Boolean
        │
   ┌────┼────┬────┬────┬────┐
   │    │    │    │    │    │
 Byte Short Int Long Float Double
```

### Primitive to Wrapper Mapping

```
┌──────────────────────────────────────────────────────────────┐
│         PRIMITIVE vs WRAPPER CLASSES                         │
├─────────────┬────────────────┬──────────────────────────────┤
│  Primitive  │  Wrapper Class │      Package                 │
├─────────────┼────────────────┼──────────────────────────────┤
│  byte       │  Byte          │  java.lang.Byte              │
│  short      │  Short         │  java.lang.Short             │
│  int        │  Integer       │  java.lang.Integer           │
│  long       │  Long          │  java.lang.Long              │
│  float      │  Float         │  java.lang.Float             │
│  double     │  Double        │  java.lang.Double            │
│  char       │  Character     │  java.lang.Character         │
│  boolean    │  Boolean       │  java.lang.Boolean           │
└─────────────┴────────────────┴──────────────────────────────┘
```

### Why Use Wrapper Classes?

```
┌─────────────────────────────────────────────────────────────┐
│           BENEFITS OF WRAPPER CLASSES                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. ✅ Null Values                                          │
│     Integer age = null;  // Possible                        │
│     int age = null;      // Compile error                   │
│                                                              │
│  2. ✅ Collections                                          │
│     List<Integer> numbers = new ArrayList<>();              │
│     // Collections require objects, not primitives          │
│                                                              │
│  3. ✅ Utility Methods                                      │
│     int value = Integer.parseInt("123");                    │
│     String hex = Integer.toHexString(255);                  │
│                                                              │
│  4. ✅ Immutability                                         │
│     • Thread-safe by design                                 │
│     • Cannot be modified after creation                     │
│                                                              │
│  5. ✅ Constants                                            │
│     Integer.MAX_VALUE, Integer.MIN_VALUE                    │
│     Double.NaN, Double.POSITIVE_INFINITY                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Creating Wrapper Objects

```java
// Method 1: Constructor (Deprecated in Java 9+)
Integer num1 = new Integer(10);  // Don't use

// Method 2: valueOf() - RECOMMENDED
Integer num2 = Integer.valueOf(10);  // Best practice

// Method 3: Autoboxing (Automatic conversion)
Integer num3 = 10;  // Compiler converts to Integer.valueOf(10)

// Comparison
Integer a = Integer.valueOf(100);  // Cached
Integer b = Integer.valueOf(100);  // Returns same object
System.out.println(a == b);  // true (same object)

Integer c = Integer.valueOf(200);  // Not cached
Integer d = Integer.valueOf(200);  // New object
System.out.println(c == d);  // false (different objects)
```

### Integer Caching

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
```

### Wrapper Class Utility Methods

```java
// Parsing Strings
int num = Integer.parseInt("123");
double price = Double.parseDouble("19.99");
boolean flag = Boolean.parseBoolean("true");

// Converting to String
String str1 = Integer.toString(123);
String str2 = String.valueOf(123);  // Preferred

// Comparing Values
Integer a = 100;
Integer b = 200;
int result = a.compareTo(b);  // Returns -1 (a < b)

// Min/Max Values
System.out.println(Integer.MAX_VALUE);  // 2147483647
System.out.println(Integer.MIN_VALUE);  // -2147483648
System.out.println(Double.MAX_VALUE);   // 1.7976931348623157E308

// Type Conversion
Integer num = 42;
long longValue = num.longValue();
double doubleValue = num.doubleValue();
byte byteValue = num.byteValue();
```

---

## Type Conversion & Casting

### Widening vs Narrowing Conversion

```
┌─────────────────────────────────────────────────────────────┐
│           TYPE CONVERSION HIERARCHY                         │
└─────────────────────────────────────────────────────────────┘

Widening (Implicit - Safe):
byte → short → int → long → float → double
  ↓      ↓      ↓     ↓       ↓       ↓
 8bit  16bit  32bit 64bit   32bit   64bit

Narrowing (Explicit - Risky):
double → float → long → int → short → byte
   ↓       ↓      ↓     ↓      ↓       ↓
 Loss of precision possible
```

### Widening Conversion (Automatic)

```java
// Widening - No data loss, automatic
byte b = 10;
short s = b;    // byte → short (automatic)
int i = s;      // short → int (automatic)
long l = i;     // int → long (automatic)
float f = l;    // long → float (automatic)
double d = f;   // float → double (automatic)

// Data Flow
byte(10) → short(10) → int(10) → long(10) → float(10.0) → double(10.0)
```

### Narrowing Conversion (Explicit Cast Required)

```java
// Narrowing - Potential data loss, requires cast
double d = 100.99;
float f = (float) d;   // double → float
long l = (long) f;     // float → long (loses decimal)
int i = (int) l;       // long → int
short s = (short) i;   // int → short
byte b = (byte) s;     // short → byte

// Data Flow with Loss
double(100.99) → float(100.99) → long(100) → int(100) → short(100) → byte(100)
                                      ↑
                                 Decimal lost
```

### Casting Dangers

```java
// Example 1: Overflow
int largeInt = 130;
byte smallByte = (byte) largeInt;
System.out.println(smallByte);  // -126 (overflow!)

// Why? byte range: -128 to 127
// 130 in binary: 10000010
// Interpreted as signed byte: -126

// Example 2: Precision Loss
double precise = 123.456789;
float lessPrec = (float) precise;
System.out.println(lessPrec);  // 123.45679 (precision lost)

// Example 3: Truncation
double decimal = 99.99;
int whole = (int) decimal;
System.out.println(whole);  // 99 (decimal part lost)
```

### Type Conversion Flow Diagram

```
┌─────────────────────────────────────────────────────────────┐
│           TYPE CONVERSION DECISION TREE                     │
└─────────────────────────────────────────────────────────────┘

                    Start
                      │
                      ▼
            ┌─────────────────────┐
            │ Source Type Smaller │
            │ than Target Type?   │
            └─────────────────────┘
                 │         │
            Yes  │         │  No
                 ▼         ▼
        ┌──────────────┐  ┌──────────────┐
        │  Widening    │  │  Narrowing   │
        │  Conversion  │  │  Conversion  │
        └──────────────┘  └──────────────┘
                 │                │
                 ▼                ▼
        ┌──────────────┐  ┌──────────────┐
        │  Automatic   │  │  Explicit    │
        │  No Cast     │  │  Cast Needed │
        │  Safe        │  │  Risky       │
        └──────────────┘  └──────────────┘
                 │                │
                 ▼                ▼
        ┌──────────────┐  ┌──────────────┐
        │  No Data     │  │  Possible    │
        │  Loss        │  │  Data Loss   │
        └──────────────┘  └──────────────┘
```

---

## Floating-Point Considerations

### The Floating-Point Problem

```java
// Problem: Precision Issues
double a = 0.1;
double b = 0.2;
double sum = a + b;
System.out.println(sum);  // 0.30000000000000004 (NOT 0.3!)

// Why? Binary representation cannot exactly represent 0.1
```

### Binary Representation Issue

```
┌─────────────────────────────────────────────────────────────┐
│         WHY FLOATING-POINT IS IMPRECISE                     │
└─────────────────────────────────────────────────────────────┘

Decimal 0.1 in Binary:
0.1₁₀ = 0.0001100110011001100110011... (repeating)
         ↑
    Cannot be exactly represented in finite bits!

Similar to how 1/3 in decimal:
0.333333... (repeating forever)

Result: Rounding errors accumulate in calculations
```

### Never Compare Floats with ==

```java
// ❌ WRONG: Never do this
double x = 0.1 + 0.2;
if (x == 0.3) {  // Will be false!
    System.out.println("Equal");
}

// ✅ CORRECT: Use epsilon comparison
double epsilon = 0.0001;
if (Math.abs(x - 0.3) < epsilon) {
    System.out.println("Equal enough");
}

// ✅ BETTER: Use BigDecimal for precision
import java.math.BigDecimal;

BigDecimal a = new BigDecimal("0.1");
BigDecimal b = new BigDecimal("0.2");
BigDecimal sum = a.add(b);
System.out.println(sum);  // Exactly 0.3
```

### Infinite Loop Danger

```java
// ❌ DANGER: Infinite loop with float
for (float f = 5.0f; f != 10.0f; f += 0.1f) {
    System.out.println(f);
    // May never reach exactly 10.0 due to rounding errors!
}

// ✅ SAFE: Use integer loop
for (int i = 50; i != 100; i++) {
    float f = i / 10.0f;
    System.out.println(f);
}
```

### Money Calculations

```java
// ❌ WRONG: Using double for money
double price = 0.1;
double quantity = 3;
double total = price * quantity;
System.out.println(total);  // 0.30000000000000004

// ✅ CORRECT: Use BigDecimal
import java.math.BigDecimal;

BigDecimal price = new BigDecimal("0.10");
BigDecimal quantity = new BigDecimal("3");
BigDecimal total = price.multiply(quantity);
System.out.println(total);  // Exactly 0.30

// ✅ ALTERNATIVE: Use cents (int/long)
int priceInCents = 10;  // $0.10
int quantity = 3;
int totalInCents = priceInCents * quantity;  // 30 cents
double totalInDollars = totalInCents / 100.0;  // $0.30
```

---

## Bitwise Operations

### Bitwise Operators

```
┌──────────────────────────────────────────────────────────────┐
│              BITWISE OPERATORS                               │
├──────────┬───────────────────────────────────────────────────┤
│ Operator │ Description                                       │
├──────────┼───────────────────────────────────────────────────┤
│    &     │ AND - Both bits must be 1                        │
│    |     │ OR - At least one bit must be 1                  │
│    ^     │ XOR - Bits must be different                     │
│    ~     │ NOT - Inverts all bits                           │
│   <<     │ Left shift - Multiply by 2ⁿ                      │
│   >>     │ Right shift - Divide by 2ⁿ (signed)             │
│   >>>    │ Unsigned right shift - Fill with zeros           │
└──────────┴───────────────────────────────────────────────────┘
```

### Bitwise Operations Examples

```java
// AND (&) - Both bits must be 1
int a = 12;  // 1100 in binary
int b = 10;  // 1010 in binary
int result = a & b;  // 1000 = 8

/*
    1100  (12)
  & 1010  (10)
  ------
    1000  (8)
*/

// OR (|) - At least one bit must be 1
result = a | b;  // 1110 = 14

/*
    1100  (12)
  | 1010  (10)
  ------
    1110  (14)
*/

// XOR (^) - Bits must be different
result = a ^ b;  // 0110 = 6

/*
    1100  (12)
  ^ 1010  (10)
  ------
    0110  (6)
*/

// NOT (~) - Inverts all bits
result = ~a;  // -13 (two's complement)

// Left Shift (<<) - Multiply by 2
int x = 5;  // 0101
result = x << 1;  // 1010 = 10 (5 * 2)
result = x << 2;  // 10100 = 20 (5 * 4)

// Right Shift (>>) - Divide by 2
result = x >> 1;  // 0010 = 2 (5 / 2)
```

### Practical Applications

```java
// 1. Checking if number is even/odd
boolean isEven = (num & 1) == 0;
boolean isOdd = (num & 1) == 1;

// 2. Multiplying/Dividing by powers of 2
int doubled = num << 1;   // num * 2
int quadrupled = num << 2;  // num * 4
int halved = num >> 1;    // num / 2

// 3. Swapping without temp variable
int a = 5, b = 10;
a = a ^ b;
b = a ^ b;  // b = (a ^ b) ^ b = a
a = a ^ b;  // a = (a ^ b) ^ a = b

// 4. Setting/Clearing/Toggling bits
int flags = 0;
flags |= (1 << 2);   // Set bit 2
flags &= ~(1 << 2);  // Clear bit 2
flags ^= (1 << 2);   // Toggle bit 2

// 5. Checking if power of 2
boolean isPowerOf2 = (num & (num - 1)) == 0 && num != 0;
```

---

## Best Practices

### 1. Choosing Data Types

```java
// ✅ GOOD: Use appropriate types
int age = 25;              // int for most integers
long timestamp = System.currentTimeMillis();  // long for timestamps
double price = 19.99;      // double for decimals
BigDecimal money = new BigDecimal("19.99");  // BigDecimal for money

// ❌ BAD: Using wrong types
byte age = 25;             // Unnecessary restriction
float price = 19.99f;      // Less precision than double
double money = 19.99;      // Precision issues for money
```

### 2. Wrapper vs Primitive

```java
// ✅ GOOD: Use primitives for performance-critical code
for (int i = 0; i < 1000000; i++) {
    int sum = i + i;  // Fast
}

// ✅ GOOD: Use wrappers when null is needed
Integer userId = getUserId();  // Can be null
if (userId != null) {
    // Process user
}

// ❌ BAD: Unnecessary autoboxing in loops
for (Integer i = 0; i < 1000000; i++) {  // Slow!
    Integer sum = i + i;  // Creates many objects
}
```

### 3. valueOf() vs Constructor

```java
// ✅ GOOD: Use valueOf() for caching
Integer num = Integer.valueOf(100);  // Cached

// ❌ BAD: Using constructor (deprecated)
Integer num = new Integer(100);  // Always creates new object
```

### 4. Comparing Wrapper Objects

```java
// ❌ WRONG: Using == for wrapper objects
Integer a = 200;
Integer b = 200;
if (a == b) {  // false! (different objects)
    System.out.println("Equal");
}

// ✅ CORRECT: Using equals()
if (a.equals(b)) {  // true
    System.out.println("Equal");
}

// ✅ ALTERNATIVE: Unbox to primitive
if (a.intValue() == b.intValue()) {  // true
    System.out.println("Equal");
}
```

---

## Common Pitfalls

### 1. Integer Overflow

```java
// Problem
int max = Integer.MAX_VALUE;
int overflow = max + 1;
System.out.println(overflow);  // -2147483648 (wraps around!)

// Solution
long result = (long) max + 1;  // Cast to long first
System.out.println(result);  // 2147483648
```

### 2. Floating-Point Comparison

```java
// Problem
double a = 0.1 + 0.2;
if (a == 0.3) {  // false!
    System.out.println("Equal");
}

// Solution
double epsilon = 0.0001;
if (Math.abs(a - 0.3) < epsilon) {  // true
    System.out.println("Equal");
}
```

### 3. Null Pointer with Autoboxing

```java
// Problem
Integer num = null;
int primitive = num;  // NullPointerException!

// Solution
Integer num = null;
int primitive = (num != null) ? num : 0;  // Safe
```

### 4. Loss of Precision

```java
// Problem
long bigNumber = 123456789012345L;
float f = bigNumber;  // Loss of precision
System.out.println((long) f);  // Not the same!

// Solution
// Use double or BigDecimal for large precise numbers
double d = bigNumber;  // Better precision
BigDecimal bd = BigDecimal.valueOf(bigNumber);  // Exact
```

---

## Summary

### Key Takeaways

```
┌─────────────────────────────────────────────────────────────┐
│              DATA TYPES BEST PRACTICES                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ✅ Use int for most integer calculations                   │
│  ✅ Use long for timestamps and large numbers               │
│  ✅ Use double for floating-point (not float)               │
│  ✅ Use BigDecimal for money calculations                   │
│  ✅ Use wrapper classes when null is needed                 │
│  ✅ Use valueOf() instead of constructors                   │
│  ✅ Never compare floats with ==                            │
│  ✅ Be aware of integer overflow                            │
│  ✅ Use equals() for wrapper object comparison              │
│  ✅ Understand widening vs narrowing conversions            │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Next Steps

1. ✅ Understand primitive types and their ranges
2. ✅ Master wrapper classes and their usage
3. ➡️ Learn [Primitives & Memory](primitives-memory.md)
4. ➡️ Study [Autoboxing & Unboxing](autoboxing-unboxing.md)
5. ➡️ Explore [String Class](string-class.md)

---

**[← Back to Module Index](README.md)** | **[Next: Primitives & Memory →](primitives-memory.md)**