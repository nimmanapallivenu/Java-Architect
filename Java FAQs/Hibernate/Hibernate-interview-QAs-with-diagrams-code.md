# Hibernate interview Q&As with diagrams & code

## Table of Contents

- [Q9: Explain hibernate object states? Explain hibernate objects life cycle?](#q9)
- [Q10: What are the benefits of detached objects?](#q10)
- [Q11: How does Hibernate distinguish between transient (i.e. newly instantiated) and d](#q11)
- [Q12: How does hibernate support lazy loading?](#q12)
- [Q13: What do you understand by automatic dirty checking in Hibernate?](#q13)
- [Q14: What do you understand by the terms optimistic locking versus pessimistic lockin](#q14)
- [Q16: What are the general steps involved in creating Hibernate related artifacts?](#q16)
- [Q17: How would you define a hibernate domain object with table mappings, native named](#q17)

---

## Q9: Explain hibernate object states? Explain hibernate objects life cycle?

**Answer:**

There are 3 states.
1. Persistent objects and collections are short lived single threaded objects, which store the persistent state. These objects synchronize their state with
the database depending on your flush strategy (i.e. auto-flush where as soon as setXXX() method is called or an item is removed from a Set, List, etc or define
your own synchronization points with session.flush(), transaction.commit() calls). If you remove an item from a persistent collection like a Set, it will be
removed from the database either immediately or when flush() or commit() is called depending on your flush strategy . They are Plain Old Java Objects (POJOs)
and are currently associated with a session. As soon as the associated session is closed, persistent objects become detached objects and are free to be used
directly as data transfer objects in any application layers like business layer , presentation layer , etc.

Hibernate Objects Life Cycle

Note: In JP A 2.0, you use an EntityManager instead of a Session. So, you will use entityManager .persist(entity) to create(..), entityManager .merge(entity) to
edit(..), entityManager .remove(entity) to remove(..), and entityManager .find(entityClass, primaryKey) to find(..).
2. Detached objects and collections are instances of persistent objects that were associated with a session but currently not associated with a session.
These objects can be freely used as Data T ransfer Objects without having any impact on your database. Detached objects can be later on attached to another
session by calling methods like session.update(), session.saveOrUpdate() etc. and become persistent objects.
3. Transient objects and collections are instances of persistent objects that were never associated with a session. These objects can be freely used as
Data T ransfer Objects without having any impact on your database. T ransient objects become persistent objects when associated to a session by calling methods
like session.save( ), session.persist( ) etc.
Note : The states of transient and detached objects cannot be synchronized with the database as they are not managed by Hibernate.

---

## Q10: What are the benefits of detached objects?

**Answer:**

Pros: When long transactions are required due to user think-time, it is the best practice to break the long transaction up into two or more transactions. Y ou can
use detached objects from the first transaction to carry data all the way up to the presentation layer . These detached objects get modified outside a transaction
and later on re-attached to a new transaction via another session.
Cons:
– In general, working with detached objects is quite cumbersome, and it is better not to clutter up the session with them if possible. It is better to discard them
and re-fetch them on subsequent requests. This approach is not only more portable but also more ef ficient because the objects hang around in Hibernate’ s cache
anyway .
– Also from pure rich domain driven design perspective, it is recommended to use DT Os (DataT ransferObjects) and DOs (DomainObjects) to maintain the
separation between Service and UI tiers.

---

## Q11: How does Hibernate distinguish between transient (i.e. newly instantiated) and detached objects?

**Answer:**

1. Hibernate uses the “version” property , if there is one.
2. No identifier value means a new object. This does work only for Hibernate managed surrogate keys. Does not work for natural keys and assigned (i.e. not
managed by Hibernate) surrogate keys.
3. Write your own strategy with “Interceptor .isUnsaved( )”.
Note: When you reattach detached objects, you need to make sure that the dependent objects are reattached as well.

---

## Q12: How does hibernate support lazy loading?

**Answer:**

Hibernate uses a proxy object to support lazy loading . Basically as soon as you reference a child or lookup object via the accessor/getter methods, if the
linked entity is not in the session cache (i.e. the first-level cache), then the proxy code will go of f to the database and load the linked object. It uses javassist (or
CGLIB ) to ef fectively and dynamically generate sub-classed implementations of your objects.
Let’s look at an example. An employee hierarchy table can be represented as Java object hierarchy as shown below:
In the above example, if you use lazy loading then the “superior” and “subordinates” will be proxied (i.e. not the actual object, but the stub object that knows
how to load the actual object) when the main “Employee” object is loaded. So, if you need to get the “subordinates” or “superior” object, you invoke the getter
method on the employee like employee.getSuperior( ) and the actual object will be loaded.
Hibernate does require the same EntityManager to be available in order to lazily load objects. If you have no EntityManager , then you have no knowledge of the
datastore. Once the transaction is committed the objects become detached, and you can’ t lazy load detached objects. So, you need to lazily load your objects
within the same transaction in your service layer .

---

## Q13: What do you understand by automatic dirty checking in Hibernate?

**Answer:**

Dirty checking is a feature of hibernate that saves time and ef fort to update the database when states of objects are modified inside a transaction. All
persistent objects are monitored by hibernate. It detects which objects have been modified and then calls update statements on all updated objects.
Hibernate Session contains a PersistenceContext object that maintains a cache of all the objects read from the database as a Map. So, when you modify an object
within the same session, Hibernate compares the objects and triggers the updates when the session is flushed. The objects that are in the PersistenceContext are
pesistent objects.public class Employee {
 private Long id;
 private String name ;
 private String title;
 private Employee superior ; //parent
 private Set<Employee > subordinates ; //children
 //getters and setters are omitted

---

## Q14: What do you understand by the terms optimistic locking versus pessimistic locking?

**Answer:**

Optimistic locking means a specific record in the database table is open for all users/sessions. Optimistic locking uses a strategy where you read a
record, make a note of the version number and check that the version number hasn’ t changed before you write the record back. When you write the record back,
you filter the update on the version to make sure that it hasn’ t been updated between when you check the version and write the record to the disk. If the record is
dirty (i.e. dif ferent version to yours) you abort the transaction and the user can re-start it.
You could also use other strategies like checking the timestamp or all the modified fields (this is useful for legacy tables that don’ t have version number or
timestamp column). Note: The strategy to compare version numbers and timestamp will work well with detached hibernate objects as well. Hibernate will
automatically manage the version numbers.
In Hibernate, you can use either long number or Date for versioning
or
Pessimistic locking means a specific record in the database table is open for read/write only for that current session. The other session users can not edit
the same because you lock the record for your exclusive use until you have finished with it. It has much better integrity than optimistic locking, but requires you
to be careful with your application design to avoid deadlocks. In pessimistic locking, appropriate transaction isolation levels need to be set, so that the records
can be locked at dif ferent levels. The general isolation levels are
— Read uncommitted isolation
— Read committed isolation@Version
private long id;
@Version
private Date version ;

— Repeatable read isolation
— Serializable isolation
It can be dangerous to use “read uncommitted isolation” as it uses one transaction’ s uncommitted changes in a dif ferent transaction. The “Serializable isolation”
is used to protect phantom reads, phantom reads are not usually problematic, and this isolation level tends to scale very poorly . So, if you are using pessimistic
locking, then read committed and repeatable reads are the most common ones.

---

## Q16: What are the general steps involved in creating Hibernate related artifacts?

**Answer:**

The general steps involved in creating Hibernate related artifacts involve the following steps:
Step #1. Define the domain (aka entity) objects like Employee, Address, etc to represent relevant tables in the underlying database with the appropriate
annotations or using the *.hbm.xml mapping files.
Step #2. Define the Repository (aka DAO — Data Access Objects) interfaces and implementations classes that use the domain objects and the hibernate session
to perform data base CRUD (Create, Read, Update and Delete) operations the hibernate way .
Step #3. Define the service interfaces and the classes that make use of one or more repositories (aka DAOs) in a transactional context.A transaction manager
will be used to coordinate transactions (i.e. commit or rollback) between a number of repositories.
Step #4. Finally , use an IoC container like Spring framework to wire up the Hibernate classes like SessionFactory , Session, transaction manager , etc and the user
defined repositories, and the service classes. A number of interceptors can be wired up as well for deadlock retry , logging, auditing, etc using Spring.
Step #5. Favor using JP A and CrudRepository from Spring.

---

## Q17: How would you define a hibernate domain object with table mappings, native named queries, and custom data conversion using annotations?

**Answer:**

Firstly , define a parent domain object class for any common method implementations.

The entity class.public class MyAppDomainObject {
 
 //for example
 protected boolean isPropertyEqual (Object comparee , Object compareT oo) {
 if (comparee == null) {
 if (compareT oo != null) {
 return false ;
 }
 } else if (!comparee .equals (compareT oo)) {
 return false ;
 }
 return true;
 }
@Entity
@org.hibernate .annotations .Entity (selectBeforeUpdate = true)
@Table(name = "tbl_employee" )
@TypeDefs (value = { @TypeDef (name = "dec" , typeClass = DecimalUserT ype.class )}) // custom data type conversion
@NamedNativeQueries ({
 @NamedNativeQuery (name = "HighSalary" , query = "select * from tbl_employee where salary > :median_salary " , resultClass = Employee .class ),
@NamedNativeQuery (name = "LowSalary" , query = "select * from tbl_employee where salary < :median_salary " , resultClass = Employee .class )
)
public class Employee extends MyAppDomainObject implements Serializable {
 
 @Id
 @GeneratedV alue(strategy = GenerationT ype.AUT O)
 @Column (name = "employee_id" )
 private Long id;
 @Column (name = "emp_code" )
 private String accountCode ;

Hibernate repository class that makes use of the Employee domain object. Firstly define the interface. @Column (name = "manager_code" )
 private String adviserCode ;
 @Column (name = "type" )
 @Enumerated (EnumT ype.STRING )
 private EmployeeT ype type = EmployeeT ype.PERMANENT ;
 @Type(type = "dec" )
 @Column (name = "base_salary" )
 private Decimal salary = Decimal .ZERO ;
 @Transient
 private Decimal salaryW ithBonus ; //not persisted to database
 @Formula ("base_salary*2" )
 private Decimal doubleSalary ; //derived or calculated read only property
 @Formula ("(select base_salary where type = 'Permanent' )" )
 private Decimal permanantLeaveLoading ; //derived or calculated read only property
 @OneT oOne (cascade = { CascadeT ype.REFRESH })
 @JoinColumn (name = "emp_code" , insertable = false , updatable = false )
 private EmployeeExtrInfo extraInfo ;
 @ManyT oOne (cascade = { CascadeT ype.REFRESH })
 @JoinColumn (name = "manager_code" , insertable = false , updatable = false )
 private Manager manager ;
 @OneT oMany (cascade = { ALL , MERGE , PERSIST , REFRESH }, fetch = FetchT ype.LAZY )
 @JoinColumn (name = "emp_code" , nullable = false )
 @Cascade ({ org.hibernate .annotations .CascadeT ype.SAVE_UPDA TE, org.hibernate .annotations .CascadeT ype.DELETE_ORPHAN })
 private List<PaymentDetail > paymentDetails = new ArrayList <PaymentDetail >();
 //getters and setters omitted for brevity
 //equals and hashcode methods

Define the implementationmport java.util.List;
public interface EmployeeT ableRepository {
Employee saveEmployee (Employee employee ) throws RepositoryException ;
Employee loadEmployee (Long employeeId ) throws RepositoryException ;
List<Employee > findAllEmployeesW ithHighSalary (BigDecimal medianSalary ) throws RepositoryException ;
List<Employee > findAllEmployeesW ithLowSalary (BigDecimal medianSalary ) throws RepositoryException 
}
@SuppressW arnings ("unchecked" )
public class EmployeeT ableHibernateRepository extends HibernateDaoSupport implements EmployeeT ableRepository {
 public EmployeeT ableHibernateRepository (HibernateT emplate hibernateT emplate ) {
 setHibernateT emplate (hibernateT emplate );
 }
 //The employee objects gets constructed and passed to repo via the Business Service layer
 public Employee saveEmployee (Employee employee ) throws RepositoryException {
 Session session = getHibernateT emplate ().getSessionFactory ().getCurrentSession ();
 session .saveOrUpdate (employee );
 session .flush ();
 session .evict (employee );
 return this.loadEmployee (employee .getId ());
 }
 public Employee loadEmployee (Long employeeId ) throws RepositoryException {
 Session session = getHibernateT emplate ().getSessionFactory ().getCurrentSession ();
 Criteria crit = session .createCriteria (Employee .class );
 crit.add(Restrictions .eq("id",employeeId ));
 List<Employee > employees = crit.list();
 if (employees .size() == 1) {
 return employees .get(0);
 }

The service layer that uses the repository layer //this is a custom exception class
 throw new RepositoryException ("Found more than one or no employee with Id:" + employeeId );
 }
 public List<Employee > findAllEmployeesW ithHighSalary (BigDecimal medianSalary ) throws RepositoryException {
 Session session = getHibernateT emplate ().getSessionFactory ().getCurrentSession ();
 Query query = session .getNamedQuery ("HighSalary" ); // query name defined in Employee class
 query .setBigDecimal (":median_salary" , medianSalary ); // query parameter defined in Employee class
 return (List<Employee >) query .list();
 }
 public List<Employee > findAllEmployeesW ithLowSalary (BigDecimal medianSalary ) throws RepositoryException {
 Session session = getHibernateT emplate ().getSessionFactory ().getCurrentSession ();
 Query query = session .getNamedQuery ("LowSalary" ); // query name defined in Employee class
 query .setBigDecimal (":median_salary" , medianSalary ); // query parameter defined in Employee class
 return (List<Employee >) query .list();
 }
 //other methods can be defined here
mport org.springframework .transaction .PlatformT ransactionManager ;
mport org.springframework .transaction .TransactionDefinition ;
mport org.springframework .transaction .TransactionStatus ;
mport org.springframework .transaction .support .DefaultT ransactionDefinition ;
/....other imports
public class EmployeeServiceImpl implements EmployeeService {
 private final EmployeeT ableRepository employeeRepository ;
 private PlatformT ransactionManager transactionManager ;
 
 public EmployeeServiceImpl (EmployeeT ableRepository employeeRepository , PlatformT ransactionManager transactionManager ) {

More Hibernate Interview Questions & Answers
1. Hibernate mistakes
2. Hibernate cache First & second level interview questions and answers
3. Hibernate automatic dirty checking
4. Hibernate entities with auditable, soft delete & optimistic locking fields
5. Understanding Hibernate proxy objects
6. Hibernate custom data type
7. 8 JPA interview questions and answers this.employeeRepository = employeeRepository ;
 this.transactionManager = transactionManager ;
 }
 
 public Employee loadEmployee (Long employeeId ) throws RepositoryException {
 return this.employeeRepository .loadEmployee (employeeId );
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
