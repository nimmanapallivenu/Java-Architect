# Java Annotations

> **Module**: Modifiers Annotations Initializers  
> **Topic**: Java Annotations

---

## 📋 Table of Contents



- [Q1: Are annotations a compile time or run-time feature?](#q1)
- [Q2: Are marker or tag interfaces like Serializable, Runnable, etc obsolete with the ](#q2)
- [Q3: What is an annotation, and why are they so popular and used in every framework l](#q3)
- [Q4: How will you go about defining a custom annotation? Can you give a practical exa](#q4)
- [Q5: What are some of the JAX-RS (i.e RESTful) web service annotations?](#q5)
- [Q6: What are some of the widely used Spring annotations?](#q6)
- [Q7: What are some of the widely used JEE CDI annotations?](#q7)
- [Q8: How would you unit test CDI with JUnit?](#q8)
- [Q9: In Servlet 3.0, why is configuring your servlet via deployment descriptor file w](#q9)

---

## 🔹 Q1: Are annotations a compile time or run-time feature?

**Answer:**

You can have either compile-time or run-time annotations.
@Override is a simple compile-time annotation to catch little mistakes like typing tostring( ) instead of toString( ) in a subclass.
If you remove the toString( ) method in Class A or misspell toString() method in Class B, the compiler will warn you.
User defined annotations can be processed at compile-time using the Annotation Pr ocessing T ool (APT) that is included in the Java 6 itself.
@Test is an annotation that JUnit framework uses at runtime with the help of reflection to determine which method(s) to execute within a test class.public class B extends A {
 
 private String input; 
 @Override
 public String toString (){
 return "input=" + input ;
 } 
public class MyT est{
 @Test
 public void testEmptyness ( ){
 org.junit.Assert .assertT rue(getList ( ).isEmpty ( ));
 }
 private List getList ( ){
 …

The test below fails if it takes more than 100ms to execute at runtime.
The code shown below fails if it does not throw “IndexOutOfBoundsException” or if it throws a different exception at runtime. A negative JUnit test.

---

## 🔹 Q2: Are marker or tag interfaces like Serializable, Runnable, etc obsolete with the advent of annotations (i.e. runtime annotations)?

**Answer:**

Everything that can be done with a marker or tag interfaces in earlier versions of Java can now be done with annotations at runtime using reflection. One of
the common problems with the marker or tag interfaces like Serializable, Cloneable, etc is that when a class implements them, all of its subclasses inherit them
as well whether you want them to or not. You cannot force your subclasses to unimplement an interface. Annotations can have parameters of various kinds, and
they’re much more flexible than the marker interfaces. This makes tag or marker interfaces obsolete, except for
— In the very rare event of the profiling indicating that the runtime checks are expensive due to being accessed very frequently, and compile-time checks with
interfaces is preferred.
— In the event of existing marker interfaces like Serializable, Cloneable, etc are used or Java 5 or later versions cannot be temporarily used.

---

## 🔹 Q3: What is an annotation, and why are they so popular and used in every framework like Spring, JAX-RS (i.e RESTful web service), JEE 6, and a lot more?
When will you favor XML based meta data over annotations based meta data? }
@Test(timeout =100)
public void testT imeout ( ) {
 while (true); //infinite loop
}
@Test (expected =IndexOutOfBoundsException .class )
public void testOutOfBounds ( ) {
 new ArrayList <Object >( ).get(1);
}

**Answer:**

One word to explain Annotation is Metadata. Metadata is data about data. So Annotations are metadata for code. The IDEs, compilers, frameworks, and
other tools read the annotations to control the behavior of the code that are annotated.
Prior to annotation (and even after) XML were extensively used for metadata, but XML is very verbose and its maintenance was becoming troublesome. Since
annotations are closely coupled with the code, they are less verbose. You can define annotations for a class, method, field, etc. XML is defined separately from
the code, so it is more verbose as you have to define the class name, method name, etc.
If you want to set some application wide constants and parameters XML would be a better choice because this is not related with any specific piece of code. If
you want to expose some method as a Web service, annotation would be a better choice as it needs to be tightly coupled with that method and developer of the
method must be aware of this.
Annotations: let you avoid boilerplate code under many circumstances by enabling tools to generate it from annotations in the source code. This leads to
“attribute oriented” (aka declarative) programming. This eliminates the need to maintain “side files” that must be kept up to date with changes in source files.
Annotations based development relieves Java developers from the pain of cumbersome configuration. Annotations provide declarative style programming where
the programmer says what should be done and tools emit the code to do it. This assists rapid application development, easier maintenance, and less likely to be
bug-prone.
In Spring’ s world, an XML config (e.g myapp-applicationContext.xml) to scan for all annotations in the base-package can be defined as shown below .
A POJO is exposed as a RESTful web service with JAX-RS annotations like @Path, @Produces, @GET, @PathParam, etc.<!-- Component -Scan automatically detects annotations in the Java classes. -->
<context :component -scan base-package ="com.myapp.batch" />
package com.mytutorial .webservice ;
mport javax .ws.rs.GET ;
mport javax .ws.rs.Path;
mport javax .ws.rs.PathParam ;
mport javax .ws.rs.Produces ;
mport com.mytutorial .pojo.User ;

In JEE 6 CDI (Contexts and Dependency Injection) world
You need to have at least an empty “ beans.xml ” file defined under MET A-INF resource folder for jar files or under WEB-INF folder for war files for DI to take
effect.
and @inject annotation to inject dependencies.@Path("userservice/1.0" )
@Produces ("application/xml" )
public class HelloUserWebServiceImpl implements HelloUserWebService {
@GET
@Path("/user/{userName}" )
public User greetUser (@PathParam ("userName" ) String userName ) {
 User user = new User ();
 user.setName (userName );
 return user;
}
<beans xmlns ="http://java.sun.com/xml/ns/javaee" xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/beans_1_0.xsd" >
</beans >
mport javax .inject .Inject ;

---

## 🔹 Q4: How will you go about defining a custom annotation? Can you give a practical example?

**Answer:**

Here is an example where methods annotated with the following custom @DeadlockRetry annotation will retry the database operation. The annotation has
the attributes maxT ries and tryIntervalMillis .public class MyApp {
 
 private final HelloService helloService ;
 @Inject
 public MyApp (HelloBean helloBean ){
 this.helloService = helloService ;
 }
 //.....
@Default
public class HelloServiceImpl implements HelloService {
 
 public void sayHello () {
 System.out.println ("say hello ........." );
 }
}
package com.myapp .deadlock ;
mport java.lang.annotation .ElementT ype;
mport java.lang.annotation .Inherited ;
mport java.lang.annotation .Retention ;
mport java.lang.annotation .RetentionPolicy ;
mport java.lang.annotation .Target;

The above custom annotation is annotated with @Retention to tell that it is used at run-time, and @Target to tell that it is for methods.
Q. Now, who processes this annotation?
A. The following dynamic pr oxy class that uses reflection to see if a method is annotated with @DeadlockRetry. If does, retries the database method call.@Retention (RetentionPolicy .RUNTIME )
@Target(ElementT ype.METHOD )
@Inherited
public @interface DeadlockRetry
{
 int maxT ries() default 10;
 int tryIntervalMillis () default 1000 ;
package com.myapp .deadlock ;
mport java.lang.annotation .Annotation ;
mport java.lang.reflect .InvocationHandler ;
mport java.lang.reflect .Method ;
public class DeadlockRetryHandler implements InvocationHandler
{ 
 private Object target;
 
 public DeadlockRetryHandler (Object target)
 {
 this.target = target;
 }
 
 @Override
 public Object invoke (Object proxy, Method method, Object [] args) throws Throwable
 {
 Annotation [] annotations = method .getAnnotations ();
 DeadlockRetry deadlockRetry = (DeadlockRetry ) annotations [0];

 final Integer maxT ries = deadlockRetry .maxT ries();
 long tryIntervalMillis = deadlockRetry .tryIntervalMillis ();
 
 int count = 0;
 
 do
 {
 try
 {
 count ++;
 Object result = method .invoke (target, args); // retry
 return result ;
 }
 catch (Throwable e)
 {
 if (!DeadlockUtil .isDeadLock (e))
 {
 throw new RuntimeException (e);
 }
 
 if (tryIntervalMillis > 0)
 {
 try
 {
 Thread .sleep (tryIntervalMillis );
 }
 catch (InterruptedException ie)
 {
 System.out.println ("Deadlock retry thread interrupted", ie);
 }
 }
 }
 }
 while (count <= maxT ries);
 
 //gets here only when all attempts have failed
 throw new RuntimeException ("DeadlockRetryMethodInterceptor failed to successfully execute tar get "
 + " due to deadlock in all retry attempts" ,
 new DeadlockDataAccessException ("Created by DeadlockRetryMethodInterceptor", null));
 } 

The DeadlockUtil class determines if the exception is related to deadlock or other SQL exception based on error code and exception type like
“LockAcquisitionException”.
Now define the target object interface with the custom annotation. The maxT ries = 10, tryIntervalMillis = 5000.

---

## 🔹 Q5: What are some of the JAX-RS (i.e RESTful) web service annotations?

**Answer:**

@GET, @POST, @PUT, @DELETE to specify what type of verb this method (or web service) will perform.
@Pr oduces to specify the type of output this method (or web service) will produce.
@Consumes to specify the MIME media types a REST resource can consume.
@Path to specify the URL path on which this method will be invoked.
@PathParam to bind REST style parameters to method arguments.
@QueryParam to access parameters in query string (http://localhost:8080/context/accounting-services?accountName=PeterAndCo).
Another example for custom annotation would be for service retry .

---

## 🔹 Q6: What are some of the widely used Spring annotations?

**Answer:**

The Spring beans can be wired either by name or type. @Autowir ed by default is a type driven injection. @Autowired is Spring annotation, while @Inject
is a JEE CDI annotation. @Inject is equivalent to @Autowired or @Autowired(required=true). @Qualifier spring annotation can be used to further fine-tune
auto-wiring. There may be a situation when you create more than one bean of the same type and want to wire only one of them with a property, in such case you
can use @Qualifier annotation along with @Autowired to remove the confusion by specifying which exact bean will be wired.package com.myapp .engine ;
mport com.myapp .DeadlockRetry ;
public interface AccountServicePersistenceDelegate
{
 @DeadlockRetry (maxT ries = 10, tryIntervalMillis = 5000 )
 abstract Account getAccount (String accountNumber );

The different POJO objects in different layers are annotated with one of the following key annotations.
Spring annotations
@Component is the parent annotation from which the other annotations like @Service, @Resource, @Repository etc are defined. Here is an example of a
DAO class annotated with @Repository, and @Resource is used to inject “jdbcT emplateSybase” with which the database calls are made.
The annotations shown above allow you to declare beans that are to be picked up by autoscanning with or @ComponentScan .@Repository (value = "myapp_Dao" )
public class CashForecastDaoImpl implements CashForecastDao
{
 
 @Resource (name = "myapp_JdbcT emplate" )
 private JdbcT emplate jdbcT emplateSybase ;//configure via jdbcContext.xml
 public PortfolioSummaryVO retrievePortfolioSummaries (MyAppPortfolioCriteria criteria ) {
 //............use jdbcT emplateSybase
 }

The @Configuration annotation was designed as the replacement for XML configuration files. You use @Bean annotation to wire up dependencies. Here is an
example.
package com.myapp .bdd.stories ;
mport com.myapp .*;
mport org.powermock .api.mockito .PowerMockito ;
mport org.springframework .context .annotation .Bean ;
mport org.springframework .context .annotation .ComponentScan ;
mport org.springframework .context .annotation .Configuration ;
mport org.springframework .context .annotation .FilterT ype;
mport org.springframework .context .annotation .Import ;
@Configuration
/packages and components to scan and include
@ComponentScan (
 basePackages =
 {
 "com.myapp.calculation.bdd" ,
 "com.myapp.refdata" ,
 "com.myapp.dm.dao"
 
 },
 useDefaultFilters = false ,
 //interfaces
 includeFilters =
 { 
 @ComponentScan .Filter (type = FilterT ype.ASSIGNABLE_TYPE, value = MyApp .class ),
 @ComponentScan .Filter (type = FilterT ype.ASSIGNABLE_TYPE, value = MyAppDroolsHelper .class ),
 @ComponentScan .Filter (type = FilterT ype.ASSIGNABLE_TYPE, value = TransactionService .class ),
 @ComponentScan .Filter (type = FilterT ype.ASSIGNABLE_TYPE, value = TransactionV alidator .class ),
 
 @ComponentScan .Filter (type = FilterT ype.annotation, value = org.springframework .stereotype .Repository .class )
 })
/import other config classes
@Import (
{
 StepsConfig .class ,
 DataSourceConfig .class ,

---

## 🔹 Q7: What are some of the widely used JEE CDI annotations?

**Answer:**

CDI is one of the biggest promises of JEE 6.
@Default, @Alternative, and @Name : A class is @Default by default. Thus marking it a redundant annotation. Mark as @Alternative to annotate the
alternative implementations that implement the same interface. Use the @Named annotation to look up by name. The @Named annotation is also used by JEE
6 application to make the bean accessible via the Unified EL.
The interface
The default implementation JmsConfig .class ,
 PropertiesConfig .class
)
public class StoryConfig
{
 //creates a partially mocked transaction service class
 @Bean (name = "txnService" )
 public TransactionService getTransactionService ()
 {
 return PowerMockito .spy(new TransactionService ());
 } 
public interface PaymentService {
 public void processPayment (BigDecimal amount, Account account );
}

An alternative implementation
An alternative can be selected via XML based deployment files in {classpath}/MET A-INF/beans.xml@Default
public class CashPaymentServiceImpl implements PaymentService {
 public void processPayment (BigDecimal amount, Account account ) {
 //.............
 }
}
@Alternative
public class BPayPaymentServiceImpl implements PaymentService {
 public void processPayment (BigDecimal amount, Account account ) {
 //.............
 }
}
@Alternative
public class CreditCardPaymentServiceImpl implements PaymentService {
 public void processPayment (BigDecimal amount, Account account ) {
 //.............
 }
}

A named implementation
Named annotations can be looked up via the JEE bean container .
@Inject to inject via fields and constructors.<beans xmlns ="http://java.sun.com/xml/ns/javaee" xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/beans_1_0.xsd" >
 <alternatives >
 <class >BPayPaymentServiceImpl </class >
 </alternatives >
</beans >
@Named ("cash" )
public class CashPaymentServiceImpl implements PaymentService {
 public void processPayment (BigDecimal amount, Account account ) {
 //.............
 }
}
public class PaymentMain {
 //...
 public static void main (String [] args) throws Exception {
 PaymentService svc = (PaymentService ) beanContainer
 .getBeanByName ("cash" );
 svc.processPayment (new BigDecimal ("9.00" ), acc);
 }

@Pr oduces for more complex object construction via factory methods .public class PaymentMain {
 
 @Inject
 private PaymentService paymentService ;
 //...
}
public class PaymentMain {
 
 private PaymentService paymentService ;
 @Inject
 public PaymentMain (PaymentService paymentService ) {
 this. paymentService = paymentService ;
 }
 
 //....
mport javax .enterprise .inject .Produces ;
public class PaymentFactory {
 @Produces
 public PaymentService createPaymentService () {

@Qualifier is required when an interface has multiple implementations to choose the one you want to inject. All objects and producers in CDI have qualifiers.
If you do not assign a qualifier, by default has the qualifier @Default and @Any. So, if you don’ t specify a qualifier, you will be assigned one.
Meta meta annotations to define a qualifier return new CashPaymentServiceImpl ();
 }
 //.…..
mport java.lang.annotation .Retention ;
mport java.lang.annotation .Target;
mport static java.lang.annotation .ElementT ype.*;
mport static java.lang.annotation .RetentionPolicy .*;
mport javax .inject .Qualifier ;
@Qualifier
@Retention (RUNTIME )
@Target({TYPE, METHOD, FIELD, PARAMETER })
public @interface BPay {
mport java.lang.annotation .Retention ;
mport java.lang.annotation .Target;
mport static java.lang.annotation .ElementT ype.*;
mport static java.lang.annotation .RetentionPolicy .*;
mport javax .inject .Qualifier ;

Use the qualifier annotations
BPay is injected@Qualifier
@Retention (RUNTIME )
@Target({TYPE, METHOD, FIELD, PARAMETER })
public @interface CreditCard {
@BPay
public class BPayPaymentServiceImpl implements PaymentService {
 public void processPayment (BigDecimal amount, Account account ) {
 //.............
 }
}
@CreditCard
public class CreditCardPaymentServiceImpl implements PaymentService {
 public void processPayment (BigDecimal amount, Account account ) {
 //.............
 }
}
public class PaymentMain {

Multiple types can be injected into the same bean 
 private PaymentService paymentService ;
 @Inject
 public PaymentMain (@BPay PaymentService paymentService ) {
 this. paymentService = paymentService ;
 }
 
 //....
public class PaymentMain {
 //...
 
 @Inject @BPay
 private PaymentService bpayService ;
 
 @Inject @CreditCard
 private PaymentService ccService ;
 private PaymentService paymentService ;
 
 //....
 @PostConstruct
 protected void init() {
 if(account .isBPay ()) {
 this.paymentService = this.bpayService ;
 }
 else {
 this.paymentService = this.ccService ;
 }
 }

Note : @PostConstruct annotation is used to decide which service to use. Alternatively, you can use a factory method with @Pr oduces annotation.

---

## 🔹 Q8: How would you unit test CDI with JUnit?

**Answer:**

@RunWith annotation and pass “CdiRunner .class”.
There are other annotations like @AdditionalClasses, @AdditionalPackages, @AdditionalClasspath, @Produces, @EnabledAlternatives,
@ProducesAlternative, etc and scoping annotations like @InRequestScope, @InSessionScope, and @InConversationScope.

---

## 🔹 Q9: In Servlet 3.0, why is configuring your servlet via deployment descriptor file web.xml optional?

**Answer:**

Servlets 3.0 have come up with a set of new Annotations for the declarations of Servlet Mappings, Init-Params, Listeners, and Filters to make the code
more readable by making the use of Deployment Descriptor (web.xml) absolutely optional.@Produces
public PaymentService createPayment (@BPay PaymentService bpayService ,
 @CreditCard PaymentService ccService ) {
 
 if (account .isBPay ()) {
 return bpayService; 
 } else {
 return ccService ;
 }
@RunWith(CdiRunner .class )
class PaymentServiceT est {
 @Inject
 PaymentService paymentService ;
 //.....
}

Servlet filters, listeners, etc can be configured with annotations.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »@WebServlet (
asyncSupported = false ,
name = "AccountingServlet" ,
urlPatterns = { "/acount" },
initParams = {
 @WebInitParam (name = "param1", value = "value1" ),
 @WebInitParam (name = "param2", value = "value2" )
}
public class AccountingServlet extends HttpServlet {
 //...

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