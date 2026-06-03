# 6 Java Objects Interview Questions & Answers

## Table of Contents

- [Q43: What can you tell about the performance of a HashMap compared to a TreeMap ? Whi](#q43)
- [Q46: When defining a user defined key class, what other consideration apart from over](#q46)
- [Q47: What is an immutable object? Why is it a best practice to use immutable objects ](#q47)
- [Q48: How do you create an immutable type?](#q48)
- [Q51: How do you exclude a field of a class from serialization?](#q51)
- [Q52: What happens to static fields during serialization?](#q52)
- [Q53: What are the benefits of serialization?](#q53)
- [Q54: What is a serial version id?](#q54)

---

## Q43: What can you tell about the performance of a HashMap compared to a TreeMap ? Which one would you prefer?

**Answer:**

A balanced tree does have O (log n) performance. The TreeMap class in Java maintains key/value objects in a sorted order by using a red-black tree. A red-black
tree is a balanced binary tree. Keeping the binary tree balanced ensures the fast insertion, removal, and lookup time of O (log n). This is not as fast as a HashMap ,
which is O(1) , but the TreeMap has the advantage of that the keys are in sorted order which opens up a lot of other capabilities.
Which one to choose?
The decision as to using an unordered collection like a HashSet or HasMap versus using a sorted data structure like a TreeSet or TreeMap depends mainly on the usage
pattern , and to some extent on the data size and the environment you run it on. The practical reason for keeping the elements in sorted order is for frequent and faster
retrieval of sorted data if the inserts and updates are frequent. If the need for a sorted result is infrequent like prior to producing a report or running a batch process,
then maintaining an unordered collection and sorting them only when it is really required with Collections.sort(…) could sometimes be more ef ficient than maintaining
the ordered elements. This is only an opinion, and no one can of fer you a correct answer . Even the complexity theories like Big-O notation O(n) assume possibly lar ge
values of n. In practice, a O(n) algorithm can be much faster than a O(log n) algorithm, provided the data set that is handled is suf ficiently small. So, always conduct a
performance testing with the real life data to tune your code.
Q44 . When providing a user defined key class for storing objects in the HashMaps , what methods do you have to provide or override (i.e. method overriding)?
A44 . You should override the equals( ) and hashCode( ) methods from the Object class. The default implementation of the equals( ) and hashcode( ) , which are
inherited from the java.lang.Object uses an object instance’ s memory location (e.g. Car@6c60f2ea). This can cause problems when two instances of the car objects
have the same color but the inherited equals( ) will return false because it uses the memory location, which is dif ferent for the two instances. Also, the toString( )
method can be overridden to provide a proper string representation of your object.

Java equals vs hashCode

Note : Java hashCode( ) and equals( ) method have to be properly implemented. The map and set interfaces also use the containsKey(Object key) and contains (Object
o) methods use the equals( ) method to determine the return value – true/false. In the class defined by yourself, if you don’ t explicitly override these methods, it will
have a default implementation. It returns true if and only if two objects refer to the same object, i.e., x == y is true.

---

## Q46: When defining a user defined key class, what other consideration apart from overriding the equals(..) and hashCode( ) method you should think of?

**Answer:**

Implement the user defined key class as an immutable object .
As per the code snippet shown below if you use a mutable user defined class “ UserKey ” as a HashMap key and subsequently if you mutate (i.e. modify via setter
method e.g. key.setName(“Sam” )) the key after the object has been added to the HashMap then you will not be able to access the object later on. The original key
object will still be in the HashMap (i.e. you can iterate through your HashMap and print it – both prints as “Sam” as opposed to “John” & Sam), but you cannot access
it with map.get(key) or querying it with map.containsKey(key) will return false because the key “John” becomes “Sam” in the “List of keys” at the key index
“345678965” if you mutate the key after adding. These types of errors can be very hard to trace and fix.
Map myMap = new HashMap (10);
/add the key “John”
UserKey key = new UserKey (“John ”); //Assume UserKey class is mutable
myMap .put(key, “Sydney ”);
/ same key object is mutated instead of creating a new instance.
/ This line modifies the key value “John” to “Sam” in the “List of keys”
/ as shown in the diagram above. This means that the key “John” cannot be
/ accessed. There will be two keys with “Sam” in positions with hash
/ values 345678965 and 76854676. 
key.setName (“Sam”);

---

## Q47: What is an immutable object? Why is it a best practice to use immutable objects in Java?

**Answer:**

Immutable objects are objects whose state (i.e. the object’ s data) cannot change after construction. Examples of immutable objects from JDK include String and
wrapper classes like Integer , Double , Character , etc.
Immutable classes can greatly simplify programming by freely allowing you to cache and share the references to the immutable objects without having to
defensively copy them or without having to worry about their values becoming stale or corrupted. 
Immutable classes are inherently thread-safe and you do not have to synchronize access to them to be used in a multi-threaded environment. So there are no
chances for negative performance consequences as multiple threads can share the same instance. 
Eliminates the possibility of data becoming inaccessible when used as keys in HashMaps or as elements in Sets. These types of errors are hard to debug and fix. 
Eliminates the need for class invariant check once constructed. 
Allow h ashCode( ) method to use lazy initialization, by caching its return value. 
Cloning is not required.
Simpler to construct, use, and test due to its deterministic state.

---

## Q48: How do you create an immutable type?

**Answer:**

1) Make the class final so that it cannot be extended or use static factories and keep constructors private.
2) Make fields private and final.myMap .put(key, “Melbourne ”);
/ The key cannot be accessed. The key hashes to the same position
/ 345678965 in the “Key index array” but cannot be found in the “List of keys”.
myMap .get(new UserKey (“John ”));
public final class MyImmutable { … }

3) Don’ t provide any methods that can change the state of the immutable object in any way , not just setXXX methods, but any methods which can change the state.
4) The “this” reference is not allowed to escape during construction from the immutable class. Defensively copy references during construction.
5) Don’ t return or expose the mutable references to the caller . This can be done by defensively copying the objects by deeply cloning them.
For example, when constructing a date object
Another example would be when using a collectionprivate final int[ ] myArray ; 
public Person (Date birthDate ) {
super ( );
/defensively copy
his.birthDate = new Date (birthDate .getDate ());
}
/Don't let the date escape by returning a defensively copied date
public Date getBirthDate ( ) {
 //defensively copy so that original date does not escape and modified
 return new Date (this.bithDate .getTime( ));
}

Q49 . What is serialization? What rules do you need to follow?
A49 . Object serialization is a process of reading or writing an object. It is a process of saving an object’ s state to a sequence of bytes, as well as a process of rebuilding
those bytes back into a live object at some future time. An object is marked serializable by implementing the java.io.Serializable interface. This simply allows the
serialization mechanism to verify that a class can be persisted, typically to a file. The common process of serialization is also called marshaling or deflating when an
object is flattened into byte streams. The flattened byte streams can be unmarshaled or inflated back to an object.public Collection <role> getRoles ()
{
/returns immutable collection, so new roles cannot be added from outside
return Collections .unmodifiableCollection (roles );
}

Serialization
Rule #1: The object to be persisted must implement the Serializable interface or inherit that interface from its object hierarchy . Alternatively , you can use an
Externalizable interface to have full control over your serialization process. For example, to construct an object from a pdf file.
Rule #2: The object to be persisted must mark all non-serializable fields as transient. For example, file handles, sockets, threads, etc.
Rule #3: You should make sure that all the included objects are also serializable. If any of the objects is not serializable, then it throws a NotSerializableException.
Rule #4: Base or parent class fields are only handled if the base class itself is serializable.

Rule #5: Serialization ignores static fields, because they are not part of any particular state.

---

## Q51: How do you exclude a field of a class from serialization?

**Answer:**

By marking it as transient . The fields marked as transient in a serializable object will not be transmitted in the byte stream. An example would be a file handle,
a database connection, a system thread, etc. Such objects are only meaningful locally . So they should be marked as transient in a serializable class.

---

## Q52: What happens to static fields during serialization?

**Answer:**

Static fields are not serialized. Serialization persists only the state of a single object. Static fields are not part of the state of an object as they are ef fectively the
state of the class shared by many other instances.

---

## Q53: What are the benefits of serialization?

**Answer:**

1) Allows you to persist objects with state to a text file on a disk, and re-assemble them by reading the file back. Application servers can do this to conserve
memory . For example, stateful EJBs can be activated and passivated using serialization. The objects stored in an HTTP session should be serializable to support in-
memory replication of sessions to achieve scalability .
2) Allows you to send objects from one Java process to another using sockets, RMI, RPC, etc. In other words passing objects between processes.
Allows you to deeply clone any arbitrary object graph.
3) Allows you to deeply clone any arbitrary object graph.

---

## Q54: What is a serial version id?

**Answer:**

Say you create a “Pet” class, and instantiate it to “ myPet “, and write it out to an object stream. This flattened “ myPet ” object sits in the file system for some time.
Meanwhile, if the “ Pet” class is modified by adding a new field, and later on, when you try to read (i.e. deserialize or inflate) the flattened “ Pet” object, you get the
java.io.InvalidClassException – because all serializable classes are automatically given a unique identifier . This exception is thrown when the identifier of the class is
not equal to the identifier of the flattened object. If you really think about it, the exception is thrown because of the addition of the new field. Y ou can avoid this
exception being thrown by controlling the versioning yourself by declaring an explicit serialV ersionUID. There is also a small performance benefit in explicitly
declaring your serialV ersionUID because it does not have to be calculated.
So, it is a best practice to add your own serialV ersionUID to your Serializable classes as soon as you define them. If no serialV ersionUID is declared, JVM will use its
own algorithm to generate a default SerialV ersionUID. The default serialV ersionUID computation is highly sensitive to class details and may vary from dif ferent JVM
implementation, and result in an unexpected InvalidClassExceptions during deserialization process.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
