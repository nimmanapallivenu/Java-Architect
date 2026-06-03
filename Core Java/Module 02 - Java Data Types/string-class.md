# Java String Class - Complete Deep Dive

> **Module**: Java Data Types  
> **Difficulty**: Intermediate to Advanced  
> **Estimated Time**: 3 hours

---

## 📋 Table of Contents

1. [Introduction to String](#introduction-to-string)
2. [String Immutability](#string-immutability)
3. [String Pool & Interning](#string-pool--interning)
4. [String vs StringBuffer vs StringBuilder](#string-vs-stringbuffer-vs-stringbuilder)
5. [String Comparison](#string-comparison)
6. [String Manipulation](#string-manipulation)
7. [String Concatenation Performance](#string-concatenation-performance)
8. [Design Patterns](#design-patterns)
9. [Regular Expressions](#regular-expressions)
10. [Java 8 String Features](#java-8-string-features)
11. [Best Practices](#best-practices)

---

## Introduction to String

The `String` class is one of the most frequently used classes in Java. It represents a sequence of characters and is **immutable**.

### String Characteristics

```
┌─────────────────────────────────────────────────────────────┐
│              STRING CLASS CHARACTERISTICS                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ✓ Immutable - Cannot be changed after creation            │
│  ✓ Final - Cannot be subclassed                            │
│  ✓ Thread-safe - Due to immutability                       │
│  ✓ Cached - String literals are pooled                     │
│  ✓ Implements CharSequence, Serializable, Comparable       │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### String Creation Methods

```java
// Method 1: String literal (RECOMMENDED)
String s1 = "Hello";  // Stored in string pool

// Method 2: new keyword (NOT RECOMMENDED)
String s2 = new String("Hello");  // Creates new object on heap

// Method 3: Character array
char[] chars = {'H', 'e', 'l', 'l', 'o'};
String s3 = new String(chars);

// Method 4: StringBuilder/StringBuffer
StringBuilder sb = new StringBuilder("Hello");
String s4 = sb.toString();

// Method 5: valueOf() - static factory method
String s5 = String.valueOf(123);
String s6 = String.valueOf(true);
```

---

## String Immutability

### What Does Immutable Mean?

Once a String object is created, its value **cannot be changed**. Any operation that appears to modify a String actually creates a new String object.

```
┌─────────────────────────────────────────────────────────────┐
│              STRING IMMUTABILITY EXPLAINED                  │
└─────────────────────────────────────────────────────────────┘

Code: String s = "Hello";
      s = s + " World";

Step 1: Create "Hello"
┌──────────────────┐
│  String: "Hello" │  ← s points here
└──────────────────┘

Step 2: Concatenate (creates NEW string)
┌──────────────────┐
│  String: "Hello" │  (becomes unreachable)
└──────────────────┘

┌────────────────────────┐
│  String: "Hello World" │  ← s now points here
└────────────────────────┘

Original "Hello" is eligible for garbage collection
```

### Why Immutable?

```
┌─────────────────────────────────────────────────────────────┐
│           BENEFITS OF STRING IMMUTABILITY                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. ✅ Security                                             │
│     • Strings used in security (passwords, URLs)            │
│     • Cannot be modified after validation                   │
│                                                              │
│  2. ✅ Thread Safety                                        │
│     • Multiple threads can share strings safely             │
│     • No synchronization needed                             │
│                                                              │
│  3. ✅ Caching/Pooling                                      │
│     • String literals can be reused                         │
│     • Saves memory                                          │
│                                                              │
│  4. ✅ Hash Code Caching                                    │
│     • Hash code calculated once                             │
│     • Efficient for HashMap/HashSet keys                    │
│                                                              │
│  5. ✅ Class Loading                                        │
│     • Class names are strings                               │
│     • Immutability ensures integrity                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Immutability Example

```java
public class StringImmutability {
    public static void main(String[] args) {
        String s = "Hello";
        s.concat(" World");  // Creates new string, but not assigned
        System.out.println(s);  // Still prints "Hello"
        
        s = s.concat(" World");  // Now assigned to s
        System.out.println(s);  // Prints "Hello World"
    }
}
```

### Common Mistake: Expecting Mutation

```java
// ❌ WRONG: Expecting string to change
String s = " Hello ";
s.trim();  // Creates new trimmed string, but not assigned!
System.out.println(s);  // Still " Hello " with spaces

// ✅ CORRECT: Assign the result
String s = " Hello ";
s = s.trim();  // Assign the new trimmed string
System.out.println(s);  // "Hello" without spaces
```

**Visual Explanation:**

```
┌─────────────────────────────────────────────────────────────┐
│         STRING TRIM() OPERATION FLOW                        │
└─────────────────────────────────────────────────────────────┘

Initial State:
s ──> " Hello " (on heap)

After s.trim() (without assignment):
s ──> " Hello " (unchanged)
      
      "Hello" (new string created, but unreachable)
      ↓
      Garbage collected

After s = s.trim() (with assignment):
      " Hello " (becomes unreachable)
      
s ──> "Hello" (new trimmed string)
```

---

## String Pool & Interning

### Q1: What is the difference between "==" and "equals()" in comparing Java String objects?

**Answer:**
- `==` compares **object references** (memory addresses)
- `equals()` compares **actual content** (character sequences)

```java
public class StringEquals {
    public static void main(String[] args) {
        String s1 = "Hello";
        String s2 = new String("Hello");
        String s3 = "Hello";
        
        System.out.println(s1.equals(s2));  // true (same content)
        System.out.println(s1 == s2);       // false (different objects)
        System.out.println(s1 == s3);       // true (same object in pool)
    }
}
```

### String Pool Architecture

```
┌─────────────────────────────────────────────────────────────┐
│              STRING POOL ARCHITECTURE                       │
└─────────────────────────────────────────────────────────────┘

HEAP MEMORY:
┌───────────────────────────────────────────────────────────┐
│                                                            │
│  Regular Heap Objects:                                    │
│  ┌────────────────────┐                                  │
│  │ new String("Hello")│  ← s2 points here                │
│  └────────────────────┘                                  │
│                                                            │
│  ┌─────────────────────────────────────────────────────┐ │
│  │              STRING POOL                            │ │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐         │ │
│  │  │ "Hello"  │  │ "World"  │  │ "Java"   │         │ │
│  │  └──────────┘  └──────────┘  └──────────┘         │ │
│  │       ↑             ↑                               │ │
│  │       │             │                               │ │
│  │      s1, s3      (other refs)                       │ │
│  └─────────────────────────────────────────────────────┘ │
│                                                            │
└───────────────────────────────────────────────────────────┘

Key Points:
• String literals automatically go to pool
• new String() creates object on regular heap
• intern() can move strings to pool
```

### Q2: Can you explain how Strings are interned in Java?

**Answer:** String interning is the process of storing only one copy of each distinct string value in the string pool.

```
┌─────────────────────────────────────────────────────────────┐
│              STRING INTERNING PROCESS                       │
└─────────────────────────────────────────────────────────────┘

Method: String.intern()

Step 1: Check if string exists in pool
   ┌─────────────────────┐
   │ Is "Hello" in pool? │
   └─────────────────────┘
            │
      ┌─────┴─────┐
      │           │
     Yes         No
      │           │
      ▼           ▼
   Return      Add to pool
   existing    and return
   reference   new reference
```

**Example:**

```java
// Creating strings
String s1 = "A";  // Automatically interned
String s2 = new String("A");  // Not interned (on heap)

// Before interning
System.out.println(s1 == s2);  // false (different objects)

// After interning
s2 = s2.intern();  // Returns reference from pool
System.out.println(s1 == s2);  // true (same object)
```

### String Pool Location

```
┌─────────────────────────────────────────────────────────────┐
│         STRING POOL LOCATION BY JAVA VERSION                │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Java 6 and earlier:                                        │
│  • String pool in PermGen (fixed size)                      │
│  • Limited by PermGen size                                  │
│  • OutOfMemoryError: PermGen space                          │
│                                                              │
│  Java 7 and later:                                          │
│  • String pool moved to Heap                                │
│  • Dynamic sizing                                           │
│  • Better memory management                                 │
│  • Can be garbage collected                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Flyweight Design Pattern

String class implements the **Flyweight pattern** to conserve memory by reusing objects.

```java
// Example: Only one "Hello" object created
String s1 = "Hello";
String s2 = "Hello";
String s3 = "Hello";

// All three references point to the same object
System.out.println(s1 == s2);  // true
System.out.println(s2 == s3);  // true
System.out.println(s1 == s3);  // true
```

---

## String vs StringBuffer vs StringBuilder

### Q4: What is the main difference between String, StringBuffer, and StringBuilder?

```
┌──────────────────────────────────────────────────────────────┐
│      STRING vs STRINGBUFFER vs STRINGBUILDER                 │
├──────────────┬─────────────┬─────────────┬──────────────────┤
│  Feature     │   String    │StringBuffer │  StringBuilder   │
├──────────────┼─────────────┼─────────────┼──────────────────┤
│  Mutability  │  Immutable  │   Mutable   │    Mutable       │
│  Thread-Safe │     Yes     │     Yes     │      No          │
│  Performance │   Slowest   │   Medium    │    Fastest       │
│  Since       │   Java 1.0  │  Java 1.0   │    Java 1.5      │
│  Use Case    │  Constants  │Multi-thread │  Single-thread   │
└──────────────┴─────────────┴─────────────┴──────────────────┘
```

### Performance Comparison

```java
// Scenario: Concatenate 10,000 strings

// ❌ SLOWEST: String (creates 10,000 objects!)
String result = "";
for (int i = 0; i < 10000; i++) {
    result += i;  // Creates new String each time
}
// Time: ~5000ms

// ✅ MEDIUM: StringBuffer (synchronized)
StringBuffer sb = new StringBuffer();
for (int i = 0; i < 10000; i++) {
    sb.append(i);  // Modifies same object
}
String result = sb.toString();
// Time: ~15ms

// ✅ FASTEST: StringBuilder (not synchronized)
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 10000; i++) {
    sb.append(i);  // Modifies same object, no sync
}
String result = sb.toString();
// Time: ~5ms
```

**Visual Performance:**

```
┌─────────────────────────────────────────────────────────────┐
│      CONCATENATION PERFORMANCE (10,000 iterations)          │
└─────────────────────────────────────────────────────────────┘

String:
████████████████████████████████████████████████████ 5000ms

StringBuffer:
███ 15ms

StringBuilder:
█ 5ms

Ratio: 1000 : 3 : 1
```

### When to Use Each

```
┌─────────────────────────────────────────────────────────────┐
│              USAGE GUIDELINES                               │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Use String when:                                           │
│  • Value won't change                                       │
│  • Small number of concatenations                           │
│  • String literals                                          │
│  • HashMap/HashSet keys                                     │
│                                                              │
│  Use StringBuffer when:                                     │
│  • Multiple threads access same buffer                      │
│  • Thread safety required                                   │
│  • Legacy code (pre-Java 1.5)                               │
│                                                              │
│  Use StringBuilder when:                                    │
│  • Single-threaded environment                              │
│  • Many concatenations in loop                              │
│  • Performance critical                                     │
│  • Local variables (inherently thread-safe)                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Internal Structure

```
┌─────────────────────────────────────────────────────────────┐
│         STRINGBUILDER INTERNAL STRUCTURE                    │
└─────────────────────────────────────────────────────────────┘

StringBuilder sb = new StringBuilder("Hello");

Internal char array:
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ H │ e │ l │ l │ o │   │   │   │   │   │   │   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15
  
  count = 5 (current length)
  capacity = 16 (default initial capacity)

After sb.append(" World"):
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ H │ e │ l │ l │ o │   │ W │ o │ r │ l │ d │   │   │   │   │   │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
  
  count = 11
  capacity = 16 (no resize needed)

If capacity exceeded, array is resized:
new capacity = (old capacity * 2) + 2
```

---

## String Comparison

### Comparison Methods

```java
String s1 = "Hello";
String s2 = "hello";
String s3 = "Hello";

// 1. equals() - case-sensitive content comparison
s1.equals(s3);  // true
s1.equals(s2);  // false

// 2. equalsIgnoreCase() - case-insensitive
s1.equalsIgnoreCase(s2);  // true

// 3. compareTo() - lexicographic comparison
s1.compareTo(s3);  // 0 (equal)
s1.compareTo(s2);  // negative (s1 < s2 lexicographically)
"abc".compareTo("xyz");  // negative

// 4. compareToIgnoreCase()
s1.compareToIgnoreCase(s2);  // 0

// 5. == operator - reference comparison
s1 == s3;  // true (if both from pool)
s1 == new String("Hello");  // false (different objects)
```

### Comparison Flow

```
┌─────────────────────────────────────────────────────────────┐
│         STRING COMPARISON DECISION TREE                     │
└─────────────────────────────────────────────────────────────┘

                    Need to compare?
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
    References?       Content?          Order?
        │                 │                 │
        ▼                 ▼                 ▼
    Use ==          Use equals()      Use compareTo()
                         │
                    ┌────┴────┐
                    │         │
              Case matters? Case doesn't matter?
                    │         │
                    ▼         ▼
              equals()  equalsIgnoreCase()
```

---

## String Manipulation

### Q5: Can you write a method that reverses a given String?

**Answer:** Multiple approaches available.

### Approach 1: Using StringBuilder (RECOMMENDED)

```java
public static String reverse(String input) {
    if (input == null || input.length() == 0) {
        return input;
    }
    
    return new StringBuilder(input).reverse().toString();
}
```

**Why this is best:**
- Uses optimized built-in method
- Handles Unicode surrogate pairs correctly
- Fast and efficient (uses bit-wise operations)
- Thread-safe (local variable)

### Approach 2: Recursive Solution

```java
public String reverse(String str) {
    // Exit condition
    if ((null == str) || (str.length() <= 1)) {
        return str;
    }
    
    // Put first character at end, recurse with rest
    return reverse(str.substring(1)) + str.charAt(0);
}
```

**Recursion Flow:**

```
┌─────────────────────────────────────────────────────────────┐
│         RECURSIVE STRING REVERSAL                           │
└─────────────────────────────────────────────────────────────┘

Input: "RAW"

Step 1: reverse("RAW")
   ↓
Step 2: reverse("AW") + "R"
   ↓
Step 3: reverse("W") + "A" + "R"
   ↓
Step 4: "W" + "A" + "R"  (exit condition reached)
   ↓
Output: "WAR"

Call Stack:
┌──────────────────┐
│ reverse("W")     │ → returns "W"
├──────────────────┤
│ reverse("AW")    │ → returns "WA"
├──────────────────┤
│ reverse("RAW")   │ → returns "WAR"
└──────────────────┘
```

### Approach 3: Iterative Solution

```java
public String reverse(String str) {
    if ((null == str) || (str.length() <= 1)) {
        return str;
    }
    
    char[] chars = str.toCharArray();
    int rhsIdx = chars.length - 1;
    
    // Swap characters from both ends
    for (int lhsIdx = 0; lhsIdx < rhsIdx; lhsIdx++) {
        char temp = chars[lhsIdx];
        chars[lhsIdx] = chars[rhsIdx];
        chars[rhsIdx--] = temp;
    }
    
    return new String(chars);
}
```

**Iterative Flow:**

```
┌─────────────────────────────────────────────────────────────┐
│         ITERATIVE STRING REVERSAL                           │
└─────────────────────────────────────────────────────────────┘

Input: "HELLO"

Initial:
┌───┬───┬───┬───┬───┐
│ H │ E │ L │ L │ O │
└───┴───┴───┴───┴───┘
  ↑               ↑
  lhs            rhs

Iteration 1: Swap H ↔ O
┌───┬───┬───┬───┬───┐
│ O │ E │ L │ L │ H │
└───┴───┴───┴───┴───┘
      ↑       ↑
     lhs     rhs

Iteration 2: Swap E ↔ L
┌───┬───┬───┬───┬───┐
│ O │ L │ L │ E │ H │
└───┴───┴───┴───┴───┘
          ↑
      lhs=rhs (stop)

Output: "OLLEH"
```

---

## String Concatenation Performance

### Q10: What are the different ways to concatenate strings? Which approach is most efficient?

### Method 1: Plus (+) Operator

```java
String s1 = "John " + "Davies";
```

**When efficient:** Concatenating constants (compiler optimizes)

### Method 2: StringBuilder/StringBuffer

```java
StringBuilder sb = new StringBuilder("John ");
sb.append("Davies");
String result = sb.toString();
```

**When efficient:** Multiple concatenations, especially in loops

### Method 3: concat() Method

```java
String result = "John ".concat("Davies");
```

**When efficient:** Single concatenation of two strings

### Performance Comparison

```
┌─────────────────────────────────────────────────────────────┐
│      CONCATENATION PERFORMANCE COMPARISON                   │
└─────────────────────────────────────────────────────────────┘

Scenario 1: Concatenating Constants
String s = "John " + "Davies";
• Compiler optimizes to single string
• Performance: Excellent
• Use: Always for constants

Scenario 2: Concatenating Variables
String s = s2 + s3 + s4;
• Creates temporary String objects
• Performance: Good for few concatenations
• Use: 2-3 concatenations

Scenario 3: Loop Concatenation
for (int i = 0; i < 1000; i++) {
    s += i;  // ❌ Creates 1000 String objects!
}
• Performance: Very Poor
• Use: Never in loops!

StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append(i);  // ✅ Modifies same object
}
• Performance: Excellent
• Use: Always in loops
```

### Real-World Example

```java
// ❌ INEFFICIENT: String concatenation in loop
String result = "";
for (int i = 0; i < 1000; i++) {
    result += "Item:" + i;
}
// Creates ~2000 String objects
// Time: ~500ms

// ✅ EFFICIENT: StringBuilder
StringBuilder sb = new StringBuilder(250);
for (int i = 0; i < 1000; i++) {
    sb.append("Item:").append(i);
}
String result = sb.toString();
// Creates 1 String object
// Time: ~2ms

// Performance improvement: 250x faster!
```

---

## Design Patterns

### Q6-Q8: Design Patterns in String

### Flyweight Pattern

**Purpose:** Conserve memory by reusing objects.

```java
// Example 1: String literals (automatic flyweight)
String author = "Little brown fox";
String authorCopy = "Little brown fox";

if (author == authorCopy) {
    System.out.println("referencing the same object");  // Prints!
}
```

**How it works:**

```
┌─────────────────────────────────────────────────────────────┐
│              FLYWEIGHT PATTERN IN STRING                    │
└─────────────────────────────────────────────────────────────┘

String Pool (Flyweight Pool):
┌────────────────────────────────────────┐
│  "Little brown fox"                    │ ← Both references point here
│  "Hello"                               │
│  "World"                               │
└────────────────────────────────────────┘
     ↑         ↑
     │         │
  author   authorCopy

Memory saved: 1 object instead of 2
```

### Static Factory Method Pattern

**Purpose:** Encapsulate object creation logic.

```java
// Example: Integer.valueOf() uses flyweight + factory
Integer value1 = Integer.valueOf(5);
Integer value2 = Integer.valueOf(5);

if (value1 == value2) {
    System.out.println("referencing the same object");  // Prints!
}

// vs new Integer(5) - always creates new object
Integer value3 = new Integer(5);  // Deprecated
Integer value4 = new Integer(5);
System.out.println(value3 == value4);  // false
```

**Benefits of Static Factory Methods:**

```
┌─────────────────────────────────────────────────────────────┐
│        STATIC FACTORY METHOD BENEFITS                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. ✅ Meaningful Names                                     │
│     • getInstance(), valueOf(), of()                        │
│     • More descriptive than constructors                    │
│                                                              │
│  2. ✅ Object Caching                                       │
│     • Can return cached objects                             │
│     • Reduces object creation                               │
│                                                              │
│  3. ✅ Subtype Flexibility                                  │
│     • Can return subclass instances                         │
│     • Caller doesn't need to know concrete type             │
│                                                              │
│  4. ✅ Lazy Initialization                                  │
│     • Create objects only when needed                       │
│     • Better resource management                            │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## Regular Expressions

### Q9: How will you split the following string into individual vehicle types?

**Input:** `"Car,Jeep, Wagon Scooter Truck, Van"`

**Answer:** Use regular expressions with `split()`.

```java
public class StringSplit {
    public static void main(String[] args) {
        String pattern = "[,\\s]+";  // Comma or whitespace (1+ times)
        
        String vehicles = "Car,Jeep, Wagon Scooter Truck, Van";
        String[] result = vehicles.split(pattern);
        
        for (String vehicle : result) {
            System.out.println("Vehicle = \"" + vehicle + "\"");
        }
    }
}
```

**Output:**
```
Vehicle = "Car"
Vehicle = "Jeep"
Vehicle = "Wagon"
Vehicle = "Scooter"
Vehicle = "Truck"
Vehicle = "Van"
```

### Regular Expression Patterns

```
┌──────────────────────────────────────────────────────────────┐
│         COMMON REGEX PATTERNS                                │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  [,\\s]+     - Comma or whitespace (1 or more)              │
│  \\d+        - One or more digits                            │
│  \\w+        - One or more word characters                   │
│  [a-zA-Z]+   - One or more letters                           │
│  \\s*        - Zero or more whitespace                       │
│  ^           - Start of string                               │
│  $           - End of string                                 │
│  .           - Any character                                 │
│  *           - Zero or more                                  │
│  +           - One or more                                   │
│  ?           - Zero or one                                   │
│  {n}         - Exactly n times                               │
│  {n,}        - n or more times                               │
│  {n,m}       - Between n and m times                         │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

### More Examples

```java
// Email validation
String email = "user@example.com";
boolean isValid = email.matches("^[\\w.-]+@[\\w.-]+\\.[a-zA-Z]{2,}$");

// Phone number extraction
String text = "Call me at 123-456-7890";
Pattern pattern = Pattern.compile("\\d{3}-\\d{3}-\\d{4}");
Matcher matcher = pattern.matcher(text);
if (matcher.find()) {
    System.out.println("Phone: " + matcher.group());
}

// Replace all digits
String text = "Order 123 costs $45.67";
String result = text.replaceAll("\\d+", "X");
// Result: "Order X costs $X.X"
```

---

## Java 8 String Features

### Q12: How do you stream a String class in Java 8?

**Answer:** Use the `chars()` method.

```java
// Example 1: Print each character
"cactus".chars().forEach(c -> System.out.println((char)c));

// Output:
// c
// a
// c
// t
// u
// s
```

### String Streaming Operations

```java
// Count vowels
long vowelCount = "Hello World"
    .chars()
    .filter(c -> "aeiouAEIOU".indexOf(c) != -1)
    .count();
// Result: 3

// Convert to uppercase
String upper = "hello"
    .chars()
    .mapToObj(c -> String.valueOf((char)c).toUpperCase())
    .collect(Collectors.joining());
// Result: "HELLO"

// Find first non-repeated character
String input = "swiss";
Optional<Character> first = input.chars()
    .mapToObj(c -> (char)c)
    .filter(c -> input.indexOf(c) == input.lastIndexOf(c))
    .findFirst();
// Result: Optional[w]
```

### Parallel Processing

```java
// Q: Does parallel processing preserve order?
// A: No

// Sequential (preserves order)
"cactus".chars().forEach(c -> System.out.print((char)c));
// Output: cactus

// Parallel (may not preserve order)
"cactus".chars().parallel().forEach(c -> System.out.print((char)c));
// Output: ctacus (or any other order)
```

---

## Best Practices

### 1. Use String Literals

```java
// ✅ GOOD: Use literals (cached)
String s = "Hello";

// ❌ BAD: Use new (creates unnecessary object)
String s = new String("Hello");
```

### 2. Use StringBuilder in Loops

```java
// ❌ BAD: String concatenation in loop
String result = "";
for (int i = 0; i < 1000; i++) {
    result += i;
}

// ✅ GOOD: StringBuilder
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append(i);
}
String result = sb.toString();
```

### 3. Use equals() for Comparison

```java
// ❌ WRONG: Using ==
String s1 = new String("Hello");
String s2 = new String("Hello");
if (s1 == s2) {  // false
    System.out.println("Equal");
}

// ✅ CORRECT: Using equals()
if (s1.equals(s2)) {  // true
    System.out.println("Equal");
}
```

### 4. Handle Null Safely

```java
// ❌ RISKY: Potential NPE
String s = null;
if (s.equals("Hello")) {  // NullPointerException!
    // ...
}

// ✅ SAFE: Null check first
if (s != null && s.equals("Hello")) {
    // ...
}

// ✅ BETTER: Constant first
if ("Hello".equals(s)) {  // No NPE even if s is null
    // ...
}
```

### 5. Use Appropriate String Class

```java
// ✅ String: For immutable text
String constant = "Hello";

// ✅ StringBuilder: For mutable text (single-threaded)
StringBuilder sb = new StringBuilder();
sb.append("Hello").append(" World");

// ✅ StringBuffer: For mutable text (multi-threaded)
StringBuffer buffer = new StringBuffer();
buffer.append("Thread-safe");
```

---

## Summary

### Key Takeaways

```
┌─────────────────────────────────────────────────────────────┐
│              STRING CLASS BEST PRACTICES                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ✅ Strings are immutable - operations create new objects   │
│  ✅ Use string literals for automatic pooling               │
│  ✅ Use equals() for content comparison, not ==             │
│  ✅ Use StringBuilder for concatenation in loops            │
│  ✅ Understand string pool and interning                    │
│  ✅ Be aware of performance implications                    │
│  ✅ Use appropriate class (String/StringBuilder/Buffer)     │
│  ✅ Handle null safely (constant first in equals)           │
│  ✅ Leverage Java 8 stream operations                       │
│  ✅ Use regex for complex string operations                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Quick Reference

| Operation | Best Choice | Why |
|-----------|-------------|-----|
| Constants | String | Immutable, cached |
| Single concat | + operator | Compiler optimized |
| Loop concat | StringBuilder | Mutable, fast |
| Multi-thread | StringBuffer | Thread-safe |
| Comparison | equals() | Content comparison |
| Null-safe | "const".equals(var) | No NPE |

### Common Mistakes to Avoid

```
┌─────────────────────────────────────────────────────────────┐
│           COMMON STRING MISTAKES                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ❌ Using == instead of equals()                            │
│  ❌ String concatenation in loops                           │
│  ❌ Creating strings with new keyword                       │
│  ❌ Not assigning result of immutable operations            │
│  ❌ Ignoring null checks                                    │
│  ❌ Using StringBuffer when StringBuilder suffices          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

**[← Back to Autoboxing & Unboxing](autoboxing-unboxing.md)** | **[Next: Module 03 →](../Module%2003%20-%20Modifiers%20Annotations%20Initializers/README.md)**

**[↑ Back to Module Index](README.md)**