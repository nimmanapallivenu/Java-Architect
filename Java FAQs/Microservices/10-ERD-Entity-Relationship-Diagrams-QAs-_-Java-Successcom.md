# 10 ERD (Entity Relationship Diagrams) Q&As   Java Success.com

## Table of Contents

- [Q1: What is an ERD?](#q1)
- [Q2: What are the dif ferences between Logical and Physical ERD models?](#q2)
- [Q3: What are the various relationships among dif ferent entities?](#q3)
- [Q4: What is a weak entity?](#q4)
- [Q5: What do the rectangles divided into 2 parts represent in an ERD diagram?](#q5)
- [Q6: Which of the following indicates the maximum number of entities that can be invo](#q6)
- [Q7: In a one-to-many relationship, the entity that is on the one side of the relatio](#q7)
- [Q8: What is a recursive relationship?
a) A relationship between itself
b) A relation](#q8)
- [Q9: Which of the following refers to an entity in which the identifier of one entity](#q9)
- [Q10: What are super and sub type entities?](#q10)

---

## Q1: What is an ERD?

**Answer:**

ERD or Entity-Relationship Diagram, is a chart that visually represents the relationship between database entities . ERDs model an or ganization’ s data
storage requirements with three main components: entities, attributes, and relationships. As UML is for charting the relationships and interactions among Java
entities like classes and interfaces, ERD is for relationships among database tables.

ERD (Entity-Relationship-Diagram)
ERD Symbols

---

## Q2: What are the dif ferences between Logical and Physical ERD models?

**Answer:**

The above diagram is a physical ERD.
— Logical and Physical ERD models are used in dif ferent stages of development, but they are inter -related.
— Logical ERD models are defined around the business’ s needs.
— Physical ERD represents the actual design of database. It deals with converting from logical design into a schema level design that will be conducive to
creating a relational database.
— In Physical ERD models a) relationships need to be resolved by introducing additional link table for a many to many relationships b) audit-able fields like
CREA TED_BY , CREA TED_TIMEST AMP , UPDA TED_BY , UPDA TED_TIMEST AMP , etc c) Constraints like Primary Key , Foreign Key , etc are defined.
— There are tools to translate Logical ERD to Physical ERD, and then to generate DDLs (Data Definition Languages) to generate the actual table schemas.

---

## Q3: What are the various relationships among dif ferent entities?

**Answer:**

ERD Relationships

ERD Symbols
This one to one, one to many , etc are referred to as cardinality of the relationship.

---

## Q4: What is a weak entity?

**Answer:**

A weak entity is an entity that depends on the existence of another entity . For example, an entity like OrderItem is meaningless without an Order . In other
words, an OrderItem depends on the existence of an order .

---

## Q5: What do the rectangles divided into 2 parts represent in an ERD diagram?

**Answer:**

The top part of the rectangle, contains the name of the entity . The bottom part contains the names of all the attributes of the entity .

---

## Q6: Which of the following indicates the maximum number of entities that can be involved in a relationship?
a) Maximum cardinality
b) Minimum cardinality
c) Maximum entity count
d) Minimum entity count

**Answer:**

a. Maximum cardinality

---

## Q7: In a one-to-many relationship, the entity that is on the one side of the relationship is called a ______________ entity .
a) child
b) parent
c) single
d) many

**Answer:**

b. parent

---

## Q8: What is a recursive relationship?
a) A relationship between itself
b) A relationship between subtypes
c) A relationship between associated entities
d) A relationship between parent and children

**Answer:**

a. A relationship between itself

---

## Q9: Which of the following refers to an entity in which the identifier of one entity includes the identifier of another entity?
a) ID dependent entity
b) Strong entity
c) Weak entity
d) ID independent entity .

**Answer:**

a.ID dependent entity

---

## Q10: What are super and sub type entities?

**Answer:**

A super type entity is related to two or more associated entities that each contain specialized attributes that apply to some but not all of the instances of
the sub type entities.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
