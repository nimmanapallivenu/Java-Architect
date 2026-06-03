# Primitives & Memory Management

> **Module**: Java Data Types  
> **Topic**: Primitives & Memory Management

---

## 📋 Table of Contents



- [Q1: How much memory space does a primitive type int occupy in Java?](#q1)
- [Q2: Java objects get stored in the heap memory space, but how about the primitive va](#q2)
- [Q3: How much space does java.lang.Integer object occupy?](#q3)
- [Q4: How much space does java.lang.Integer[] array with 1 element occupy on a 32 bit ](#q4)
- [Q5: Can you arrange the following Collection data types in terms if their memory ove](#q5)
- [Q6: How will you go about evaluating sizeof an object in Java?](#q6)
- [Q7: What are the best practices with regard to conserving memory when using Java Col](#q7)

---

## 🔹 Q1: How much memory space does a primitive type int occupy in Java?

**Answer:**

4 bytes.
Java primitive data types

---

## 🔹 Q2: Java objects get stored in the heap memory space, but how about the primitive variables?

**Answer:**

It depends.
Stack vs Heap

1) Primitives defined locally would be on the stack .
2) If a primitive is defined as part of an instance of an object, that primitive would be on the heap

---

## 🔹 Q3: How much space does java.lang.Integer object occupy?

**Answer:**

Java objects need to store 1) object metadata information and then the 2) data .
java.lang.Integer object metadata on a 32bit JVMpublic class Primitive
{
 public static void main (String [] args)
 {
 int number = 1; // This is on the stack.
 }
}
class MyW rapper {
 int number; // this will be on the heap.
 public MyW rapper (int number ) {
 this.number = number ;
 } 

1) Class information: 32 bits = 4 bytes .
2) Flags : array or not, hashCode, etc : 32 bits = 4 bytes .
3) Lock information: synchronization 32 bits = 4 bytes .
java.lang.Integer data
int is = 32 bits = 4 bytes .
Total memory occupied on a 32bit JVM is = 128 bits = 16 bytes. This is 4 times the space occupied by a primitive.
java.lang.Integer object metadata on a 64bit JVM
1) Class information: 64 bits = 8 bytes .
2) Flags : array or not, hashCode, etc : 64 bits = 8 bytes .
3) Lock information: synchronization 64 bits = 8 bytes .
java.lang.Integer data
int is 32 bits = 4 bytes .
Total memory occupied on a 64bit JVM is = 224 bits = 28 bytes .
So, if you take an application that was running on a 32 bit JVM and port it to a 64 bit JVM, it is going to require more memory .

---

## 🔹 Q4: How much space does java.lang.Integer[] array with 1 element occupy on a 32 bit JVM?

**Answer:**

Very similar to an Integer object, but requires an extra object data called “ size”
java.lang.Integer[] object metadata on a 32bit JVM
1) Class information: 32 bits = 4 bytes .
2) Flags: array or not, hashCode, etc: 32 bits = 4 bytes .
3) Lock information: synchronization: 32 bits = 4 bytes .
4) Size of the array 32 bits = 4 bytes .
Then depending on how many elements are in an array: 32 bits or 4 bytes per data.
An array of size 1 will consume = 160 bits = 20 bytes.

---

## 🔹 Q5: Can you arrange the following Collection data types in terms if their memory overheads in ascending order?
1) ArrayList
2) LinkedList
3) HashSet
4) HashMap

**Answer:**

ArrayList –> LinkedList –> HashMap –> HashSet.
1) ArrayList is the least in terms of memory overhead as it is backed by a data structure of type array. A default size of an array list is 10 entries.
2) A HashSet has the highest memory overhead and it takes more memory than a HashMap because internally a HashSet uses a HashMap to store data. So, it
needs space for the HashMap + additional meta data space to wrap around a HashMap.
3) A HashMap by default creates a backing data structure (i.e. an array) with a capacity for 16 objects regardless of you add all 16 objects or not. Hence it
consumes more memory than a LinkedList as a linked list only occupies space for whatever data that is added.
4) A HashMap uses additional object entries for key, value, next reference (i.e. for iterating), and an int to stor e hash value whereas a LinkedList uses only
next & previous references in addition the data themselves.
So, when using a collection type in Java, it is always a trade off between memory usage & functionality. Some collection types even though consume more
memory, but functionally more ef ficient. For example, a HashMap lookup of elements on average is O(1). This is explained Understanding “Big O” Notation in
Java with examples .

---

## 🔹 Q6: How will you go about evaluating sizeof an object in Java?

**Answer:**

Java does not have a sizeof operator like C++ does. Java uses automatic memory management known as the Garbage Collection, hence it is not that
important to evaluate size of various objects. But, for the learning purpose, I have used “jvisualvm”, which is a very handy & free profiling tool that gets
shipped with the JDK. Step by step instructions are provided: jvisualvm to sample Java heap memory

---

## 🔹 Q7: What are the best practices with regard to conserving memory when using Java Collections?

**Answer:**

1) Set the initial capacity of the collection appropriately so that the space is not unnecessarily wasted. Most collections double their capacity when the
current capacity is reached.
For example, to store 130 elements in a Map, initialize it to say 150, rather than using the default capacity of 16, which has to grow like 16 –> 32 –> 64 –> 128
–> 256, where 256, where 256 is a lot greater than 130.
2) Lazily initialize your collections. This means initialize it just before adding elements.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed

« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
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