# 4 Java Garbage Collection Interview Questions & Answers

## Table of Contents

- [Q37: What do you know about the Java garbage collector? When does the garbage collect](#q37)
- [Q38: What is an unreachable object?](#q38)
- [Q39: What is the dif ference between a weak reference and a soft reference? Which one](#q39)
- [Q40: If you have a circular reference of objects, but you no longer reference it from](#q40)
- [Q41: What is the main dif ference between pass-by-reference and pass-by-value? Which ](#q41)
- [Q42: How would you take advantage of Java being a stack based language? What is a ree](#q42)

---

## Q37: What do you know about the Java garbage collector? When does the garbage collection occur?

**Answer:**

Each time an object is created in Java, it goes into the area of memory known as heap. The Java heap is called the garbage collectable heap. The garbage
collection cannot be forced. The garbage collector runs in low memory situations. When it runs, it releases the memory allocated by an unreachable object. The
garbage collector runs on a low priority daemon (i.e. background) thread. Y ou can nicely ask the garbage collector to collect garbage by calling System.gc( ) but
you can’ t force it.

---

## Q38: What is an unreachable object?

**Answer:**

An object’ s life has no meaning unless something has reference to it. If you can’ t reach it then you can’ t ask it to do anything. Then the object becomes
unreachable and the garbage collector will figure it out. Java automatically collects all the unreachable objects periodically and releases the memory consumed
by those unreachable objects to be used by the future reachable objects.
You can use the following options with the Java command to enable tracing for garbage collection events.
ava -verbose :gc

Java Garbage Collection reachable vs unreachable objects

---

## Q39: What is the dif ference between a weak reference and a soft reference? Which one would you use for caching?

**Answer:**

Weak r eference : A weak reference, simply put, is a reference that isn’ t strong enough to force an object to remain in memory . Weak references allow you
to leverage the garbage collector ’s ability to determine reachability for you, so you don’ t have to do it yourself. Y ou create a weak reference like this:
A weak reference is a holder for a reference to an object, called the referent. W eak references and weak collections are powerful tools for heap management,
allowing the application to use a more sophisticated notion of reachability , rather than the “all or nothing” reachability of fered by ordinary (i.e. strong)
references.
A WeakHashMap stores the keys using WeakRefer ence objects, which means that as soon as the key is not referenced from somewhere else in your program, the
entry may be removed and is available for garbage collection. One common use of WeakRefer ences and WeakHashMaps in particular is for adding properties to
objects. If the objects you are adding properties to tend to get destroyed and created a lot, you can end up with a lot of old objects in your map taking up a lot of
memory . If you use a WeakHashMap instead the objects will leave your map as soon as they are no longer used by the rest of your program, which is the desired
behavior .
Soft r eference is similar to a weak reference, except that it is less eager to throw away the object to which it refers. An object which is only weakly reachable
will be discarded at the next garbage collection cycle, but an object which is softly reachable will generally stick around for a while as long as there is enough
memory . Hence the soft references are good candidates for a cache .
The garbage collector may or may not reclaim a softly reachable object depending on how recently the object was created or accessed, but is required to clear allCar c1 = new Car( ); //referent is c1 is a strong reference
WeakReference <Car> wr = new WeakReference <Car>(c1);
byte[ ] cache = new byte[1024 ];
/... populate the cache. The referent is cache
SoftReference <byte> sr = new SoftReference <byte>(cache );

soft references before throwing an OutOfMemoryError .
Note : The weak references are eagerly garbage collected, and the soft references are lazily garbage collected under low memory situations.

---

## Q40: If you have a circular reference of objects, but you no longer reference it from an execution thread, will this object be a potential candidate for garbage
collection?

**Answer:**

Yes. Refer diagram below .
Java GC cyclic reference

---

## Q41: What is the main dif ference between pass-by-reference and pass-by-value? Which one does Java use?

**Answer:**

Other languages use pass-by-reference or pass-by-pointer . But in Java no matter what type of ar gument you pass the corresponding parameter (primitive
variable or object reference) will get a copy of that data, which is exactly how pass-by-value (i.e. copy-by-value) works.

pass-by-ref vs pass-by-value
In Java, if a calling method passes a reference of an object as an ar gument to the called method, then the passed-in reference gets copied first and then passed to
the called method. Both the original reference that was passed-in and the copied reference will be pointing to the same object. So no matter which reference you
use, you will be always modifying the same original object , which is how the pass-by-r eference works as well.

pass-by-ref vs pass-by-value
If your method call involves inter -process (e.g. between two JVMs) communication, then the reference of the calling method has a dif ferent address space to the
called method sitting in a separate process (i.e. separate JVM). Hence inter -process communication involves calling method passing objects as ar guments to
called method by-value in a serialized form.

---

## Q42: How would you take advantage of Java being a stack based language? What is a reentrant method?

**Answer:**

Recursive method calls are possible with stack based languages and re-entrant methods.
A re-entrant method would be one that can safely be entered, even when the same method is being executed, further down the call stack of the same thread. A
non-re-entrant method would not be safe to use in that way . For example, writing or logging to a file can potentially corrupt that file, if that method were to be
re-entrant.
A function is recursive if it calls itself. Given enough stack space, recursive method calls are perfectly valid in Java though it is tough to debug. Recursive
functions are useful in removing iterations from many sorts of algorithms. All r ecursive functions ar e re-entrant , but not all re-entrant functions are recursive.
Stack uses LIFO (Last In First Out), so it remembers its ‘caller ’ and knows whom to return when the function has to return. Recursion makes use of system
stack for storing the return addresses of the function calls.
public class RecursiveCall {
 public int countA (String input ) {
 
 // exit condition – recursive calls must have an exit condition
 //otherwise you will get stack overflow error
 if (input == null || input .length ( ) == 0) {
 return 0;
 }
 int count = 0;
 
 //check first character of the input
 if (input .substring (0, 1).equals ("A")) {
 count = 1;
 }
 
 //recursive call to evaluate rest of the input
 //(i.e. 2nd character onwards)
 return count + countA (input .substring (1));
 }
 public static void main (String [ ] args) {
 System .out.println (new RecursiveCall ( ).countA ("AAA rating" )); // 3

Java is a stack based language
Recursion might not be the ef ficient way to code, but recursive functions are shorter , simpler , and easier to read and understand. Recursive functions are very
handy in working with tree structures and avoiding unsightly nested for loops. If a particular recursive function is identified to be a real performance bottleneck
as it is invoked very frequently or it is easy enough to implement using iteration like the sample code below , then favor iteration over recursion. }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
