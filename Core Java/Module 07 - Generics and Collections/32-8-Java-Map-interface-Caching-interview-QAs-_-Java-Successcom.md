# 32. 8 Java Map interface Caching interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q1: What is the purpose of a Map interface in Java collection API?](#q1)
- [Q2: What is the purpose of using a cache?](#q2)
- [Q3: What are some of the requirements you need to consider when writing your own cac](#q3)
- [Q4: What are the differences between HashMap , TreeMap (i.e. SortedMap ), and a Lin](#q4)
- [Q6: How will you implement your own LRU cache in Java?](#q6)
- [Q7: What are the different types of caches?](#q7)
- [Q8: Will WeakHashMap make a good cache? Explain your answer?](#q8)
- [Q9: Where would you use a WeakHashMap ?](#q9)

---

## 🔹 Q1: What is the purpose of a Map interface in Java collection API?

**Answer:**

A map is a set of associations between pairs of objects. One is known as a “key” and the other is known as a “value”. It is used very frequently in
programming in case where for a given X, what is the Y?
1) For a given “user id” what is the “user detail”?
2) For a given “user id” what is the count of “logins”?
3) For a given “emp id” what is the cached copy of “employment details”?
A common use for a map is as a simple cache. It can be used as an enterprise level cache or as a temporary cache in data processing algorithms to lookup values
by key with O(1).

---

## 🔹 Q2: What is the purpose of using a cache?

**Answer:**

A cache is an area of local memory that holds a copy of frequently accessed data that is otherwise expensive to get or compute. Examples of such data
include a result of a query to a database, a disk file or a report. Caching is used primarily for performance and scalability . It is a mechanism by which subsequent
requests for the data are served faster .
Avoid the anti-pattern or pitfall of “Cache Them All”, and cache only the data that is hard to get.

---

## 🔹 Q3: What are some of the requirements you need to consider when writing your own cache or using a caching framework like EHCache , OSCache , etc?

**Answer:**

#1 Weak References to be Garbage Collected
The cache needs to have some boundary conditions defined to limit the memory usage. Each item in the cache can consume unbounded amounts of memory ,
hence the cache should hold its values using WeakReferences , which makes it possible for the cache to evict entries when memory is short, even when the cache
is not full or the boundary conditions are not met.
#2 Boundary Conditions
You need to have a replacement algorithm to pur ge entries from a cache when the boundary conditions are reached. For example, reaching the maximum
number of entries allowed. One such algorithm is LRU (Least Recently Used). In this algorithm cache entries which have not been accessed recently will be
replaced. If you are writing your own cache, one approach is to maintain a timestamp at which the entry was inserted and select the entry with the oldest
timestamp to be removed. But this search would be linear taking O(N) time. A more ef ficient approach would be to maintain the entries in a sorted collection
based on the order in which the entries were accessed. Alternatively , you can use a doubly linked list where the items that are accessed via the cache are moved
towards the end of the list, and pur ging of the entries in the cache are done from the front of the list.

#3 Stale Data Prevention
Some caches need to be regularly refreshed to prevent them from becoming stale. This can be accomplished by adding an expiry timestamp.
#4 Don’t reinvent the wheel
If you are writing your own caching mechanism, then the cache item insertion and lookup operations need to be fast preferably O(1) time. This means a
HashMap will be a potential candidate. The cached data can be accessed concurrently , hence a ConcurrentHashMap will be a good candidate. The cache
should not be locked when an entry is searched, and only the searched entry should be locked. In the scenario where multiple threads search for the same key ,
the computation of the key value should be performed only once.
Some or ganizations ask you to write your own LRU cache as a pre-interview screening exercise.
In real-life writing an industry strength caching framework is not a trivial task. So, “ don’t r einvent the wheel ” and favor utilizing a proven and tested caching
framework like EHCache that takes care of memory management with LRU and FIFO eviction strategies, disk overflow , data expiration and many other
optional advanced features, as opposed to writing your own.
The Google Gauva library provides a caching library:
mport com.google .common .base.MoreObjects ;
mport com.google .common .cache .CacheBuilder ;
mport com.google .common .cache .CacheLoader ;
mport com.google .common .cache .LoadingCache ;
/........
 //create a cache for employees based on their employee id
 LoadingCache <String , User > userCache =
 CacheBuilder .newBuilder ()
 .maximumSize (1000 ) // maximum 1000 records can be cached
 .expireAfterAccess (15, TimeUnit .MINUTES ) // cache will expire after 15 minutes of access
 .build (new CacheLoader <String , User >(){ // build the cacheloader
 
 @Override
 public User load(String userId ) throws Exception {

Having said this, there are cases where you might want to write your own simple caching solution or you might be asked in job interviews with questions like
— How will you implement an LRU cache in Java?

---

## 🔹 Q4: What are the differences between HashMap , TreeMap (i.e. SortedMap ), and a LinkedHashMap ?

**Answer:**

All three classes implement the Map interface and of fer mostly the same functionality . The difference is in how the entries are iterated through
HashMap does not of fer any guarantees about the iteration order . Its iteration order can change completely when new elements are added.
TreeMap will iterate according to the “ natural ordering ” of the keys according to their compar eTo( ) method or based on an externally supplied Comparator .
Additionally , it implements the SortedMap interface, which contains methods that depend on this sort order .
LinkedHashMap will iterate in the order in which the entries were put into the map . The LinkedHashMap also provides a great starting point for creating a
LRU Cache by overriding the removeEldestEntry( ) method. This lets you create a Cache that can expire data using some criteria that you define.

---

## 🔹 Q6: How will you implement your own LRU cache in Java?

**Answer:**

Here is an example code in Java
Firstly , define an interface so that the implementation can be changed if in future you have a ConcurrentLinkedHashMap class. A typical interface for a Java
cache will look like //make the expensive call
 return getFromDatabase (userId );
 }
 });
/....

Now , the implementation class.public interface LRUCache <K,V> {
 public abstract V put(K key, V item);
 public abstract V get(K key);
 public abstract V atomicGetAndSet (K key, V item);
}
mport java.lang.ref.SoftReference ;
mport java.util.LinkedHashMap ;
mport java.util.Map;
public class LRUCacheImpl <K, V> implements LRUCache <K, V> {
/SoftReference is used for a memory friendly cache.
/the value will be removed under memory shortage situations and
/the keys of the values will be removed from the cache map.
private final Map<K, SoftReference <V>> cache ;
public LRUCacheImpl (final int cacheSize ) {
// 'true' uses the access order instead of the insertion order .
this.cache = new LinkedHashMap <K, SoftReference <V>> (cacheSize , 0.75f , true) {
 private static final long serialV ersionUID = 1L;
 @Override
 protected boolean removeEldestEntry (Map.Entry <K, SoftReference <V>> eldest ) {
 // When to remove the eldest entry i.e Least Recently Used (i.e LRU) entry

The client class that uses the LRUCache return size() > cacheSize ; // Size exceeded the max allowed.
 }
};
@Override
public V put(K key, V value ) {
 SoftReference <V> previousV alueReference = cache .put(key, new SoftReference <V>(value ));
 return previousV alueReference != null ? previousV alueReference .get() : null;
@Override
public V get(K key) {
 SoftReference <V> valueReference = cache .get(key);
 return valueReference != null ? valueReference .get() : null;
@Override
public V atomicGetAndSet (K key, V value ) {
 V result = get(key);
 put(key, value );
 return result ;
public class LRUCacheClient {
 public static void main (String [] args) {
 LRUCache <String , String > cache = new LRUCacheImpl <String , String >(5);
 for (int i = 0; i < 10; i++) {
 cache .put("key" + i, "value" + i);
 }

Output:
Here are some key points on the above code that are worthy of mentioning not only in the job interviews to stand out from your competition, but also to write
quality code and get thumbs up in the code reviews.
#1. The above code would have been written as LRUCacheImpl<K, V>extends LinkedHashMap<K, V>, but the GoF design pattern favors composition over
inheritance as inheritance is more fragile to changes.
#2. SoftReference is used as opposed to a hard reference to force the items to be garbage collected when the Java heap runs low in memory .
#3. The methods are synchronized to be thread-safe. In future, if a Concurr ent LinkedHashMap were to be added to the Java API, then it can be used instead a
LinkedHashMap. . 
 
 //oldest entry will be returning null
 //only last 5 entries will be available
 for (int i = 0; i < 10; i++) {
 String value = cache .get("key" + i);
 System .out.println (value );
 }
 }
null
null
null
null
null
value5
value6
value7
value8
value9

#4. If performance is of utmost importance, then the public V get(K key) method can be executed asynchronously via a thread pool as this method gets
frequently executed.

---

## 🔹 Q7: What are the different types of caches?

**Answer:**

There are different types of caches
An application cache is a cache that an application accesses directly . An application benefits from using a cache by keeping most frequently accessed data in
memory . This is also known as the first level cache. For example, Hibernate can use EHCache, OSCache, SwarmCache, or JBoss T reeCache as its first level
cache .
Level 2 (aka L2) cache provides caching services to an object-relational mapping (ORM) framework or a data mapping (DM) framework such as Hibernate or
iBatis respectively by reducing the number of trips to the database. An L2 cache hides the complexity of the caching logic from an application.
In memory Data Grids/Clouds for distributed caching with powerful analysis and management tools to give you a complete solution for managing fast-
changing data in multiple servers, computer grid, or in the cloud. For example, Apache Hadoop , GridGain , Oracle Coherence , etc. A data grid is a reliable
distrbuted cache that uses an external data source to retrieve data that is not present in the cache and an external data store to write the updates. In simple terms,
grid or cloud computing is all about setting up an HTTP server to farm out requests RESTfully , and accept responses the same way . You can also use messaging
with JMS, RMI or even the old-school Corba. But why reinvent the wheel when there are open-source frameworks like Hadoop , GridGain , Hazelcast , etc

---

## 🔹 Q8: Will WeakHashMap make a good cache? Explain your answer?

**Answer:**

No. There are 2 reasons for it.
It uses weak references as the underlying memory management mechanism. If the garbage collector determines that an object is weakly r eachable , it will clear
atomically all weak references to the object, whereas if the garbage collector determines that an object is softly r eachable (i.e. uses a soft reference as opposed
to a weak reference), it may clear atomically all soft references to the object, in the case that it finds that memory is running low , or at its own discretion.
In a WeakHashMap weak references are used for the keys and not for the values that are stored. You want the map values to use weaker references.

---

## 🔹 Q9: Where would you use a WeakHashMap ?

**Answer:**

It is used for implementing canonical maps where you can associate some extra information to an object that you have a strong reference to. You put an
entry in a WeakHashMap with the object as the key , and the extra information as the map value. This means, as long as you keep a strong reference to the object,
you will be able to check the map to retrieve the extra information, and once you release the object, the map entry will be cleared and the memory used by the
extra information will be released to conserve memory .
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

