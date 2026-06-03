# JNDI & LDAP

> **Module**: JMS JNDI JMX  
> **Topic**: JNDI & LDAP

---

## 📋 Table of Contents



- [Q1: What is JNDI? And what are the typical uses within a JEE application?](#q1)
- [Q2: What resources can you look up via a JNDI tree?](#q2)
- [Q3: How does a JNDI to File system, and database?](#q3)
- [Q4: What is a JNDI InitialContext?](#q4)
- [Q5: What is an LDAP server? And what is it used for in an enterprise environment?](#q5)
- [Q6: Why use LDAP when you can do the same with relational database (RDBMS)?](#q6)
- [Q7: What are key LDAP terms?](#q7)
- [Q8: So where does JNDI fit into this LDAP?](#q8)
- [Q9: What are the portable JNDI namespaces in JEE6?](#q9)

---

## Q1: What is JNDI? And what are the typical uses within a JEE application?

**Answer:**

JNDI stands for Java Naming and Directory Interface. It provides a generic interface to LDAP (Lightweight Directory Access Protocol) and other
directory services like NDS (Novell Directory Service), DNS (Domain Name Service) etc. It provides a means for an application to locate components that exist
in a name space according to certain attributes. A JEE application component uses JNDI interfaces to look up and reference system-provided and user -defined
objects in a component environment. JNDI is not specific to a particular naming or directory service. It can be used to access many dif ferent kinds of systems
including file systems.
JNDI and LDAP

---

## Q2: What resources can you look up via a JNDI tree?

**Answer:**

The JNDI API enables applications to look up objects such as DataSour ces, EJBs, MailSessions, JMS connection factories and destinations
(Topics/Queues) by name. These Objects can be loaded into a JNDI tree using a JEE application server ’s administration console. T o load an object in a JNDI
tree, choose a name under which you want the object to appear in a JNDI tree. The JEE deployment descriptors indicate the placement of JEE components in a
JNDI tree.

LDAP T ree

---

## Q3: How does a JNDI to File system, and database?

**Answer:**

JNDI is like a file system or a Database.

File System, JNDI, and Database comparison

---

## Q4: What is a JNDI InitialContext?

**Answer:**

All naming operations are relative to a context. The InitalContext implements the Context interface and provides an entry point for the resolution of names.
For example,
Map env = new HashMap (); 
env.put(Context .INITIAL_CONTEXT_F ACT ORY,"com.ibm.websphere.naming.WsnInitialContextFactory" );
env.put(Context .PROVIDER_URL, " iiop://localhost:1050" ); 
Context ctx = new InitialContext (env);

---

## Q5: What is an LDAP server? And what is it used for in an enterprise environment?

**Answer:**

LDAP stands for Lightweight Directory Access Protocol. This is an extensible open network protocol standard that provides access to distributed directory
services. LDAP is an Internet standard for directory services that run on TCP/IP. Under OpenLDAP and related servers, there are two servers – slapd, the LDAP
daemon where the queries are sent to and slurpd, the replication daemon where data from one server is pushed to one or more slave servers. By having multiple
servers hosting the same data, you can increase reliability, scalability, and availability .
It defines the operations one may perform like search, add, delete, modify, change name It defines how operations and data are conveyed.
LDAP has the potential to consolidate all the existing application specific information like user, company phone and e-mail lists. This means that the change
made on an LDAP server will take ef fect on every directory service based application that uses this piece of user information. The variety of information about a
new user can be added through a single interface which will be made available to Unix account, NT account, e-mail server, Web Server, Job specific news
groups etc. When the user leaves his account can be disabled to all the services in a single operation.
So, LDAP is most useful to provide “white pages” (e.g. names, phone numbers, roles etc) and “yellow pages” (e.g. location of printers, application servers etc)
like services. T ypically in a JEE application environment it will be used to authenticate and authorise users.

---

## Q6: Why use LDAP when you can do the same with relational database (RDBMS)?

**Answer:**

In general LDAP servers and RDBMS are designed to provide dif ferent types of services. LDAP is an open standard access mechanism, so an RDBMS can
talk LDAP. However the servers, which are built on LDAP, are optimized for read access so likely to be much faster than RDBMS in pr oviding r ead access .
So in a nutshell, LDAP is more useful when the information is often searched but rarely modified. (Another difference is that RDBMS systems store
information in rows of tables whereas LDAP uses object oriented hierarchies of entries).

---

## Q7: What are key LDAP terms?

**Answer:**

DIT: Directory Information T ree. Hierarchical structure of entries, those make up a directory .
DN: Distinguished Name. This uniquely identifies an entry in the directory. A DN is made up of relative DNs of the entry and each of entry’ s parent entries up
to the root of the tree. DN is read from right to left and commas separate these names. For example ‘cn=Peter Smith, o=ACME, c=AUS’.
objectClass: An objectClass is a formal definition of a specific kind of objects that can be stored in the directory. An ObjectClass is a distinct, named set of
attributes that represent something concrete such as a user, a computer, or an application.
LDAP URL: This is a string that specifies the location of an LDAP resource. An LDAP URL consists of a server host and a port, search scope, baseDN, filter ,
attributes and extensions. Refer to diagram below:

LDAP structure
So the complete distinguished name for bottom left entry (ie Peter Smith) is cn=Peter Smith, o=ACME, c=AUS. Each entry must have at least one attribute that
is used to name the entry. To manage the part of the LDAP directory we should specify the highest level parent distinguished names in the server configuration.
These distinguished names are called suf fixes. The server can access all the objects that are below the specified suf fix in the hierarchy. For example in the above
diagram, to answer queries about ‘Peter Smith’ the server should have the suf fix of ‘o=ACME, c=AUS’. So we can look for “Peter Smith” by using the
following distinguished name:
LDAP schema: defines rules that specify the types of objects that a directory may contain and the required optional attributes that entries of dif ferent types
should have./where o=ACME, c=AUS is the suf fix
cn=Peter Smith, o=ACME, c=AUS

Filters: In LDAP the basic way to retrieve data is done with filters. There is a wide variety of operators that can be used as follows: & (and), | (or), ! (not), ~=
(approx equal), >= (greater than or equal), <= (less than or equal), * (any) etc.

---

## Q8: So where does JNDI fit into this LDAP?

**Answer:**

JNDI provides a standard API for interacting with naming and directory services using a service provider interface (SPI), which is analogous to JDBC
driver. To connect to an LDAP server, you must obtain a reference to an object that implements the DirContext. In most applications, this is done by using an
InitialDirContext object that takes a Hashtable as an argument:

---

## Q9: What are the portable JNDI namespaces in JEE6?

**Answer:**

Three JNDI namespaces are used for portable JNDI lookups: java:global, java:module, and java:app .& (uid=a*) (uid=*l) )
Hashtable env = new Hashtable ();
env.put(Context .INITIAL_CONTEXT_F ACT ORY, “com.sun.jndi.ldap.LdapCtxFactory ”); 
env.put(Context .PROVIDER_URL, “ldap://localhost:387”);
env.put(Context .SECURITY_AUTHENTICA TION, “simple ”);
env.put(Context .SECURITY_PRINCIP AL, “cn=Directory Manager ”);
env.put(Context .SECURITY_CREDENTIALS, “myPassword ”);
DirContext ctx = new InitialDirContext (env);
/finding remote enterprise beans using JNDI lookups.
ava:global [/application name ]/module name /enterprise bean name [/interface name ]

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »/finding local enterprise beans within the same module.
ava:module /enterprise bean name /[interface name ]
/finding local enterprise beans packaged within the same application.
ava:app[/module name ]/enterprise bean name [/interface name ]

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03