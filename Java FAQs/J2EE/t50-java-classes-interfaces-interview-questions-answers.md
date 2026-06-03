# 3 T50 Java classes & interfaces interview questions & answers

## Table of Contents

Focus is on Java classes, interfaces and generics the interview questions and answers style. Java classes and interfaces are the building blocks.
Q24 . What happens when a parent and a child class have the same variable name?
A24 . When both a parent class and its subclass have a field with the same name, this technique is called variable shadowing or variable hiding . The variable
shadowing depends on the static type of the variable in which the object’ s reference is stored at compile time and NOT based on the dynamic type of the actual
object stored at runtime as demonstrated in polymorphism via method overriding.
Q25 . What happens when the parent and the child class has the same non-static method with the same signature?
A25 . Unlike variables, when a parent class and a child class each has a non-static method (aka an instance method) with the same signature, the method of the
child class overrides the method of the parent class. The method overriding depends on the dynamic type of the actual object being stored and NOT the static
type of the variable in which the object reference is stored. The dynamic type can only be evaluated at runtime. As you can see, the rules for variable shadowing
and method overriding are directly opposed. The method overriding enables polymorphic behavior .
Q26 . What happens when the parent and the child class has the same static method with the same signature?
A26 . The behavior of static methods will be similar to the variable shadowing or variable hiding , and not recommended. It will be invoking the static method
of the referencing static object type determined at compile time, and NOT the dynamic object type being stored at runtime.
The concepts of overriding, shadowing or hiding, and overloading are explained with code and examples .
Q27 . What is the dif ference between an abstract class and an interface and when should you use them?
A27 . In design, you want the base class to present only an interface for its derived classes. This means, you don’ t want anyone to actually instantiate an object
of the base class.
Before Java 8 :
Abstract class Interface
Can maintain state in instance and static variables. No state.
Have executable methods and abstract methods. Have no implementation code. All methods are abstract.
Can only extend one abstract class. A class can implement any number of interfaces.
public ClassB extends ClassA {....} public ClassB implements InterfaceA , InterfaceB {...}

Java 8 onwards :
Abstract class Interface
Can maintain state in instance and static variables. No state.
Have executable methods and abstract methods. Have executable default methods and static helper methods.
Can only subclass one abstract class. A class can implement any number of interfaces.
So, from Java 8 onwards, the key dif ference is only one class can extend an abstract class, but a class can implement more than one interfaces.
This is a very important “MUST KNOW” question, and if you are a beginner to intermediate level Java developer , then learn more
1. Java abstract classes Vs interfaces Q&A
2. Java classes and interfaces are the building blocks
3. Explain abstraction, encapsulation, Inheritance, and polymorphism with the given code?
Q28 . When will you use an abstract class?
A28 . In case where you want to use implementation inheritance then it is usually provided by an abstract base class. Abstract classes are excellent candidates
inside of application frameworks. Abstract classes let you define some default behavior and force subclasses to provide any specific behavior . Care should be
taken not to overuse implementation inheritance as discussed before. The template method design pattern is a good example to use an abstract class where the
abstract class provides a default implementation.
Q29 . When will you use an interface? 
A29 . For polymorphic interface inheritance, where the client wants to only deal with a type and does not care about the actual implementation, then use
interfaces. If you need to change your design frequently , you should prefer using interface to abstract class. Coding to an interface reduces coupling. Another
justification for using interfaces is that they solve the ‘diamond problem’ of traditional multiple inheritance as shown in the diagram. Java does not support
multiple inheritance. Java only supports multiple interface inheritance. Interface will solve all the ambiguities caused by this ‘diamond problem’. Java 8 has
introduced functional interfaces, and partially solves the diamond problem by allowing default and static method implementations in interfaces to inherit
multiple behaviors.

Multiple inheritance and diamond issue
Strategy design pattern lets you swap new algorithms and processes into your program without altering the objects that use them.
Q30 . What is a marker or a tag interface? Why there are some interfaces with no defined methods (i.e. marker interfaces) in Java? 
A30 . The interfaces with no defined methods act like markers. They just tell the compiler that the objects of the classes implementing the interfaces with no
defined methods need to be treated dif ferently . For example, java.io.Serializable , java.lang.Cloneable , java.util.EventListener , etc. Marker interfaces are also
known as “tag” interfaces since they tag all the derived classes into a category based on their purpose.
Now with the introduction of annotations in Java 5, the marker interfaces make less sense from a design standpoint. Everything that can be done with a marker
or tag interfaces in earlier versions of Java can now be done with annotations at runtime using reflection. One of the common problems with the marker or tag
interfaces like Serializable , Cloneable , etc is that when a class implements them, all of its subclasses inherit them as well whether you want them to or not. Y ou
cannot force your subclasses to un-implement an interface. Annotations can have parameters of various kinds, and they’re much more flexible than the marker
interfaces. This makes tag or marker interfaces obsolete, except for situations in which empty or tag interfaces can be checked at compile-time using the type-
system in the compiler
Q31 . What is a functional interface? 
A31 . Functional interfaces are introduced in Java 8 to allow default and static method implementations to enable functional programming (aka closures) with
lambda expressions.

Q32 . What does the @FunctionalInterface do, and how is it dif ferent from the @Override annotation?
A32 . The @Override annotation takes advantage of the compiler checking to make sure you actually are overriding a method when you think you are. This
way, if you make a common mistake of misspelling a method name or not correctly matching the parameters, you will be warned that you method does not
actually override as you think it does. Secondly , it makes your code easier to understand because it is more obvious when methods are overwritten.
The annotation @FunctionalInterface acts similarly to @Override by signaling to the compiler that the interface is intended to be a functional interface. The
compiler will then throw an error if the interface has multiple abstract methods.
Q33 . What do you understand by the term type erasure with regards to generics? 
A33 . The term type erasures is used in Java generics. In the interest of backward compatibility , robustness of generics has been sacrificed through type erasure.
Type erasure takes place at compile-time. So, after compiling List and List , both end up as List at runtime. They are both just lists.
after compilation becomes@FunctionalInterface
public interface Summable {
 abstract int sum(int input1 , int input2 );
}
List<String > myList = new ArrayList <String >( ); //in Java 6 and 7
List<String > myList = new ArrayList <>(); // In Java 8 can use empty <>
List myList = new ArrayList ( ); 

Java generics dif fer from C++ templates. Java generics (at least until JDK 8), generate only one compiled version of a generic class or method regardless of the
number of types used. During compile-time, all the parametrized type information within the angle brackets are erased and the compiled class file will look
similar to code written prior to JDK 5.0. In other words, Java does not support runtime generics.
Q34 . Why do you need generics?
A34 . Generics was introduced in JDK 5.0, and allows you to abstract over types. W ithout generics, you could put heterogeneous objects into a collection. This
can encourage developers to write programs that are harder to read and maintain. For example,
As demonstrated above, without generics you can add any type of object to a collection. This means you would not only have to use “instanceof” operator , but
also have to explicitly cast any objects returned from this list. The code is also less readable. The following code with generics is not only more readable,
but also throws a compile time error if you try to add an Integer object to list1 or a String object to list2.
Q35 . What are the dif ferences amongList list = new ArrayList ( );
ist.add(new Integer ( ));
ist.add(“A String ”);
ist.add(new Mango ( ));
List<String > list1 = new ArrayList <String >( );
List<Integer > list2 = new ArrayList <Integer >( );

– Raw or plain old collection type e.g. Collection
– Collection of unknown e.g. Collection<?>
– Collection of type object e.g. Collection<Object>
What is the super type for all the collection class?
A35 . 1) The plain old Collection : is a heterogeneous mixture or a mixed bag that contains elements of all types, for example Integer , String, Fruit, V egetable,
etc.
2) The Collection<object> : is also a heterogeneous mixture like the plain old Collection, but not the same and can be more restrictive than a plain old
Collection discussed above. It is incorrect to think of this as the super type for a collection of any object types.
Unlike an Object class, which is a “super type” for all objects like String , Integer , Fruit , etc, List<Object> is not a super type for List<String> , List<Integer> ,
List<Fruit> , etc. So it is illegal to do the following:
Though Integer is a subtype of Object , List<Integer> is not a subtype of List<Object> because List of Objects is a bigger set comprising of elements of various
types like Strings, Integers, Fruits, etc. A List of Integer should only contain Integers, hence the above line is illegal. If the above line was legal, then you can
end up adding objects of any type to the list, violating the purpose of generics.
3) The Collection<?> : is a homogenous collection that represents a family of generic instantiations of Collection like Collection<String> ,
Collection<Integer> , Collection<Fruit> , etc.
Collection<?> is the super type for all generic collection as Object[ ] is the super type for all arrays.List<object > list = new ArrayList <integer >( ); //illegal
List<?> list = new ArrayList <Integer >( ); //legal
List<? extends Number > list = new ArrayList <Integer >( ); //legal

Note : An Integer is a sub type of a Number .
Q36 . How will you go about deciding which of the following to use?
<Number> 
? extends <Number>
? super <Number>
A36 . Here is the guide:
1. Use the ? extends wildcard if you need to retrieve object from a data structure. That is read only . You can’ t add elements to the collection.
2. Use the ? super wildcard if you need to put objects in a data structure.
3. If you need to do both things (read and add elements), don’t use any wildcard .
Generics are covered in detail: 
1. Understanding Java generics with the help of scenarios .
2. Java generics interview questions & answers . 
3. Java coding question on recursion and generics .



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
