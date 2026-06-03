# 27. 12 Java Generics Interview Q&As   Java Successcom

> **Module**: Core Java
> **Last Updated**: 2026-06-03
> **Difficulty**: Intermediate to Advanced

---

## 📋 Overview

This document covers essential Java concepts through interview questions and detailed answers.

---

## Table of Contents

- [Q2: Why do you need generics?](#q2)
- [Q3: What are the differences among?
– Raw or plain old collection type e.g. Collect](#q3)
- [Q4: How will you go about deciding which of the following to use?
1) <Number>
2) <? ](#q4)
- [Q6: Is the following line legal? If not, how will you fix it?List<String > list1 = n](#q6)
- [Q8: What do you understand by the term type argument inference?](#q8)
- [Q9: Is the following code snippet legal? If yes why and if not why not?](#q9)
- [Q10: Is it possible to generify methods in Java?](#q10)
- [Q11: Does the following code snippet compile? What does it demonstrate?](#q11)
- [Q12: Can you identify any issues with the following code? public <T> void doSomething](#q12)

---

## 🔹 Q2: Why do you need generics?

**Answer:**

Generics was introduced in JDK 5.0, and allows you to abstract over types. W ithout generics, you could put heterogeneous objects into a collection. This
can encourage developers to write programs that are harder to read and maintain. For example,List<String > myList = new ArrayList <String >( ); //in Java 6 and 7
List<String > myList = new ArrayList <>(); // In Java 8 can use empty <>
List myList = new ArrayList ( );
List list = new ArrayList ( );
ist.add(new Integer ( ));
ist.add(“A String ”);

As demonstrated above, without generics you can add any type of object to a collection. This means you would not only have to use “instanceof” operator , but
also have to explicitly cast any objects returned from this list. The code is also less readable. The following code with generics is not only more readable,
but also throws a compile time error if you try to add an Integer object to list1 or a String object to list2.

---

## 🔹 Q3: What are the differences among?
– Raw or plain old collection type e.g. Collection
– Collection of unknown e.g. Collection<?>
– Collection of type object e.g. Collection<Object>

**Answer:**

1) The plain old Collection : is a heterogeneous mixture or a mixed bag that contains elements of all types, for example Integer , String, Fruit, V egetable,
etc.
2) The Collection<object> : is also a heterogeneous mixture like the plain old Collection, but not the same and can be more restrictive than a plain old
Collection discussed above. It is incorrect to think of this as the super type for a collection of any object types.
Unlike an Object class is a super type for all objects like String, Integer , Fruit, etc, List<Object> is not a super type for List<String>, List<Integer>, List<Fruit>,
etc. So it is illegal to do the following:ist.add(new Mango ( ));
List<String > list1 = new ArrayList <String >( );
List<Integer > list2 = new ArrayList <Integer >( );
List<Object > list = new ArrayList <Integer >( );//illegal

Though Integer is a subtype of Object, List is not a subtype of List<Object> because List of Objects is a bigger set comprising of elements of various types like
Strings, Integers, Fruits, etc. A List of Integer should only contain Integers, hence the above line is illegal. If the above line was legal, then you can end up
adding objects of any type to the list, violating the purpose of generics.
3) The Collection<?> : is a homogenous collection that represents a family of generic instantiations of Collection like Collection<String>, Collection<Integer>,
Collection<Fruit>, etc.
Collection<?> is the super type for all generic collection as Object[ ] is the super type for all arrays.
Number class hierachy

---

## 🔹 Q4: How will you go about deciding which of the following to use?
1) <Number>
2) <? extends Number>
3) <? super Number>List<?> list = new ArrayList <Integer >( ); //legal
List<? extends Number > list = new ArrayList <Integer >( ); //legal

**Answer:**

Many developers struggle with the wild cards. Here is the guide:
1. Use the ? extends wildcard if you need to retrieve object from a data structure. That is read only . You can’ t add elements to the collection.
2. Use the ? super wildcard if you need to put objects in a data structure.
3. If you need to do both read and add elements, don’t use any wildcard .

---

## 🔹 Q6: Is the following line legal? If not, how will you fix it?List<String > list1 = new ArrayList <String >( );
List<Integer > list2 = new ArrayList <Integer >( );
System .out.println (list1.getClass ( ) == list2.getClass ( ));
f(list1 instanceof List<String >) //illegal
List<Object > list = new ArrayList <Integer >( );

**Answer:**

It is Illegal because Unlike an Object class is a super type for all objects like String, Integer , Fruit, etc, List&l;tObject> is not a super type for List<String>,
List<Integer>, List<Fruit>, etc. List<?> is the super type.
Note:<? extends Number> is read only and <?> is almost read only allowing only removce( ) and clear ( ) operations.
The Collection<?> can only be used as a reference type, and you cannot instantiate it.

---

## 🔹 Q8: What do you understand by the term type argument inference?

**Answer:**

The type inference happens when the compiler can deduce the type arguments of a generic type or method from a given context information. There are 2
situations in which the type argument inference is attempted during compile-time.
1. When an object of a generic type is created as demonstrated in the MyGenericClass<T>.
2. When a generic method is invoked. For example,}
public static void main (String [ ] args) {
 MyGenericClass val1 = new MyGenericClass (Integer .valueOf (37)); //auto-box
 MyGenericClass val2 = new MyGenericClass (Long .valueOf (250L )); //auto-box
 long result = ((Integer )val1.getObjT ype( )).longV alue( ) + ((Long )val2.getObjT ype( )).longV alue( );
 System .out.println (result );
 }
/T is inferred as an Integer
MyGenericClass <Integer > val1 = new MyGenericClass <Integer >(37);
/T is inferred as a Long 
MyGenericClass <Long > val2 = new MyGenericClass <Long >(250L );
mport java.util.ArrayList ;

mport java.util.List;
public class MyBasket {
 /**
 * The 'src' is the inferred type T or its sub type and the 'dest' is the
 * inferred type T or its super type.
 */
 public static <T> void copy (List<? extends T> src, List<? super T> dest) {
 for (T obj : src) {
 dest.add(obj);
 }
 }
 public static void main (String [] args) {
 List<Orange > orangeBasket = new ArrayList <Orange >(10);
 List<Mango > mangoBasket = new ArrayList <Mango >(10);
 orangeBasket .add(new Orange ());
 mangoBasket .add(new Mango ());
 List<Fruit > fruitBasket = new ArrayList <Fruit >(10);
 List<Orange > orangeBasket2 = new ArrayList <Orange >(10);
 orangeBasket2 .add(new Orange ());
 List<Mango > mangoBasket2 = new ArrayList <Mango >(10);
 mangoBasket2 .add(new Mango ());
 List<Fruit > fruitBasket2 = new ArrayList <Fruit >(10);
 fruitBasket2 .add(new Mango ());
 MyBasket .copy (orangeBasket2 , orangeBasket ); // T is an Orange
 MyBasket .copy (mangoBasket2 , mangoBasket ); // T is a Mango
 MyBasket .<Orange > copy (orangeBasket , fruitBasket ); // T is an Orange
 MyBasket .<Mango > copy (mangoBasket , fruitBasket ); // T is a Mango
 MyBasket .copy (fruitBasket2 , fruitBasket ); // T is a Fruit
 for (Fruit fruit : fruitBasket ) {
 fruit.peel();
 }
 }

The copy(…) method ensures that fruits from a mixed fruit basket cannot be copied to a basket that only holds oranges or mangoes. But a mixed fruit basket
allows fruits to be copied from any basket.

---

## 🔹 Q9: Is the following code snippet legal? If yes why and if not why not?

**Answer:**

It is not legal as new T( ) will cause a compile-time error . This is partially because there’ s no guarantee that the tar get class for raw type “T” has a
constructor that takes zero parameters and partially due to type erasure where the raw type “T” does not have any way of knowing the type of object you want to
construct at runtime.

---

## 🔹 Q10: Is it possible to generify methods in Java?

**Answer:**

Yes.public MyGenericClass ( ) {
 this.objType = new T( ); 
}
mport java.util.ArrayList ;
mport java.util.List;
public class MyGenericMethod {
 
 //Generified method
 public static <T> void addV alue(T value , List<T> list){//Line A
 list.add(value );
 }
 public static void main (String [ ] args) {
 List<Integer > listIntegers = new ArrayList <Integer >( );
 Integer value1 = new Integer (37);

Note : If you had used the wildcard List<?> instead of List<T> on line A, it would not have been possible to add elements. You will get a compile-time error . So
how does the compiler know the type of “T”? It infers this from your use of the method. The generated class file looks pretty much the same as the source file
without the <Integer> and <String> angle brackets

---

## 🔹 Q11: Does the following code snippet compile? What does it demonstrate?

**Answer:**

Yes, the above code snippet does compile. It demonstrates that the type parameter in the class name and the type parameter in the method are actually
different parameters. The method signature, addV alue(value1 , listIntegers ); //T is inferred as an Integer
 System .out.println ("listIntegers=" + listIntegers );
 
 List<String > listString = new ArrayList <String >( );
 String value2 = "Test";
 addV alue(value2 , listString ); //T is inferred as a String
 System .out.println ("listString=" + listString );
 }
public class Generics4 <T> {
 
 public <T> void doSomething (T data) {
 System .out.println (data);
 }
 
 public static void main (String [ ] args) {
 Generics4 <String > g4 = new Generics4 <String >( );
 g4.doSomething (123);
 }

really means,

---

## 🔹 Q12: Can you identify any issues with the following code? public <T> void doSomething (T data)
public void doSomething (Object data)
mport java.util.ArrayList ;
mport java.util.Iterator ;
mport java.util.List;
public class GenericsW ithIterators {
 public static void main (String [ ] args) {
 List<Integer > listIntegers = new ArrayList <Integer >( ); //1
 listIntegers .add(5); //2
 listIntegers .add(3); //3
 
 Iterator it = listIntegers .listIterator ( ); //4
 
 while (it.hasNext ( )){ //5
 Integer i = it.next( ); //6
 System .out.println (i); //7

**Answer:**

Line 4 will cause compile-time error on line 6 as the iterator is not generic. T o fix this, replace line 4 with:
or add an explicit cast to line 6.
The fix 1 is preferred. When you get an iterator , keyset, or values from a collection, assign it to an appropriate parametrized type as shown in fix 1.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » }
 }
terator <Integer > it = listIntegers .listIterator ( ); // fix 1
nteger i = (Integer ) it.next( );// fix 2

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

