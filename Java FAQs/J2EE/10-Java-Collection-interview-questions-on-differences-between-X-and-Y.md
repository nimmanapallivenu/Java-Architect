# 10 Java Collection interview questions on differences between X and Y

## Table of Contents

- [Q1: What are the dif ferences between legacy and non-legacy collection classes in th](#q1)
- [Q2: What are the dif ferences between Enumeration and Iterator?](#q2)
- [Q3: What is the dif ference between an Iterable & an Iterator ?](#q3)
- [Q4: What are the dif ferences between fail-fast and fail-safe iterators?](#q4)
- [Q5: What are the dif ferences between ArrayList and LinkedList?](#q5)
- [Q6: Does a HasMap internally use a LinkedList or an ArrayList?](#q6)
- [Q7: What is the dif ference between an “unmodifiable” & an “immutable” collection?](#q7)
- [Q8: What are the dif ferences among HashSet, ArrayList, synchronized list/set, CopyO](#q8)
- [Q9: What are dif ferences between a HasMap and a T reeMap?](#q9)
- [Q10: What are dif ferences between a HashMap and a ConcurrentHashMap?](#q10)
- [Q11: What are dif ferences between a HashMap and a LinkedHashMap?](#q11)
- [Q12: What are dif ferences between a Queue and a BlockingQueue?](#q12)
- [Q13: What are dif ferences between an Array and a List?](#q13)
- [Q14: What is the dif ference between a Comparable and a Comparator?](#q14)
- [Q15: What is the dif ference between ArrayList and V ector?](#q15)
- [Q16: What is the dif ference between “with and without lambdas ” to the collections A](#q16)

---

## Q1: What are the dif ferences between legacy and non-legacy collection classes in the java.util package?

**Answer:**

Early version of Java defined several classes and one interface to store objects. These old classes are known as legacy classes. The legacy classes like
Vector , Hashtable, and Stack still exist in the API to provide forwards compatibility to code written during early days of Java.
These classes are now re-engineered with the “ Java Collections Framework (JCF) ” that looks as shown below with interfaces & classes.
List, Set, and Queue
Legacy classes & JCF
Map

Legacy Hashtable & JCF
All the methods of the legacy classes are synchr onized (i.e. locks the whole collection), hence can lead to performance issues. The methods of the re-
engineered Collections framework classes like ArrayList, HashSet, BlockingQueue, HashMap, etc are NOT synchronized (i.e. they don’ t lock). If you need
thread-safety you have two choices:
1) Using the java.util. Collections API methods provide a basic conditionally thread-safe implementation of Map and List.
2) Using the java.util. concurr ent package classes like ConcurrentHashMap, CopyOnW riteArrayList, etc that use better concurrency techniques. For example,
the ConcurrentHashMap uses a technique known as the “ lock striping “, which divides the whole map into several segments and locks only the relevant
segments, which allows multiple threads to access other segments of same ConcurrentHashMap without locking. So, concurrent reads are possible.Collections .synchronizedMap (aMap )
Collections .synchronizedList (aList )

Reference: http://www .javarticles.com/2012/06/concurrenthashmap.html
Similarly , CopyOnW riteArrayList & CopyOnW riteArraySet allow concurr ent r eads by multiple threads without requiring any locking and when a write
happens it copies the whole ArrayList or HashSet and swap with a newer one.

---

## Q2: What are the dif ferences between Enumeration and Iterator?

**Answer:**

Enumeration is old and it’ s there from JDK1.0 whereas iterator is newer . The key dif ference between Enumeration and iterator is that “Iterator has a
remove() method” whereas Enumeration doesn’ t. Enumeration acts as Read-only interface, whilst an Iterator can manipulate the objects like adding and
removing. Enumeration can only be used with legacy collection classes whereas an Iterator can be used with both legacy & non-legacy classes.

---

## Q3: What is the dif ference between an Iterable & an Iterator ?

**Answer:**

Java Iterable Vs Iterator dif ferences and know how .

---

## Q4: What are the dif ferences between fail-fast and fail-safe iterators?

**Answer:**

Iterators returned by most of pre JDK1.5 collection classes like V ector , ArrayList, HashSet, etc are fail-fast iterators . Iterators returned by JDK 1.5+
ConcurrentHashMap and CopyOnW riteArrayList classes are fail-safe iterators .

Use copy-on-write List/Set and concurrent maps from the java.util. concurr ent package to prevent Concurr entModificationException being thrown whilst
preserving thread safety . These classes provide fail-safe iteration as opposed to non-concurrent classes like ArrayList, HashSet, etc use fail-fast iteration
leading to Concurr entModificationException if you try to remove an element while iterating over a collection.
[ Further r eadings: Beginner what is wrong with this Java code? | Top 5 Core Java Exceptions and best practices ]
You need to choose the right data structure based on its usage/access patterns: More reads Vs more writes, FIFO (First-In-First-Out) Vs LIFO (Last-In-
First-Out), random reads, inserts in the middle vs end, does ordering matter?, duplicates allowed?, concurrent access possible? etc.

---

## Q5: What are the dif ferences between ArrayList and LinkedList?

**Answer:**

1. Insertions and deletions are faster in LinkedList compared to an ArrayList as LinkedList uses links (i.e. before and next reference) as opposed to an
ArrayList, which uses an array under the covers, and may need to resize the array if the array gets full. Adding to an ArrayList has a worst case scenario of O(n)
whilst LinkedList has O(1).
2. LinkedList has more memory footprint than ArrayList . An ArrayList only holds actual object whereas LinkedList holds both data and reference of next
and previous node.
3. Random access has the worst case scenario of O(n) in LinkedList as to access 6th element in a LinkedList with 8 elements, you need to traverse through 1
to 5th element before you can get to the 6th element, whereas in an ArrayList , you can get the 6th element with O(1) with list.get(5).
4. Add or remove from the head of the list is in favor of LinkedList with O(1) whereas O(n) for ArrayList.
5. Iterating over either kind of List is the same. ArrayList is technically faster , but the dif ference is small unless you’re doing something really performance-
sensitive.
What are these O(n), O(log n), O(1), etc? Learn more about understanding Big O notations through Java examples .

---

## Q6: Does a HasMap internally use a LinkedList or an ArrayList?

**Answer:**

Until Java 8: LinkedList. Java 8 onwards: a “binary tree” because in case of high collision the lookup is reduced to O(log n) from O(n) by using binary
trees. Learn more at HashMap & HashSet and how do they internally work?
Also when hash keys arrive from untrusted sources like HTTP headers, the resulting keys will have the same hashcode, which not only causes high collisions,
but also when you perform lots of look-ups, you will experience the Denial of Service (i.e. DoS) attacks.

---

## Q7: What is the dif ference between an “unmodifiable” & an “immutable” collection?

**Answer:**

An unmodifiable collection is often a wrapper around a modifiable collection. “unmodifiableList” will throw “java.lang.UnsupportedOperationException”
if you try to add/remove an element from it, but other code may still have access to “modifiableList”, which is a back door . So, you can’ t rely on the contents
not changing.
An immutable collection ensures that the collection itself cannot be altered by deeply cloning or copying the collection. It does not let its reference escape via
its constructors & getter methods as shown below:Collection <String > unmodifiableList = Collections .unmodifiableCollection (modifiableList );
mport java.util.ArrayList ;
mport java.util.List;
public final class MyImmutableList {
 
 private final List<String > myList ;
 public MyImmutableList (List<String > list) {
 
 List<String > clone = new ArrayList <String >(list.size());
 for (String item : list){
 clone .add(item);
 }
 
 this.myList = clone ; //cloned list is assigned
 //original list cannot be mutated from outside this class
 }
 public List<String > getMyList () {
 List<String > clone = new ArrayList <String >(myList .size());

---

## Q8: What are the dif ferences among HashSet, ArrayList, synchronized list/set, CopyOnW riteArraySet and CopyOnW riteArrayList?

**Answer:**

Compared to a list interface, a set interface does not allow duplicates. HashSet and ArrayList are not thread-safe and you need to provide your own
synchronization with locks or use the java.util. Collections class that provides utility methods like
The above synchronizedXXX lock the whole collection like the legacy V ector whereas CopyOnW riteArraySet and CopyOnW riteArrayList are not only thread-
safe, but also 1) more ef ficient as they allow concurr ent multiple reads and single write . This concurrent read and write behavior is accomplished by
making a brand new copy of the list every time it is altered.
The CopyOnW riteArrayList’ s iterator 2) never throws ConcurrentModificationException while Collections.synchronizedList’ s iterator may throw it. for (String item : myList ){
 clone .add(item);
 }
 return clone ; // cloned list is returned
 // original list cannot be mutated from outside this class
 }
 // ....equals(), hashCode(), etc
 @Override
 public String toString () {
 return "MyImmutableList [myList=" + myList + "]";
 } 
Collections .synchronizedList (aList );
Collections .synchronizedSet (aSet);
Collections .synchronizedCollection (aSetOrList );
Collections .synchronizedSortedSet (aSortedSet )

It is also imperative to note that as per the Java API for the “Collections” class’ s synchronizedXXX methods, the user must manually synchronize on the
returned list when iterating over it as shown below .
When to favor a CopyOnW riteXXX structur e? Write operations for a CopyOnW riteXXX can potentially be very slow as they involve copying the entire list .
The CopyOnW riteXXX is favored when the number of reads & traversals via an iterator are significantly more than the number of writes. It also gives you fail
safe iteration when you want to add/remove elements during iteration.

---

## Q9: What are dif ferences between a HasMap and a T reeMap?

**Answer:**

TreeMap is an implementation of a SortedMap, where the order of the keys can be sorted, and when iterating over the keys, you can expect that keys will
be in order . HashMap on the other hand, makes no such guarantee on the order .

---

## Q10: What are dif ferences between a HashMap and a ConcurrentHashMap?

**Answer:**

HashMap is not thread-safe and you need to provide your own synchronization with Collections.synchornizedMap(hashMap), which will return a
collection which is almost equivalent to the legacy Hashtable, where every modification operation on Map is locked.
What is wr ong with this code? HashMap race condition example & how ConcurrentHashMap fixes it?
As the name implies, ConcurrentHashMap provides thread-safety by dividing the whole map into dif ferent segments based upon concurrency level and locking
only particular segment instead of locking the whole map . In the ConcurrentHashMap API, you will find the following constants are usedList list = Collections .synchronizedList (new ArrayList ());
 ...
synchronized (list) {
 Iterator i = list.iterator (); // Must be in synchronized block
 while (i.hasNext ())
 foo(i.next());
}

and the constructor takes “concurrencyLevel” as an ar gument.
Instead of a map wide lock, ConcurrentHashMap maintains a list of 16 locks by default, which means each to lock on a single segment of the Map. Each
segment can have multiple buckets. This indicates that 16 threads can modify the ConcurrentHashMap at the same time as long as each thread works on a
different segment. The reads don’ t block at all.
ConcurrentHashMap does not allow NULL key values, whereas HashMap can hold only one null key . This is because if map.get(key) returns a null, you can’ t
distinguish even with map.contains(key) call whether the value is null or the key itself is not present as the map might have changed between the map.get(key)
and map.contains(key) calls due its concurrent read/write feature.
What is wr ong with this code? ConcurrentHashMap atomic operations issue & how to fix?

---

## Q11: What are dif ferences between a HashMap and a LinkedHashMap?

**Answer:**

LinkedHashMap will iterate in the order in which the entries were put into the map. HashMap does not provide any guarantees about the iteration order .

---

## Q12: What are dif ferences between a Queue and a BlockingQueue?

**Answer:**

BlockingQueue is a Queue that supports additional operations that wait for the queue to become non-empty when retrieving an element, and wait for
space to become available in the queue when storing an element. The main advantage is that a BlockingQueue is that it provides a correct, thread-safe
implementation with throttling.
The producers are throttled to add elements if the consumers get too far behind.
If the Queue capacity is limited, the memory consumption will be limited as well.

---

## Q13: What are dif ferences between an Array and a List?

**Answer:**

static final int DEFAUL T_INITIAL_CAP ACITY = 16;
static final int DEFAUL T_CONCURRENCY_LEVEL = 16;
public ConcurrentHashMap (int initialCapacity , float loadFactor , int concurrencyLevel )

Array is a fixed length data structure whilst a List is a variable length Collection class. List allows you to add and subtract elements even it is an O(n)
operation in worst case scenario.
An array can use primitive data types or objects, but the List classes can only use objects.
Arrays are inflexible and do not have the expressive power of generic types.
List gives you the data abstraction as you can swap ArrayList, LinkedList, CopyOnW riteArrayList, etc depending on the requirements.

---

## Q14: What is the dif ference between a Comparable and a Comparator?

**Answer:**

The Comparable interface provides a compareT o(..) method to be called while sorting naturally (i.e.by default). Y ou can define your own ordering (i.e.
custom ) logic through the compare(…) method by implementing the Comparator interface.
[ Further r eadings: 4 Sorting objects in Java interview Q&As | Different ways to sort a collection of objects in pre and post Java 8 ]

---

## Q15: What is the dif ference between ArrayList and V ector?

**Answer:**

Vector , Stack, and Hashtable are legacy data structures and must not to be used . All methods in these classes are synchronized (i.e. coarse grained lock),
hence not ef ficient. Favor the concurrent data structures for concurrent reads and single write.
Java 8

---

## Q16: What is the dif ference between “with and without lambdas ” to the collections API?

**Answer:**

Lambdas introduced in Java 8 would be worthless if we didn’ t have any means for applying lambdas to the JCF . So, “ default methods ” were
introduced to Java interfaces, which has the benefit that default methods don’ t break the implementations . In other words, interfaces in Java 8 onwards can now
implement behavior via the default methods. So, default methods filter , map, reduce, forEach, etc are now added to the “java.util.stream. Stream” interface.
The Iterable interface with the Default method is shown below ,
public interface Iterable <T> {
 public default void forEach (Consumer <? super T> consumer ) {
 for (T t : this) {
 consumer .accept (t);
 }
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
