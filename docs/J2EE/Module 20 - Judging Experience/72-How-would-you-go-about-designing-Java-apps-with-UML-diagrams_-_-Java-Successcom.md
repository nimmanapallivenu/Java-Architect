# 72 How would you go about designing Java apps with UML diagrams    Java Success.com

## Table of Contents

- [Q1: How would you go about designing a system as described below?
A barn that contai](#q1)
- [Q2: How would you go about designing a car parking station?](#q2)

---

## Q1: How would you go about designing a system as described below?
A barn that contains animals such as cows and horses. A farmer milks the cows in the farm, and the animals eat hay that are stored in the barn. The barn is
constructed with wooden planks.

**Answer:**

“is a” relationships
A Cow is an animal, and so is a Horse.
“has a ” relationships
Zero to many (i.e. 0..*) animals live in a Barn. The Barn is also made of at least 1 and probably many (i.e. 1..*) W oodenPlanks. The Barn also stores the Hay for
the animals.
“uses” relationship(s)
A farmer interacts with many (i.e. 0..*) Cows by sending messages.
Conceptualize your thoughts with a UML diagram as shown below .

UML Diagram

The above class diagram was generated using the open source tool ar goUML. In the above design, the animals are tightly coupled to the barn. The above design
can be made more flexible to handle other farm animals like hen, etc as shown below . The design below is better because of lower coupling between the animals
and the enclosures like Barn, HenHouse, etc. Similar changes can be applied between the Enclosure and the W oodenPlank so that the enclosures can be made
with other materials like Mesh, etc. While designing, you can also think of the design principles discussed above and patterns like factory pattern or dependency
injection to promote looser coupling between entities.

UML Diagram

---

## Q2: How would you go about designing a car parking station?

**Answer:**

Map out the r equir ements:
The car park needs to cater for dif ferent types of car parks like regular , handicapped, and compact.
It should keep track of empty and filled spaces.
It should also cater for valet parking.
Map out the classes that would be required. Use a UML class diagram. Here are some points to get started. Decide as to apply inheritance (i.e. is a), composition
(i.e. has a), or delegation. Favor composition over inheritance.
A CarPark class to represent a parking station.
A ParkingSpace can be an abstract class or an interface to represent a parking space, and RegularParkingSpace, HandicappedParkingSpace,
CompactParkingSpace, etc are subtypes of a ParkingSpace. This means a RegularParkingSpace is a ParkingSpace.
A CarPark has a (i.e. composition) finite number of ParkingSpaces. A CarPark also keeps track of all the parking spaces and a separate list of all the
vacant parking spaces.
A Vehicle class uses a (i.e. delegation) ParkingSpace. The V ehicle class will hold attributes using enum classes like VehicleT ype and ParkingT ype. The vehicle
types could be Compact, Regular , and Handicapped. The parking types could be Self or V alet. Depending on the requirements, the self or valet types could be
designed as subtypes of the V ehicle class.
Note : Care must be taken to have a right balance between over engineering and not engineering at all. All depends on requirements and which parts of the
design are most likely to grow in the future phases. The only way to get better at designing is with experience and keeping all the design concepts, principles,
and patterns in mind to design a loosely coupled system that is simple, easy to understand, flexible and maintainable.

UML Refresher

UML Diagram
In UML terms, association relationship denotes that two classes are connected with each other . A navigability arrow on an association shows which direction
the association can be traversed or queried. For example, a Dog can be queried about its AnimalHelper or traversed to the AnimalHelper . The multiplicity of an
association end is the number of possible instances of the class associated with a single instance of the other end. In the above example, there can be only one
AnimalHelper for each Dog.
Association example,
Aggregation and composition are two types of associations. An aggregation is a weaker relationship as shown below where the life cycle of a Product is not
controlled by the LineItem. If a line item is deleted, the associated product does not have to be deleted as well. This product can be used by other line items in
other orders./ A Message has an association with a Person 
public class Message { 
 private Person recipient ; 
 private Person sender ; 
 private String text; 
 public Message (Person recipient , Person sender , String text) { 
 this.recipient = recipient ; 
 this.sender = sender ; 
 this.text = text; 
 } 

Composition is a stronger relationship where the life cycle of the composed class is managed by the composing class as shown below . If an order is deleted, the
line item needs to be deleted as well.
Conceptually an animal is composed of legs, head, heart, etc and aggregated (or preferably use associated) with location (i.e. where they live?), food, breeder ,
owner , etc.
Have you completed this unit? Then mark this unit as completed./ A LineItem is an aggregate of Product & Quantity
public class LineItem { 
 private Product product ; 
 private Quantity qty;
 
 public LineItem (Product product , Quantity qty) { 
 this.product = product ; // product & qty are created outside this
 this.qty = qty; //constructor and passed in.
 } 
 //...
 
/An Order is composed of a LineItem and controls life cycle of the LineItem
public class Order { 
 private LineItem lineItem ; 
 public Order ( ) { 
 this.lineItem = new LineItem (...); //lineItem is built in the enclosing
 //object's (i.e. Order's) constructor .
 
 }
 //.... 

 Mark as Completed
« Previous Unit

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
