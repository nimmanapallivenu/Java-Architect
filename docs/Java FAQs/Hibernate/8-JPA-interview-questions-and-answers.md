# 8 JPA interview questions and answers

## Table of Contents

- [Q1: What is a JP A? What are its key components?](#q1)
- [Q2: What is the dif ference between hibernate.cfg.xml and persistence.xml?](#q2)
- [Q3: What are the 3 artifacts required to implement a JP A compliant project?](#q3)
- [Q4: What is an EntityManagerFactory and a Persistence unit?](#q4)
- [Q5: What is an EntityManager?](#q5)
- [Q6: What is an Entity?](#q6)
- [Q7: What are the dependency jars required for a JP A application?](#q7)
- [Q9: What steps are required for Spring CrudRepository to work with JP A?](#q9)

---

## Q1: What is a JP A? What are its key components?

**Answer:**

The process of mapping Java objects to database tables and vice versa is called “Object-relational mapping” (ORM). The Java Persistence API provides
Java developers with an object/relational mapping (ORM) facility for managing relational data in Java applications. JP A is a specification and several
implementations are available like EJB, JDO, Hibernate, and T oplink. Using JP A and relevant implementation like Hibernate, developers can map, store, update
and retrieve data from relational databases to Java objects and vice versa.

JPA

---

## Q2: What is the dif ference between hibernate.cfg.xml and persistence.xml?

**Answer:**

If you are using Hibernate’ s proprietary API, you’ll need the hibernate.cfg.xml. If you are using JP A i.e. Hibernate EntityManager , you’ll need the
persistence.xml . You will not need both as you will be using either Hibernate proprietary API or JP A. However , if you had used Hibernate Proprietary API

using hibernate.cfg.xml with hbm.xml mapping files, and now wanted to start using JP A, you can reuse the existing configuration files by referencing the
hibernate.cfg.xml in the persistence.xml in the hibernate.ejb.cfgfile property and reuse the existing hbm.xml files. In a long run, migrate hbm.xml files to JP A
annotations.

---

## Q3: What are the 3 artifacts required to implement a JP A compliant project?

**Answer:**

1. An entity class
2. A persistence.xml file
3. An interface which you will use to perform CRUD operations like insert, update, or find an entity

---

## Q4: What is an EntityManagerFactory and a Persistence unit?

**Answer:**

The EntityManager is created by the EntitiyManagerFactory which is configured by the persistence unit. The persistence unit is described via the file
“persistence.xml ” in the directory MET A-INF in the source folder . It defines a set of entities which are logically connected and the connection properties as
shown below .
persistence.xml
<persistence version ="1.0"
xmlns ="http://java.sun.com/xml/ns/persistence" xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
xsi:schemaLocation ="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_1_0.xsd" >
<persistence -unit name ="myapp-server" transaction -type="RESOURCE_LOCAL" >
 <provider >org.hibernate .ejb.HibernatePersistence </provider >
 <!-- import other mapping files if any -->
 <mapping -file>MET A-INF/myApp2 .xml</mapping -file>
 <class >com.mycompany .myapp .Person </class >
 <exclude -unlisted -classes >true</exclude -unlisted -classes >
 <properties >
 <property name ="javax.persistence.jdbc.driver" value ="org.apache.derby .jdbc.EmbeddedDriver" />
 <property name ="javax.persistence.jdbc.url" value ="jdbc:derby:/home/myapp/databases/simpleDb;create=true" />
 <property name ="javax.persistence.jdbc.user" value ="test" />
 <property name ="javax.persistence.jdbc.password" value ="test" />
 </properties >
</persistence -unit>

Usually , JPA defines a persistence unit through the MET A-INF/persistence.xml file. Starting with Spring 3.1, this XML file is no longer necessary – the
LocalContainerEntityManagerFactoryBean now supports a ‘packagesT oScan’ property where the packages to scan for @Entity classes can be specified. The
snippet below shows how you can bootstrap with or without persistence.xml.</persistence >
/...
@Configuration
@PropertySource (value =
{"classpath:/common/jpa.properties"
)
@EnableT ransactionManagement
public class JpaConfig
{
 @Value("${my_app_common_jpa_showSql:false}" )
 private Boolean showSql ;
 
 @Value("${my_app_common_jpa_hibernateDialect}" )
 private String hibernateDialect ;
 
 @Value("${my_app_common_jpa_generateStatistics:false}" )
 private Boolean generateStatistics ;
 
 @Value("${my_app_common_jpa_generateDdl:false}" )
 private Boolean generateDdl ;
 
 @Value("${my_app_common_jpa_databasePlatform}" )
 private String databasePlatform ;
 
 @Resource (name = "myAppDataSource" )
 private DataSource dataSource ;

 @Bean
 public Map<String , Object > jpaProperties ()
 { 
 Map<String , Object > props = new HashMap <String , Object >();
 props .put("hibernate.dialect" , hibernateDialect );
 props .put("hibernate.generate_statistics" , generateStatistics );
 return props ;
 }
 
 @Bean
 public JpaV endorAdapter jpaVendorAdapter ()
 {
 HibernateJpaV endorAdapter hibernateJpaV endorAdapter = new HibernateJpaV endorAdapter ();
 hibernateJpaV endorAdapter .setShowSql (showSql );
 hibernateJpaV endorAdapter .setGenerateDdl (generateDdl );
 hibernateJpaV endorAdapter .setDatabasePlatform (databasePlatform );
 
 return hibernateJpaV endorAdapter ;
 }
 
 @Bean
 public PlatformT ransactionManager transactionManager ()
 {
 return new JpaT ransactionManager (entityManagerFactory ());
 }
 
 @Bean
 public EntityManagerFactory entityManagerFactory ()
 {
 LocalContainerEntityManagerFactoryBean lef = new LocalContainerEntityManagerFactoryBean ();
 lef.setDataSource (dataSource );
 lef.setJpaPropertyMap (this.jpaProperties ());
 lef.setJpaV endorAdapter (this.jpaVendorAdapter ());
 lef.setPersistenceXmlLocation ("MET A-INF/persistence.xml" );
 lef.afterPropertiesSet ();
 return lef.getObject ();
 }
 
 @Bean
 public PersistenceExceptionT ranslationPostProcessor exceptionT ranslation ()
 {
 return new PersistenceExceptionT ranslationPostProcessor ();
 }

The jpa.pr operties can be defined as shown below .

---

## Q5: What is an EntityManager?

**Answer:**

The entity manager javax.persistence. EntityManager provides the operations from and to the database, e.g. find objects, persists them, remove objects
from the database, etc. Entities which are managed by an EntityManager will automatically propagate these changes to the database (if this happens within a
commit statement). These objects are known as persistent object. If the Entity Manager is closed (via close()) then the managed entities are in a detached state.
These are known as the detached objects. If you want synchronize them again with the database, the a Entity Manager provides the mer ge() method. Once
merged, the object(s) becomes perstent objects again.
The EntityManager is the API of the persistence context, and an EntityManager can be injected directly in to a DAO without requiring a JP A Template. The
Spring Container is capable of acting as a JP A container and of injecting the EntityManager by honoring the @PersistenceContext (both as field-level and a
method-level annotation). @Bean
 public HibernateExceptionT ranslator hibernateExceptionT ranslator ()
 {
 return new HibernateExceptionT ranslator ();
 }
# properties for JP A
my_app_common_jpa_showSql =false
my_app_common_jpa_generateDdl =false
my_app_common_jpa_databasePlatform =SYBASE
my_app_common_jpa_hibernateDialect =org.hibernate .dialect .SybaseASE15Dialect
my_app_common_jpa_generateStatistics =true
my_app_common_aesJndiName =java:comp /env/jdbc/my_db

---

## Q6: What is an Entity?

**Answer:**

A class which should be persisted in a database it must be annotated with javax.persistence. Entity . Such a class is called Entity . An instances of the class
will be a row in the person table. So, the columns in the person table will be mapped to the Person java object annotated as @Entity . Here is the sample Person
class./...
mport com.mycompany .myapp .model .Person
mport java.util.ArrayList ;
mport java.util.List;
mport javax .persistence .EntityManager ;
mport javax .persistence .PersistenceContext ;
mport javax .persistence .Query ;
mport org.springframework .stereotype .Repository ;
@Repository ("personDao" )
public class PersonDaoImpl implements PersonDao
{
 @PersistenceContext
 private EntityManager em;
 
 @Override 
 public List<Person > fetchPersonByFirstname (String fName )
 {
 Query query = em.createQuery ( "from Person p where p.firstname = :personName" , Person .class );
 query .setParameter ("firstName" , fName );
 
 List<Person > persons = new ArrayList <Person >();
 List<Person > results = query .getResultList ();
 
 return persons ;
 }
 
 public Person find(Integer id)
 {
 return em.find(Person .class , id);
 } 

@Entity
@Table(name = "person" , schema = "dbo" , catalog = "my_db_schema" )
@Where (clause = "inactiveFlag = 'N'" )
@TypeDefs (
{@TypeDef (name = "IdColumn" , typeClass = IdColumnT ype.class )})
public class UnitT rustPrice implements java.io.Serializable
{
 private int id;
 private String firstName ;
 private byte[] timestamp ;
 public Person (){}
 @Id
 @GeneratedV alue(strategy = GenerationT ype.IDENTITY )
 @Column (name = "person_id" , nullable = false , precision = 9, scale = 0)
 @Type(type = "int")
 public int getId ()
 {
 return this.id;
 }
 
 public void setId (int id)
 {
 this.id = id;
 }
 @Version
 @Column (name = "timestamp" , insertable = false , updatable = false , unique = true, nullable = false )
 @Generated (GenerationT ime.ALWAYS)
 public byte[] getTimestamp ()
 {
 return this.timestamp ;
 }
 
 public void setTimestamp (byte[] timestamp )
 {
 this.timestamp = timestamp ;
 }

---

## Q7: What are the dependency jars required for a JP A application?

**Answer:**

Add relevant jar files via your Maven pom.xml file.
persistence-api
Hibernate as the implementation @Column (name = "first_name" , nullable = false , length = 30)
 public String getFirstName ()
 {
 return this.firstName ;
 }
 public void setFirstName (String fName )
 {
 this.firstName = fName ;
 }
<dependency >
 <groupId >javax .persistence </groupId >
 <artifactId >persistence -api</artifactId >
 <version >1.0.2 </version >
</dependency >
<!-- JPA and hibernate -->

Q8 What is dif ference between CrudRepository and JpaRepository interfaces in Spring Data?
A8.JpaRepository extends PagingAndSortingRepository , which in turn extends CrudRepository . Their main functions are:
— CrudRepository mainly provides CRUD (Create Read Update and Delete) functions.
— PagingAndSortingRepository provide methods to do pagination and sorting records.
— JpaRepository provides some JP A related method such as flushing the persistence context and delete records in a batch.

---

## Q9: What steps are required for Spring CrudRepository to work with JP A?

**Answer:**

Step 1 : Add the jar dependency to your maven pom.xml file.<dependency >
 <groupId >org.hibernate </groupId >
 <artifactId >hibernate -core</artifactId >
</dependency >
<dependency >
 <groupId >org.hibernate </groupId >
 <artifactId >hibernate -entitymanager </artifactId >
</dependency >
<dependency >
 <groupId >org.hibernate .javax .persistence </groupId >
 <artifactId >hibernate -jpa-2.0-api</artifactId >
</dependency >
<!-- validator -->
<dependency >
<groupId >javax .validation </groupId >
 <artifactId >validation -api</artifactId >
</dependency >
<dependency >
 <groupId >org.hibernate </groupId >
 <artifactId >hibernate -validator </artifactId >
</dependency >

Step 2 : Define the JP A entity — that is your model class that maps to the table in the database.
Step 3 : Define the CRUD repository by extending Spring’ s CrudRepository class. The CrudRepository gives you out of the box access to the following standard
methods
— delete(T entity) , which deletes the entity given as a parameter .
— findAll() , which returns a list of entities.<dependency >
<groupId >org.springframework .data</groupId >
<artifactId >spring -data-jpa</artifactId >
<version >1.2.0 -RELEASE </version >
<dependency >
/...
@Entity
@Table(name = "ReportStructure" )
public class Node extends AbstractPersistable <Long >
{
 private static final long serialV ersionUID = 1L;
 
 @ManyT oOne
 @JoinColumn (name = "ParentId" , insertable = false , updatable = false )
 private Node parent ;
 
 @OneT oMany (cascade = CascadeT ype.ALL )
 @JoinColumn (name = "ParentId" , nullable = false )
 private List<Node > children = new ArrayList <Node >();
 
 @OneT oOne (cascade = CascadeT ype.ALL )
 @PrimaryKeyJoinColumn
 private NodeAttributes attributes ;
 
 //....

— findOne(ID id) , which returns the entity using the id given a parameter as a search criteria.
— save(T entity) which saves the entity given as a parameter .
You can provide additional custom methods as shown below ,
Step 4: : The Spring config file to wire up JP A. This example uses HSQL.mport org.springframework .data.jpa.repository .JpaRepository ;
mport org.springframework .data.jpa.repository .Query ;
mport org.springframework .data.repository .CrudRepository ;
public interface NodeRepository extends CrudRepository <Node , Long >
{
 @Query ("SELECT n from Node n JOIN n.key k WITH k.clientId = ?1 and k.evalDate = ?2 "
 + "WHERE n.parent is null and n.isSoftDeleted = false " )
 List<Node > find(String clientId , Date evalDate );
 
 @Query ("SELECT key from NodeKey key WHERE key .clientId = ?1 and key .isSoftDeleted = false" )
 List<NodeKey > fetch (String clientId ); 
<!-- Directory to scan for repository classes -->
<jpa:repositories
 base-package ="com.mydomain.model" />
<bean class ="org.springframework.orm.jpa.JpaT ransactionManager"
id="transactionManager" >
<property name ="entityManagerFactory"
 ref="entityManagerFactory" />
<property name ="jpaDialect" >
 <bean class ="org.springframework.orm.jpa.vendor .HibernateJpaDialect" />

Step 5: : Use the NodeRepository for CRUD operations in the service layer .</property >
</bean >
<bean id="entityManagerFactory"
class ="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean" >
<property name ="dataSource" ref="dataSource" />
<property name ="jpaV endorAdapter" >
 <bean class ="org.springframework.orm.jpa.vendor .HibernateJpaV endorAdapter" >
 <property name ="generateDdl" value ="true" />
 <property name ="database" value ="HSQL" />
 </bean >
</property >
</bean >
public class ReportServiceImpl extends ReportService {
 @Autowired
 NodeRepository nodeRepository ;
 //...
}

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
