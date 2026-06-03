# Java Primitives & Memory Consumption

> **Module**: Java Data Types  
> **Difficulty**: Intermediate  
> **Estimated Time**: 2 hours

---

## 📋 Table of Contents

1. [Memory Architecture Overview](#memory-architecture-overview)
2. [Primitive Types Memory Layout](#primitive-types-memory-layout)
3. [Object Memory Overhead](#object-memory-overhead)
4. [Memory Comparison: Primitives vs Objects](#memory-comparison-primitives-vs-objects)
5. [Array Memory Consumption](#array-memory-consumption)
6. [Collection Memory Overhead](#collection-memory-overhead)
7. [Memory Optimization Strategies](#memory-optimization-strategies)
8. [Memory Profiling Tools](#memory-profiling-tools)
9. [Real-World Scenarios](#real-world-scenarios)

---

## Memory Architecture Overview

### JVM Memory Structure

```
┌─────────────────────────────────────────────────────────────┐
│                    JVM MEMORY LAYOUT                        │
└─────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                         HEAP MEMORY                           │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │              Young Generation                           │ │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐            │ │
│  │  │  Eden    │  │Survivor 0│  │Survivor 1│            │ │
│  │  │  Space   │  │   (S0)   │  │   (S1)   │            │ │
│  │  └──────────┘  └──────────┘  └──────────┘            │ │
│  └─────────────────────────────────────────────────────────┘ │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │              Old Generation (Tenured)                   │ │
│  │         Long-lived objects stored here                  │ │
│  └─────────────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                      STACK MEMORY                             │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │  Thread 1 Stack    │  Thread 2 Stack  │  Thread N Stack │ │
│  │  ┌──────────────┐  │  ┌──────────────┐│  ┌────────────┐│ │
│  │  │ Local Vars   │  │  │ Local Vars   ││  │Local Vars  ││ │
│  │  │ Method Calls │  │  │ Method Calls ││  │Method Calls││ │
│  │  │ Primitives   │  │  │ Primitives   ││  │Primitives  ││ │
│  │  └──────────────┘  │  └──────────────┘│  └────────────┘│ │
│  └─────────────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                    METASPACE (Java 8+)                        │
│              Class metadata, static variables                 │
└───────────────────────────────────────────────────────────────┘
```

### Where Data Lives

```
┌─────────────────────────────────────────────────────────────┐
│           STACK vs HEAP MEMORY ALLOCATION                   │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  STACK (Fast, Limited):                                     │
│  • Primitive local variables                                │
│  • Object references (not the objects themselves)           │
│  • Method call frames                                       │
│  • Thread-specific                                          │
│  • LIFO (Last In, First Out)                                │
│  • Automatically managed                                    │
│                                                              │
│  HEAP (Slower, Larger):                                     │
│  • All objects (including wrapper objects)                  │
│  • Instance variables                                       │
│  • Arrays                                                   │
│  • Shared across threads                                    │
│  • Garbage collected                                        │
│  • Dynamically allocated                                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Memory Allocation Example

```java
public class MemoryExample {
    private int instanceVar = 10;  // Heap (part of object)
    
    public void calculate() {
        int localVar = 5;          // Stack
        Integer wrapperObj = 20;   // Reference on stack, object on heap
        
        int[] array = new int[3];  // Reference on stack, array on heap
    }
}
```

**Memory Visualization:**

```
┌─────────────────────────────────────────────────────────────┐
│                    MEMORY ALLOCATION                        │
└─────────────────────────────────────────────────────────────┘

STACK (Thread-specific):
┌────────────────────────────────────┐
│  calculate() method frame          │
│  ┌──────────────────────────────┐ │
│  │ localVar = 5                 │ │  4 bytes
│  │ wrapperObj = 0x1234 ────────┼─┼──┐
│  │ array = 0x5678 ─────────────┼─┼──┼─┐
│  └──────────────────────────────┘ │  │ │
└────────────────────────────────────┘  │ │
                                        │ │
HEAP (Shared):                          │ │
┌────────────────────────────────────┐  │ │
│  MemoryExample object              │  │ │
│  ┌──────────────────────────────┐ │  │ │
│  │ instanceVar = 10             │ │  │ │
│  └──────────────────────────────┘ │  │ │
│                                    │  │ │
│  Integer object (0x1234) ◄─────────┼──┘ │
│  ┌──────────────────────────────┐ │    │
│  │ value = 20                   │ │    │
│  └──────────────────────────────┘ │    │
│                                    │    │
│  int[] array (0x5678) ◄────────────┼────┘
│  ┌──────────────────────────────┐ │
│  │ [0] = 0, [1] = 0, [2] = 0    │ │
│  └──────────────────────────────┘ │
└────────────────────────────────────┘
```

---

## Primitive Types Memory Layout

### Memory Size Chart

```
┌──────────────────────────────────────────────────────────────┐
│         PRIMITIVE TYPES MEMORY CONSUMPTION                   │
├─────────────┬──────────┬────────────────────────────────────┤
│    Type     │   Size   │         Visual Representation      │
├─────────────┼──────────┼────────────────────────────────────┤
│   boolean   │  1 bit*  │  ▪                                 │
│   byte      │  8 bits  │  ▪▪▪▪▪▪▪▪                         │
│   char      │ 16 bits  │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪               │
│   short     │ 16 bits  │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪               │
│   int       │ 32 bits  │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪ │
│   float     │ 32 bits  │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪ │
│   long      │ 64 bits  │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪ │
│             │          │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪ │
│   double    │ 64 bits  │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪ │
│             │          │  ▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪▪ │
└─────────────┴──────────┴────────────────────────────────────┘
* boolean typically uses 1 byte (8 bits) in practice
```

### Q1: How much memory space does a primitive type int occupy?

**Answer:** 4 bytes (32 bits)

```
┌─────────────────────────────────────────────────────────────┐
│              INT MEMORY LAYOUT (32 bits)                    │
└─────────────────────────────────────────────────────────────┘

int value = 42;

Binary representation:
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 0 │ 1 │ 0 │ 1 │ 0 │ 1 │ 0 │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
 Byte 0      Byte 1      Byte 2      Byte 3
                                      = 42

Total: 4 bytes = 32 bits
```

### Q2: Where do primitive variables get stored?

**Answer:** It depends on the context.

```
┌─────────────────────────────────────────────────────────────┐
│         PRIMITIVE VARIABLE STORAGE LOCATIONS                │
└─────────────────────────────────────────────────────────────┘

Case 1: Local Variables → STACK
┌────────────────────────────────────┐
│  public void method() {            │
│      int number = 1;  // STACK     │
│  }                                 │
└────────────────────────────────────┘

Case 2: Instance Variables → HEAP
┌────────────────────────────────────┐
│  class MyClass {                   │
│      int number;  // HEAP          │
│  }                                 │
└────────────────────────────────────┘

Case 3: Static Variables → METASPACE
┌────────────────────────────────────┐
│  class MyClass {                   │
│      static int number;  // META   │
│  }                                 │
└────────────────────────────────────┘
```

**Example:**

```java
public class Primitive {
    public static void main(String[] args) {
        int number = 1;  // This is on the stack
    }
}

class MyWrapper {
    int number;  // This will be on the heap
    
    public MyWrapper(int number) {
        this.number = number;
    }
}
```

---

## Object Memory Overhead

### Object Header Structure

Every object in Java has memory overhead beyond its fields:

```
┌─────────────────────────────────────────────────────────────┐
│              OBJECT MEMORY STRUCTURE                        │
└─────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────┐
│                    Object Header                          │
│  ┌─────────────────────────────────────────────────────┐ │
│  │  Mark Word (8 bytes on 64-bit JVM)                  │ │
│  │  • Hash code                                        │ │
│  │  • GC generation age                                │ │
│  │  • Lock information                                 │ │
│  │  • Biased locking info                              │ │
│  └─────────────────────────────────────────────────────┘ │
│  ┌─────────────────────────────────────────────────────┐ │
│  │  Class Pointer (4 bytes with compressed oops)       │ │
│  │  • Points to class metadata                         │ │
│  │  • Compressed on 64-bit JVM                         │ │
│  └─────────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────────┘
┌───────────────────────────────────────────────────────────┐
│                    Instance Data                          │
│  • Actual field values                                    │
│  • Aligned to 8-byte boundaries                           │
└───────────────────────────────────────────────────────────┘
┌───────────────────────────────────────────────────────────┐
│                    Padding                                │
│  • Ensures 8-byte alignment                               │
└───────────────────────────────────────────────────────────┘

Total Object Overhead: 12-16 bytes (before any fields!)
```

### Q3: How much space does java.lang.Integer object occupy?

**Answer:** 16 bytes on 32-bit JVM, 24 bytes on 64-bit JVM (with compressed oops)

#### 32-bit JVM Breakdown

```
┌─────────────────────────────────────────────────────────────┐
│           INTEGER OBJECT ON 32-BIT JVM                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Object Header:                                             │
│  ┌────────────────────────────────────┐                    │
│  │  Class information   │  4 bytes    │                    │
│  ├────────────────────────────────────┤                    │
│  │  Flags (array, hash) │  4 bytes    │                    │
│  ├────────────────────────────────────┤                    │
│  │  Lock information    │  4 bytes    │                    │
│  └────────────────────────────────────┘                    │
│                                                              │
│  Instance Data:                                             │
│  ┌────────────────────────────────────┐                    │
│  │  int value           │  4 bytes    │                    │
│  └────────────────────────────────────┘                    │
│                                                              │
│  Total: 16 bytes                                            │
│                                                              │
│  Compare to primitive int: 4 bytes                          │
│  Overhead: 4x more memory!                                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 64-bit JVM Breakdown

```
┌─────────────────────────────────────────────────────────────┐
│           INTEGER OBJECT ON 64-BIT JVM                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Object Header:                                             │
│  ┌────────────────────────────────────┐                    │
│  │  Mark Word           │  8 bytes    │                    │
│  ├────────────────────────────────────┤                    │
│  │  Class Pointer       │  4 bytes    │  (compressed)      │
│  └────────────────────────────────────┘                    │
│                                                              │
│  Instance Data:                                             │
│  ┌────────────────────────────────────┐                    │
│  │  int value           │  4 bytes    │                    │
│  └────────────────────────────────────┘                    │
│                                                              │
│  Padding:                                                   │
│  ┌────────────────────────────────────┐                    │
│  │  Alignment padding   │  4 bytes    │                    │
│  └────────────────────────────────────┘                    │
│                                                              │
│  Total: 24 bytes (with compressed oops)                     │
│  Total: 28 bytes (without compression)                      │
│                                                              │
│  Compare to primitive int: 4 bytes                          │
│  Overhead: 6x more memory!                                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Key Insight:** Porting from 32-bit to 64-bit JVM increases memory usage!

---

## Memory Comparison: Primitives vs Objects

### Single Value Comparison

```
┌──────────────────────────────────────────────────────────────┐
│      MEMORY USAGE: PRIMITIVE vs WRAPPER                      │
├──────────────┬─────────────────┬──────────────────────────────┤
│    Type      │   Primitive     │      Wrapper Object          │
├──────────────┼─────────────────┼──────────────────────────────┤
│   boolean    │    1 byte       │   16 bytes (Boolean)         │
│   byte       │    1 byte       │   16 bytes (Byte)            │
│   char       │    2 bytes      │   16 bytes (Character)       │
│   short      │    2 bytes      │   16 bytes (Short)           │
│   int        │    4 bytes      │   16 bytes (Integer)         │
│   float      │    4 bytes      │   16 bytes (Float)           │
│   long       │    8 bytes      │   24 bytes (Long)            │
│   double     │    8 bytes      │   24 bytes (Double)          │
└──────────────┴─────────────────┴──────────────────────────────┘

Key Insight: Wrapper objects use 4-16x more memory!
```

### Collection Memory Impact

```java
// Scenario: Storing 1 million integers

// Option 1: int array
int[] primitiveArray = new int[1_000_000];
// Memory: 4 MB + array overhead (~24 bytes)
// Total: ~4 MB

// Option 2: Integer array
Integer[] wrapperArray = new Integer[1_000_000];
// Memory: 16 MB (objects) + 4 MB (references) + array overhead
// Total: ~20 MB

// Option 3: ArrayList<Integer>
List<Integer> list = new ArrayList<>(1_000_000);
// Memory: 16 MB (objects) + 4 MB (references) + ArrayList overhead
// Total: ~20+ MB

// Memory Difference: 5x more with wrappers!
```

**Visual Comparison:**

```
┌─────────────────────────────────────────────────────────────┐
│        MEMORY USAGE FOR 1 MILLION INTEGERS                  │
└─────────────────────────────────────────────────────────────┘

int[] array:
████ 4 MB

Integer[] array:
████████████████████ 20 MB

ArrayList<Integer>:
████████████████████▓ 20+ MB

Ratio: 1 : 5 : 5+
```

---

## Array Memory Consumption

### Q4: How much space does java.lang.Integer[] array with 1 element occupy on a 32-bit JVM?

**Answer:** 20 bytes

### Array Object Structure

```
┌─────────────────────────────────────────────────────────────┐
│              ARRAY MEMORY LAYOUT                            │
└─────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────┐
│                  Array Object Header                      │
│  ┌─────────────────────────────────────────────────────┐ │
│  │  Mark Word (8 bytes on 64-bit)                      │ │
│  └─────────────────────────────────────────────────────┘ │
│  ┌─────────────────────────────────────────────────────┐ │
│  │  Class Pointer (4 bytes compressed)                 │ │
│  └─────────────────────────────────────────────────────┘ │
│  ┌─────────────────────────────────────────────────────┐ │
│  │  Array Length (4 bytes)                             │ │
│  └─────────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────────┘
┌───────────────────────────────────────────────────────────┐
│                  Array Elements                           │
│  • Primitive values OR object references                  │
│  • Contiguous memory                                      │
└───────────────────────────────────────────────────────────┘

Array Overhead: 16 bytes + (element_size × length)
```

**32-bit JVM Breakdown:**

```
┌─────────────────────────────────────────────────────────────┐
│         INTEGER[] ARRAY WITH 1 ELEMENT (32-bit JVM)         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Array Header:                                              │
│  ┌────────────────────────────────────┐                    │
│  │  Class information   │  4 bytes    │                    │
│  ├────────────────────────────────────┤                    │
│  │  Flags               │  4 bytes    │                    │
│  ├────────────────────────────────────┤                    │
│  │  Lock information    │  4 bytes    │                    │
│  ├────────────────────────────────────┤                    │
│  │  Array size          │  4 bytes    │                    │
│  └────────────────────────────────────┘                    │
│                                                              │
│  Array Data:                                                │
│  ┌────────────────────────────────────┐                    │
│  │  Reference to Integer│  4 bytes    │                    │
│  └────────────────────────────────────┘                    │
│                                                              │
│  Total: 20 bytes (just for the array)                       │
│  Plus: 16 bytes for each Integer object                     │
│                                                              │
│  Grand Total: 36 bytes for array with 1 Integer             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Memory Calculation Examples

```java
// Example 1: int array
int[] numbers = new int[100];
// Memory = 16 (header) + (4 × 100) = 416 bytes

// Example 2: Integer array
Integer[] numbers = new Integer[100];
// Memory = 16 (header) + (4 × 100) [references] + (16 × 100) [objects]
//        = 16 + 400 + 1600 = 2016 bytes

// Example 3: 2D int array
int[][] matrix = new int[10][10];
// Memory = 16 (outer array) + (4 × 10) [references to inner arrays]
//        + 10 × [16 (inner array header) + (4 × 10) [elements]]
//        = 16 + 40 + 10 × (16 + 40)
//        = 16 + 40 + 560 = 616 bytes
```

---

## Collection Memory Overhead

### Q5: Can you arrange the following Collection data types in terms of their memory overheads in ascending order?

1. ArrayList
2. LinkedList
3. HashSet
4. HashMap

**Answer:** ArrayList → LinkedList → HashMap → HashSet

```
┌─────────────────────────────────────────────────────────────┐
│         COLLECTION MEMORY OVERHEAD COMPARISON               │
└─────────────────────────────────────────────────────────────┘

1. ArrayList (Lowest Overhead)
┌────────────────────────────────────┐
│  Array-backed structure            │
│  Default capacity: 10 elements     │
│  Overhead: Array header + refs     │
│  Memory: ~40 bytes + data          │
└────────────────────────────────────┘

2. LinkedList
┌────────────────────────────────────┐
│  Node-based structure              │
│  Each node: data + next + prev     │
│  Overhead: 3 references per node   │
│  Memory: ~24 bytes per node        │
└────────────────────────────────────┘

3. HashMap
┌────────────────────────────────────┐
│  Array + Entry objects             │
│  Default capacity: 16 buckets      │
│  Each entry: key + value + next    │
│  Memory: ~32 bytes per entry       │
└────────────────────────────────────┘

4. HashSet (Highest Overhead)
┌────────────────────────────────────┐
│  Wraps HashMap internally          │
│  Uses HashMap + dummy value        │
│  Overhead: HashMap + wrapper       │
│  Memory: HashMap memory + extra    │
└────────────────────────────────────┘
```

### Detailed Breakdown

```
┌──────────────────────────────────────────────────────────────┐
│         COLLECTION MEMORY DETAILS (10 elements)              │
├──────────────┬───────────────────────────────────────────────┤
│  Collection  │  Memory Usage                                 │
├──────────────┼───────────────────────────────────────────────┤
│  ArrayList   │  ~200 bytes                                   │
│              │  • Array: 16 + (4 × 10) = 56 bytes           │
│              │  • Objects: 16 × 10 = 160 bytes              │
│              │  • ArrayList overhead: ~40 bytes              │
├──────────────┼───────────────────────────────────────────────┤
│  LinkedList  │  ~400 bytes                                   │
│              │  • Nodes: 24 × 10 = 240 bytes                │
│              │  • Objects: 16 × 10 = 160 bytes              │
│              │  • LinkedList overhead: ~48 bytes             │
├──────────────┼───────────────────────────────────────────────┤
│  HashMap     │  ~600 bytes                                   │
│              │  • Buckets array: 16 + (4 × 16) = 80 bytes   │
│              │  • Entries: 32 × 10 = 320 bytes              │
│              │  • Keys/Values: 16 × 20 = 320 bytes          │
├──────────────┼───────────────────────────────────────────────┤
│  HashSet     │  ~650 bytes                                   │
│              │  • HashMap: ~600 bytes                        │
│              │  • HashSet wrapper: ~50 bytes                 │
└──────────────┴───────────────────────────────────────────────┘
```

**Trade-off:** Memory vs Functionality

- HashMap: O(1) lookup (worth the memory)
- ArrayList: O(1) random access
- LinkedList: O(1) insert/delete at ends
- HashSet: O(1) contains check

---

## Memory Optimization Strategies

### Q7: What are the best practices with regard to conserving memory when using Java Collections?

### 1. Set Initial Capacity Appropriately

```java
// ❌ BAD: Default capacity causes multiple resizes
Map<String, Integer> map = new HashMap<>();
// Default: 16 → 32 → 64 → 128 → 256 (for 130 elements)
// Wasted: 256 - 130 = 126 slots

// ✅ GOOD: Set appropriate initial capacity
Map<String, Integer> map = new HashMap<>(150);
// Allocates 150 slots directly, no resizing needed
```

**Growth Pattern:**

```
┌─────────────────────────────────────────────────────────────┐
│         HASHMAP GROWTH WITHOUT INITIAL CAPACITY             │
└─────────────────────────────────────────────────────────────┘

Adding 130 elements:

Step 1: Default capacity = 16
████████████████

Step 2: Resize to 32 (when 12 elements added)
████████████████████████████████

Step 3: Resize to 64 (when 24 elements added)
████████████████████████████████████████████████████████████████

Step 4: Resize to 128 (when 48 elements added)
████████████████████████████████████████████████████████████████
████████████████████████████████████████████████████████████████

Step 5: Resize to 256 (when 96 elements added)
████████████████████████████████████████████████████████████████
████████████████████████████████████████████████████████████████
████████████████████████████████████████████████████████████████
████████████████████████████████████████████████████████████████

Final: 256 slots for 130 elements (126 wasted!)
```

### 2. Lazy Initialization

```java
// ❌ BAD: Always allocating memory
public class Report {
    private List<String> errors = new ArrayList<>();  // Always allocated
    private Map<String, Object> metadata = new HashMap<>();  // Always allocated
}

// ✅ GOOD: Create only when needed
public class Report {
    private List<String> errors;     // null until needed
    private Map<String, Object> metadata;  // null until needed
    
    public void addError(String error) {
        if (errors == null) {
            errors = new ArrayList<>();
        }
        errors.add(error);
    }
    
    public void addMetadata(String key, Object value) {
        if (metadata == null) {
            metadata = new HashMap<>();
        }
        metadata.put(key, value);
    }
}
```

### 3. Use Primitives When Possible

```java
// ❌ BAD: Unnecessary wrapper usage
public class UserStats {
    private Integer loginCount;      // 16 bytes
    private Double averageTime;      // 24 bytes
    private Boolean isActive;        // 16 bytes
    // Total: 56 bytes + overhead
}

// ✅ GOOD: Use primitives
public class UserStats {
    private int loginCount;          // 4 bytes
    private double averageTime;      // 8 bytes
    private boolean isActive;        // 1 byte
    // Total: 13 bytes + overhead
    
    // Savings: 43 bytes per object!
    // For 1 million users: 43 MB saved!
}
```

### 4. Choose Appropriate Types

```java
// ❌ BAD: Using larger types than needed
public class Product {
    private long id;                 // 8 bytes
    private int quantity;            // 4 bytes
    private double price;            // 8 bytes
}

// ✅ GOOD: Right-sized types
public class Product {
    private int id;                  // 4 bytes (if IDs < 2 billion)
    private short quantity;          // 2 bytes (if quantity < 32,767)
    private float price;             // 4 bytes (if precision OK)
    
    // Savings: 6 bytes per object
    // For 10 million products: 60 MB saved!
}
```

---

## Memory Profiling Tools

### Q6: How will you go about evaluating sizeof an object in Java?

Java doesn't have a `sizeof` operator like C++, but we can use profiling tools.

### Using jmap

```bash
# Step 1: Find process ID
jps

# Output:
# 8896 MyApplication
# 8420 Jps

# Step 2: Generate heap histogram
jmap -histo:live 8896 > mem.txt

# Step 3: Inspect mem.txt
```

**Example Output:**

```
┌─────────────────────────────────────────────────────────────┐
│                  JMAP HEAP HISTOGRAM                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  num  #instances    #bytes  class name                      │
│  ───────────────────────────────────────────────────────    │
│   1:    1000000  16000000  java.lang.Integer               │
│   2:     500000  12000000  java.lang.String                │
│   3:     250000   6000000  com.example.Product             │
│   4:     100000   2400000  java.util.ArrayList             │
│                                                              │
│  Total: 36.4 MB                                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Using VisualVM

```
┌─────────────────────────────────────────────────────────────┐
│                  VISUALVM HEAP DUMP                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Class Name              Instances    Size (MB)             │
│  ─────────────────────────────────────────────────────      │
│  Integer                 1,000,000      15.3                │
│  String                    500,000      12.0                │
│  Product                   250,000       6.0                │
│  ArrayList                 100,000       2.4                │
│  HashMap$Entry              50,000       1.6                │
│                                                              │
│  Total Heap Usage: 512 MB / 1024 MB                         │
│  GC Activity: 15 collections in last minute                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Monitoring Object Creation

```java
// Problem: Detecting unnecessary object creation
public class AutoBoxUnbox {
    public static void main(String[] args) throws InterruptedException {
        Integer sum = 0;  // Wrapper!
        for (int i = 1000; i < 500000; i++) {
            sum += i;  // Creates new Integer each iteration!
            Thread.sleep(100);
        }
    }
}
```

**jmap Output Over Time:**

```
Initial:
num  #instances    #bytes  class name
───────────────────────────────────────
 7:      1318     21088  java.lang.Integer

After 10 seconds:
num  #instances    #bytes  class name
───────────────────────────────────────
 7:      1704     27264  java.lang.Integer

After 20 seconds:
num  #instances    #bytes  class name
───────────────────────────────────────
 7:      2156     34496  java.lang.Integer

Growing instances = Memory leak!
```

**Fixed Version:**

```java
public class AutoBoxUnbox {
    public static void main(String[] args) throws InterruptedException {
        int sum = 0;  // Primitive!
        for (int i = 1000; i < 500000; i++) {
            sum += i;  // No object creation
            Thread.sleep(100);
        }
    }
}
```

**jmap Output (Fixed):**

```
Initial:
num  #instances    #bytes  class name
───────────────────────────────────────
 8:       256      4096  java.lang.Integer

After 20 seconds:
num  #instances    #bytes  class name
───────────────────────────────────────
 8:       256      4096  java.lang.Integer

Stable! No memory leak.
```

---

## Real-World Scenarios

### Scenario 1: E-Commerce Product Catalog

```java
// Problem: 10 million products in memory

// ❌ INEFFICIENT: 1.5 GB memory
public class Product {
    private Long id;                    // 24 bytes
    private String name;                // 40+ bytes
    private Double price;               // 24 bytes
    private Integer stockQuantity;      // 16 bytes
    private Boolean available;          // 16 bytes
    // Total: ~120 bytes per product
    // 10M products: 1.2 GB
}

// ✅ OPTIMIZED: 400 MB memory
public class Product {
    private int id;                     // 4 bytes
    private String name;                // 40+ bytes (can't optimize much)
    private float price;                // 4 bytes
    private short stockQuantity;        // 2 bytes
    private boolean available;          // 1 byte
    // Total: ~51 bytes per product
    // 10M products: 510 MB
    
    // Savings: 690 MB (58% reduction!)
}
```

**Memory Comparison:**

```
┌─────────────────────────────────────────────────────────────┐
│      E-COMMERCE CATALOG MEMORY (10M products)               │
└─────────────────────────────────────────────────────────────┘

Inefficient (wrappers):
████████████████████████████████████████████████████ 1.2 GB

Optimized (primitives):
████████████████████ 510 MB

Savings: 690 MB (58%)
```

### Scenario 2: Time-Series Sensor Data

```java
// Problem: Storing 1 million sensor readings per day

// ❌ INEFFICIENT: 96 MB per day
public class SensorReading {
    private Long timestamp;             // 24 bytes
    private Double temperature;         // 24 bytes
    private Double humidity;            // 24 bytes
    private Integer sensorId;           // 16 bytes
    // Total: 88 bytes per reading
}

// ✅ OPTIMIZED: 24 MB per day
public class SensorReading {
    private long timestamp;             // 8 bytes
    private float temperature;          // 4 bytes
    private float humidity;             // 4 bytes
    private int sensorId;               // 4 bytes
    // Total: 20 bytes per reading
    
    // Savings: 72 MB per day (75% reduction!)
    // Over 1 year: 26 GB saved!
}
```

### Scenario 3: Gaming - Position Tracking

```java
// Problem: Tracking 100,000 player positions in real-time

// ❌ INEFFICIENT: 9.6 MB
public class PlayerPosition {
    private Integer playerId;           // 16 bytes
    private Double x;                   // 24 bytes
    private Double y;                   // 24 bytes
    private Double z;                   // 24 bytes
    // Total: 88 bytes per position
}

// ✅ OPTIMIZED: 2 MB
public class PlayerPosition {
    private int playerId;               // 4 bytes
    private float x;                    // 4 bytes
    private float y;                    // 4 bytes
    private float z;                    // 4 bytes
    // Total: 16 bytes per position
    
    // Savings: 7.6 MB (79% reduction!)
    // Better cache performance too!
}
```

**Performance Impact:**

```
┌─────────────────────────────────────────────────────────────┐
│         GAMING POSITION UPDATE PERFORMANCE                  │
└─────────────────────────────────────────────────────────────┘

Inefficient (wrappers):
Memory: 9.6 MB
Cache misses: High
Update time: 15ms per frame

Optimized (primitives):
Memory: 2 MB
Cache misses: Low
Update time: 3ms per frame

Result: 5x faster updates!
```

---

## Summary

### Key Takeaways

```
┌─────────────────────────────────────────────────────────────┐
│           MEMORY OPTIMIZATION CHECKLIST                     │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ✅ Use primitives instead of wrappers when possible        │
│  ✅ Choose the smallest appropriate type                    │
│  ✅ Be aware of object header overhead (12-16 bytes)        │
│  ✅ Wrapper objects use 4-16x more memory                   │
│  ✅ Arrays have 16 bytes overhead                           │
│  ✅ Set initial capacity for collections                    │
│  ✅ Use lazy initialization for optional fields             │
│  ✅ Profile memory usage with jmap/VisualVM                 │
│  ✅ Optimize hot paths and large collections                │
│  ✅ Balance memory vs code clarity                          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Memory Impact Summary

| Optimization | Memory Saved | When to Use |
|--------------|--------------|-------------|
| Primitive vs Wrapper | 4-16x | Always, unless null needed |
| Right-sized types | 2-4x | Large collections |
| Initial capacity | 2-3x | Known collection size |
| Lazy initialization | Variable | Optional/rare fields |

### Quick Reference

```
┌──────────────────────────────────────────────────────────────┐
│         MEMORY CONSUMPTION QUICK REFERENCE                   │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  Primitive int:           4 bytes                            │
│  Integer object:         16 bytes (32-bit) / 24 bytes (64-bit)│
│  int[100]:              416 bytes                            │
│  Integer[100]:        2,016 bytes                            │
│  ArrayList<Integer>:  2,000+ bytes (for 100 elements)        │
│  HashMap<K,V>:        ~32 bytes per entry + overhead         │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

---

**[← Back to Data Types](data-types.md)** | **[Next: Autoboxing & Unboxing →](autoboxing-unboxing.md)**

**[↑ Back to Module Index](README.md)**