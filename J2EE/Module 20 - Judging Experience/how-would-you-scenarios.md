# How Would You... Scenarios

> **Module**: Judging Experience  
> **Topic**: How Would You... Scenarios

---

## 📋 Table of Contents



- [Q1: How would you go about judging the code quality of other developers?](#q1)
- [Q2: How would you go about documenting your Java/JEE application?](#q2)
- [Q3: How would you go about designing a Java/JEE application?](#q3)

---

## Q1: How would you go about judging the code quality of other developers?

**Answer:**

1) Code written with unit tests and progressively re-factored where necessary to make it more maintainable, testable, and readable.
2) Unit tests need to be written properly — mock objects using frameworks like Mockito and Power Mock, independent tests that don’ t depend on other tests,
monitor code coverage with tools like Sonar, and apply “ Given (arg1 and ar g2)-Then (validate calculated result )” style. BDD uses Given[Context]-
When[Event Occurs]-Then[Outcome]. This is derived from the agile stories in the form of As a [Role] – I want [Feature] – So that I receive [V alue].
3) Integration testing of your services with JUnit or T estNG. Y our integration tests are only as good as the quality of the data. Y ou could either use dedicated test
databases or use frameworks like DBUnit to manage extraction and insertion of data.
4) Code that takes into consideration dif ferent input values. For example, negative numbers, positive numbers, and zero value in an algorithm requiring to
calculate two numbers in a series of number that add up to be greater than 100.
5) Code fails fast by validating the input parameters for null, etc. In other words, apply the principle of “ design by contract ” by checking for pre-conditions
and post-conditions .
6) Code that uses proper OO design concepts and design patterns. Unsightly if/else if clauses need to be replaced with “Open Closed Principle (OCP)”. Favor
composition over inheritance. Strive to write code with Strong encapsulation, high cohesion, and low coupling .
7) Code that is written with non functional areas like concurrent access (i.e multi-threading), security, performance, memory usage and potential GC and
memory leak concerns, scalability, maintainability, etc in mind.
8) Follow design principles DR Y (Don’ t Repeat Y ourself), KISS (Keep It Simple and Stupid) and Y AGNI(Y ou Aint Going to Need It).
9) W eb testing using Selenium + W ebDriver. Selenium + W ebDriver ( Selenium interview questions and answers) allows you to reenact web user experience
and run it as an automated unit test using JUnit or T estNG.
10) Load testing your application with tools like JMeter. The Badboy functional testing tool compliments JMeter by allowing you to record scripts and then
export the scripts as a JMeter file to be used in JMeter .
Here are some finer points:
Naming conventions.
Existence of unused code and commenting code out. Delete unused code. Source control is there for maintaining the history of your code.
Unnecessary comments. The method and variable names must be self explanatory without cluttering the code with excessive comments. The methods
should do only one thing with fewer lines of code. More than 15-20 lines of code is questionable. The number of parameters passed in must also be small.

The public methods must fail fast with proper validations.
Repeated code due to copy-paste. For example, same logic or hard coded values repeated in a number of places.
Favoring trivial performance optimization over readability and maintainability. 
Tightly copuled code. For example, not coding to interfaces, favoring inheritance over composition, etc.
Badly defined variable scopes or variable types. For example, using a data type double to represent monetary values instead of BigDecimal. The variable
scopes must be as narrow as possible.
Using mutable objects and read only varaibles where immutable objects make more sense.
Proper implementation of language contracts. For example, not properly implemented equals and hashCode methods.
Deeply nested loops or conditionals. Nested loops can be replaced with changing the logic or through recursion. Nested if-else conditionals are a good
candidate for applying polymorphism.
Not properly handling exceptions. For example, burying exceptions, exposing internal exception details to the users without proper friendly messages at
the W eb layer, etc.
Badly written unit tests.
Not designing the classes and interfaces with proper design concepts and principles. For example, strongly coupled classes, classes trying to do more
things than it should, modelling it as a class when it should be an attribute, etc.
Not handling and testing non-functional scenarios. For example, not properly handling service timeouts or using incorrect timeout values.
Reinventing the wheel by writing your own implementation, when there is already a proven and tested implementation provided by the API.

---

## Q2: How would you go about documenting your Java/JEE application?

**Answer:**

Before embarking on coding get the business r equir ements down. Build a complete list of requested features, sample screen shots (if available), use case
diagrams, business rules etc as a functional specification document. This is the phase where business analysts and developers will be asking questions about
user interface requirements, data tier integration requirements, use cases etc. Also prioritize the features based on the business goals, lead-times and iterations
required for implementation.
— Prepare a technical specification document based on the functional specification. The technical specification document should cover:
Purpose of the document: e.g. This document will emphasize the customer service functionality .
Overview: This section basically covers background information, scope, any inclusions and/or exclusions, referenced documents etc.
Baseline ar chitectur e: discusses or references baseline architecture document. Answers questions like W ill it scale? Can this performance be improved?
Is it extendable and/or maintainable? Are there any security issues? Describe the vertical slices to be used in the early iterations, and the concepts to be
proved by each slice.

Assumptions, Dependencies, Risks and Issues: highlight all the assumptions, dependencies, risks and issues. For example list all the risks you can
identify .
Design alternatives for each key functional requirement. Also discuss why a particular design alternative was chosen over the others. This process will
encourage developers analyze the possible design alternatives without having to jump at the obvious solution, which might not always be the best one.
Processing logic: discuss the processing logic for the client tier, middle tier and the data tier. Where required add process flow diagrams. Add any pre-
process conditions and/or post-process conditions.
UML diagrams to communicate the design to the fellow developers, solution designers, architects etc. Usually class diagrams and sequence diagrams
are required. The other diagrams may be added for any special cases l
— Prepar e a coding standards document for the whole team to promote consistency and ef ficiency .
— Prepar e a code r eview document and templates for the whole team.
— Prepar e additional optional guideline documents as per requirements to be shared by the team. This will promote consistency and standards. For example:
How to set up the environment? How to set up the application? SDLC document, data modelling guidelines, ER(Entity Relationship) diagrams, etc.

---

## Q3: How would you go about designing a Java/JEE application?

**Answer:**

Design should be specific to a problem but also should be general enough to address future requirements. Designing reusable object oriented software
involves decomposing the business use cases into relevant objects and converting objects into classes.
— Create a tier ed ar chitectur e: client tier, business tier and data tier. Each tier can be further logically divided into layers.
— Create a data model: A data model is a detailed specification of data oriented structures. This is dif ferent from the class modeling because it focuses solely
on data whereas class models allow you to define both data and behavior. Conceptual data models (aka domain models) are used to explore domain concepts
with project stakeholders. Logical data models are used to explore the domain concepts, and their relationships. Logical data models depict entity types, data
attributes and entity relationships (with Entity Relationship (ER) diagrams). Physical data models are used to design the internal schema of a database
depicting the tables, columns, and the relationships between the tables. Data models can be created by performing the following tasks:
Identify entity types, attributes and relationships: use entity relationship (E-R) diagrams.
Apply naming conventions (e.g. for tables, attributes, indices, constraints etc): Y our or ganization should have standards and guidelines applicable to data
modeling.
Assign keys: surrogate keys (e.g. assigned by the database like Oracle sequences etc, max()+1, universally unique identifiers UUIDs, etc), natural keys
(e.g. T ax File Numbers, Social Security Numbers etc), and composite keys.
Normalize to reduce data redundancy and denormalize to improve performance: Normalized data have the advantage of information being stored in one
place only, reducing the possibility of inconsistent data. Furthermore, highly normalized data are loosely coupled. But normalization comes at a
performance cost because to determine a piece of information you have to join multiple tables whereas in a denormalized approach the same piece of

information can be retrieved from a single row of a table. Denormalization should be used only when performance testing shows that you need to improve
database access time for some of your tables.
— Create a design model: A design model is a detailed specification of the objects and relationships between the objects as well as their behavior .
Class diagram: contains the implementation view of the entities in the design model. The design model also contains core business classes and non-core
business classes like persistent storage, security management, utility classes etc. The class diagrams also describe the structural relationships between the
objects.
Use case r ealizations: are described in sequence and collaboration diagrams.
— Design considerations when decomposing business use cases into relevant classes: designing reusable and flexible design models requires the following
considerations:
Granularity of the objects (fine-grained, coarse-grained etc): Can we minimize the network trip by passing a coarse-grained value object instead of
making 4 network trips with fine-grained parameters? Should we use method level (coarse-grained) or code level (fine-grained) thread synchronization?
Should we use a page level access security or a fine-grained programmatic security?
Coupling between objects (loosely coupled versus strongly coupled). Should we use business delegate pattern to loosely couple client and business tier?
or Should we use dependency injection?
Definition of class interfaces and inheritance hierar chy: Should we use an abstract class or an interface? Is there any common functionality that we can
move to the super class (or parent class)? Should we use interface inheritance with object composition for code reuse as opposed to implementation
inheritance?
Establishing key r elationships (aggregation, composition, association etc): Should we use aggregation or composition? [composition may require
cascade delete], Should we use an “is a” (generalization) relationship or a “has a” (composition) relationship?
Applying polymorphism and encapsulation: Should we hide the member variables to improve integrity and security? Can we get a polymorphic
behavior so that we can easily add new classes in the future?
Applying well-pr oven design patterns (like Gang of four design patterns, JEE design patterns, EJB design patterns, Enterprise Integration Patterns
(EIP), etc) help designers to base new designs on prior experience. Design patterns also help you to analyze design alternatives
Scalability of the system: Vertical scaling is achieved by increasing the number of servers running on a single machine. Horizontal scaling is achieved by
increasing the number of machines in the cluster. Horizontal scaling is more reliable than the vertical scaling because there are multiple machines
involved in the cluster. In vertical scaling the number of server instances that can be run on one machine are determined by the CPU usage and the JVM
heap memory .
How do we replicate the session state? Should we use stateful session beans or HTTP session? Should we serialize this object so that it can be replicated?
Can we go stateless?
Internationalization r equir ements for multi-language support: Should we support other languages? Should we support multi-byte characters in the
database?

— Vertical slicing: Getting the reusable and flexible design the first time is impossible. By developing the initial vertical slice of your design you eliminate any
nasty integration issues later in your project. Also get the design patterns right early on by building the vertical slice. It will give you experience with what does
work and what does not work with Java/JEE. Once you are happy with the initial vertical slice then you can apply it across the application. The initial vertical
slice should be based on a typical business use case.
— Ensure the system is configurable through property files, xml descriptor files, annotations etc. This will improve flexibility and maintainability. Avoid hard
coding any values. Use a constant class for values, which rarely change and use property files, xml descriptor files, annotations etc for values, which can change
more frequently (e.g. process flow steps etc) and/or environment related configurations(e.g. server name, server port, LDAP server location etc).
— Design considerations during design, development and deployment phases: designing a fast, secured, reliable, robust, reusable and flexible system require
considerations in the following key areas:
Performance issues (network overheads, quality of the code etc): Can I make a single coarse-grained network call to my remote object instead of 3 fine-
grained calls?
Concurrency issues (multi-threading etc): What if two threads access my object simultaneously will it corrupt the state of my object?
Transactional issues (ACID properties): What if two clients access the same data simultaneously? What if one part of the transaction fails, do we
rollback the whole transaction? What if the client resubmits the same transactional page again?
Security issues: Are there any potential security holes for SQL injection or URL injection by hackers?
Memory issues: Is there any potential memory leak problems? Have we allocated enough heap size for the JVM? Have we got enough perm space
allocated since we are using 3rd party libraries, which generate classes dynamically? (e.g. JAXB, XSL T, JasperReports etc)
Scalability issues: Will this application scale vertically and horizontally if the load increases? Should this object be serializable? Does this object get
stored in the HttpSession?
Maintainability, reuse, extensibility etc: How can we make the software reusable, maintainable and extensible? What design patterns can we use? How
often do we have to refactor our code?
Logging and auditing if something goes wrong can we look at the logs to determine the root cause of the problem? Splunk?
Object life cycles: Can the objects within the server be created, destroyed, activated or passivated depending on the memory usage on the server? (e.g.
EJB).
Resour ce pooling: Creating and destroying valuable resources like database connections, threads etc can be expensive. So if a client is not using a
resource can it be returned to a pool to be reused when other clients connect? What is the optimum pool size?
Caching can we save network trips by storing the data in the server ’s memory? How often do we have to clear the cache to prevent the in memory data
from becoming stale?
Load balancing: Can we redirect the users to a server with the lightest load if the other server is overloaded?
Transpar ent fail over: If one server crashes can the clients be routed to another server without any interruptions?

Clustering: What if the server maintains a state when it crashes? Is this state replicated across the other servers?
Back-end integration: How do we connect to the databases and/or legacy systems?
Clean shutdown: Can we shut down the server without af fecting the clients who are currently using the system?
Systems monitoring: In the event of a catastrophic system failure who is monitoring the system? Any alerts or alarms? Should we use JMX? Should we
use any performance monitoring tools like T ivoli etc?
Dynamic r edeployment: How do we perform the software deployment while the site is running? (Mainly for mission critical applications 24hrs X
7days).
Portability: Can I port this application to a dif ferent server 2 years from now?
Archival and data r etention .
Disaster r ecovery .
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03