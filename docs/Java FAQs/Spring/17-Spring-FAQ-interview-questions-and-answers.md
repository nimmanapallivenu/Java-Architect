# 17 Spring FAQ interview questions and answers

## Table of Contents

- [Q1: Does Spring dependency injection happen during compile time or runtime?](#q1)
- [Q2: What is the dif ference between prototype scope and singleton scope? Which one i](#q2)
- [Q3: When will you use singleton scope? When will you use prototype scope?](#q3)
- [Q4: How do you provide configuration metadata or wire up dependencies in the Spring ](#q4)
- [Q5: Would both singleton and prototype bean’ s life cycle be managed by the Spring I](#q5)
- [Q6: What happens if you inject a prototype scoped bean into a singleton scoped bean?](#q6)
- [Q7: What if you want the singleton scoped bean to be able to acquire a brand new ins](#q7)
- [Q8: What are the scopes defined in HTTP context?](#q8)
- [Q9: Does Spring allow you to define your own bean scopes?](#q9)
- [Q10: Spring promotes the “Open/Closed principle (OCP)”. What is your understanding of](#q10)
- [Q11: What is AOP and how does Spring provide AOP support?](#q11)
- [Q12: What are the various AOP terminologies? What is the dif ference between join poi](#q12)
- [Q13: What does a typical Spring application contain? What is an @Autowired annotation](#q13)
- [Q14: Which DI would you favor – Constructor -based or setter -based DI?](#q14)
- [Q15: How can you inject a Java Collection in Spring?](#q15)
- [Q16: What does the @Required annotation mean?](#q16)
- [Q17: What does the @Qualifier annotation mean?](#q17)

---

## Q1: Does Spring dependency injection happen during compile time or runtime?

**Answer:**

Runtime during creating an object.

---

## Q2: What is the dif ference between prototype scope and singleton scope? Which one is the default?

**Answer:**

Singleton means single bean instance per IoC container , and prototype means any number of object instances per IoC container . The default scope is
“singleton”.

---

## Q3: When will you use singleton scope? When will you use prototype scope?

**Answer:**

Singleton scope is used for stateless object use. For example, injecting a DAO (i.e. Data Access Object) into a business service object. DAOs don’ t need to
maintain conversational states.
Single instance of “myDaoDef” and “myServiceDef” are created
Q. Is a singleton bean thread-safe?
A. No. Multiple threads access a single instance. It needs to be coded in a thread-safe manner .
Prototype is useful when your objects maintain state in a multi-threaded environment. Each thread needs to use its own object and cannot share the single
object. For example, you might hava a RESTFul web service client making multi-threaded calls to W eb services. The REST easy client APIs like RESTEasy
uses the Apache Connection manager which is not thread safe and each thread should use its own client. Hence, you need to use the prototype scope.
Multiple instances of “myDaoDef” and “myServiceDef” are created<beans xmlns ="http://www .springframework.or g/schema/beans"
 xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://www .springframework.or g/schema/beans http://www .springframework.or g/schema/beans/spring-beans-3.0.xsd" >
 <bean id="myDaoDef" class ="com.mycompany .understanding.spring.MyDaoImpl" scope ="singleton" />
 
 <bean id="myServiceDef" class ="com.mycompany .understanding.spring.MyServiceImpl" scope ="singleton" >
 <constructor -arg name ="myDao" ref="myDaoDef" />
 </bean >
</beans >

---

## Q4: How do you provide configuration metadata or wire up dependencies in the Spring Container?

**Answer:**

There are three important methods to provide configuration metadata to the Spring Container:
1) XML based configuration file. This was shown in the above XML file with “myDaoDef” and “myServiceDef”
2) Annotation-based configuration.
3) Java-based configuration.
2) Annotation-based configuration
Spring annotations<beans xmlns ="http://www .springframework.or g/schema/beans"
 xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://www .springframework.or g/schema/beans http://www .springframework.or g/schema/beans/spring-beans-3.0.xsd" >
 <bean id="myDaoDef" class ="com.mycompany .understanding.spring.MyDaoImpl" scope ="prototype" />
 
 <bean id="myServiceDef" class ="com.mycompany .understanding.spring.MyServiceImpl" scope ="prototype" >
 <constructor -arg name ="myDao" ref="myDaoDef" />
 </bean >
</beans >

The MyDaoImpl :
The MyServiceImpl :
The annotations shown above allow you to declare beans that are to be picked up by autoscanning with or @ComponentScan .package com.mycompany .understanding .spring ;
@Repository (value = "myDaoDef" )
@Scope ("singleton" )
public class MyDaoImpl implements MyDao
{
 //...
}
package com.mycompany .understanding .spring ;
@Service (value = "myServiceDef" )
@Scope ("singleton" )
@Transactional (propagation = Propagation .SUPPOR TS)
public class MyServiceImpl implements MyService
{
 
 @Resource (name = "myDaoDef" )
 private MyAppDao myAppDao ;
 //.....

3) Java-based configuration.
@Configuration annotation was designed as the replacement for XML configuration files.
@Bean annotation tells that this bean to be managed by the Spring IoC container .

---

## Q5: Would both singleton and prototype bean’ s life cycle be managed by the Spring IoC container?

**Answer:**

Yes and no. The singleton bean’ s complete life cycle will be managed by Spring IoC container , but with regards to prototype scope, IoC container only
partially manages the life cycle – instantiates, configures, decorates and otherwise assembles a prototype object, hands it to the client and then has no further
knowledge of that prototype instance. As per the spring documentation
“This means that while initialization lifecycle callback methods will be called on all objects regardless of scope, in the case of prototypes, any configured
destruction lifecycle callbacks will not be called. It is the responsibility of the client code to clean up prototype scoped objects and release any expensive
resources that the prototype bean(s) are holding onto.”

---

## Q6: What happens if you inject a prototype scoped bean into a singleton scoped bean?

**Answer:**

A new prototype scoped bean will be injected into a singleton scoped bean once at runtime, and the same prototype bean will be used by the singleton bean.@Configuration
@ComponentScan (
 basePackages ={"com.mycompany" },
 useDefaultFilters = false ,
 includeFilters =
 { 
 @ComponentScan .Filter (type = FilterT ype.ASSIGNABLE_TYPE , value = MyService .class ),
 @ComponentScan .Filter (type = FilterT ype.ASSIGNABLE_TYPE , value = MyDao .class )
 })
public class AppConfig {
 @Bean (name ="anotherMyAppBean" )
 public HelloW orldService anotherMyAppService () {
 return new AnotherMyAppServiceImpl ();
 }

---

## Q7: What if you want the singleton scoped bean to be able to acquire a brand new instance of the prototype-scoped bean again and again at runtime?

**Answer:**

In this use-case, there is no use in just dependency injecting a prototype-scoped bean into your singleton bean because as stated above, this only happens
once when the Spring container is instantiating the singleton bean and resolving and injecting its dependencies. Y ou can just inject a singleton (e.g. a factory)
bean and then use Java class to instantiate (e.g with a newInstance(…) or create(..) method) a new bean again and again at runtime without relying on Spring or
alternatively have a look at Spring’ s “method injection”. As per Spring documentation for “Lookup method injection”.
“Lookup method injection refers to the ability of the container to override methods on container managed beans, to return the result of looking up another
named bean in the container . The lookup will typically be of a prototype bean as in the scenario described above. The Spring Framework implements this
method injection by dynamically generating a subclass overriding the method, using bytecode generation via the CGLIB library .”
Lookup method to inject new “myDaoDef” prototype bean into singleton “myServiceDef” bean
Note that the class is abstract as Spring will decorate this class with cgilib .<beans xmlns ="http://www .springframework.or g/schema/beans"
 xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://www .springframework.or g/schema/beans http://www .springframework.or g/schema/beans/spring-beans-3.0.xsd" >
 <bean id="myDaoDef" class ="com.mycompany .understanding.spring.MyDaoImpl" scope ="prototype" />
 
 <bean id="myServiceDef" class ="com.mycompany .understanding.spring.MyServiceImpl" scope ="singleton" >
 <lookup -method name ="createMyDao" bean ="myDaoDef" />
 </bean >
</beans >
package com.mycompany .understanding .spring ;
public abstract class MyServiceImpl implements MyService {
protected abstract MyDao createMyDao ();
@Override
public void performT ask() {

---

## Q8: What are the scopes defined in HTTP context?

**Answer:**

Following scopes are only valid in the context of a web-aware Spring ApplicationContext.
– request scope is for a single bean definition to the lifecycle of a single HTTP request.In other words each and every HTTP request will have its own instance
of a bean created of f the back of a single bean definition.
– session scope is for a single bean definition to the lifecycle of a HTTP Session.
– global session scope is for a single bean definition to the lifecycle of a global HTTP Session. T ypically only valid when used in a portlet context.

---

## Q9: Does Spring allow you to define your own bean scopes?

**Answer:**

Yes, from Spring 2.0 onwards you can define custom scopes. For example,
– You can define a ThreadOrRequest and ThreadOrSession scopes to be able to switch between the environment you run in like JUnit for testing and Servlet
container for running as a W eb application.
– You can write a custom scope to inject stateful objects into singleton services or factories.System .out.println ("Performing tasks ............." );
createMyDao ().printData ();
package com.mycompany .understanding .spring ;
public class MyDaoImpl implements MyDao {
@Override
public void printData () {
System .out.println ("printing data" );
System .out.println (this);

– You can write a custom bean scope that would create new instances per each JMS message consumed
– Oracle Coherence has implemented a datagrid scope for Spring beans. Y ou will find many others like this.

---

## Q10: Spring promotes the “Open/Closed principle (OCP)”. What is your understanding of this principle?

**Answer:**

According to GoF design pattern authors “software entities (classes, modules, functions, etc.) should be open for extension , but closed for
modification “. Wherever you have lar ge if/else statements, you need to think if OCP can be applied. Instead of modifying the existing lar ge if/else loops, you
need to add new classes to extend behavior .
Bad: Not closed for modification. T o support “division” or “subtraction” in the future, this class has to be modified with more “else if”s
Good: Closed for modification. T o support “division” or “subtraction” add new classes like “SubtractOperation” that implements the interface “Operation”.mport javax .management .RuntimeErrorException ;
mport org.apache .commons .lang.StringUtils ;
public class MathOperation {
public int operate (int input1 , int input2 , String operator ){
if(StringUtils .isEmpty (operator )){
 throw new IllegalAr gumentException ("Invalid operator: " + operator );
}
if(operator .equalsIgnoreCase ("+")){
 return input1 + input2 ;
}
else if(operator .equalsIgnoreCase ("*")){
 return input1 * input2 ;
} else {
 throw new RuntimeException ("unsupported operator: " + operator );
}

---

## Q11: What is AOP and how does Spring provide AOP support?

**Answer:**

AOP stands for “Aspect Oriented Programming” and it compliments OOP . AOP is used for cross cutting concerns like logging, auditing, service retry ,
deadlock retry , performance profiling, transaction management, etc. In OOP unit of modularity is an object, and in AOP unit of modularity is an Aspect . AOP
framework is pluggable in Spring. AOP provides declarative cross cutting services and allows custom aspects to be implemented.public interface Operation {
 abstract int operate (int input1 , int input2 );
}
public class AddOperation implements Operation {
@Override
public int operate (int input1 , int input2 ) {
 return input1 + input2 ;
}
}
public class MultiplyOperation implements Operation {
@Override
public int operate (int input1 , int input2 ) {
 return input1 * input2 ;
}
}

Spring AOP can be used together with AOP Alliance MethodInterceptors. AOP Alliance compliant interceptors foster interoperability with other AOP
frameworks such as Google Guice. Spring can be used with AspectJ as well, which has annotation syntax that is concise and expressive. AOP Alliance intends
to facilitate and standardize the use of AOP to enhance existing middle ware environments (such as JEE), or development environements (e.g. Eclipse,
NetBeans). The AOP Alliance also aims to ensure interoperability between Java/JEE AOP implementations to build a lar ger AOP community .
Example:

---

## Q12: What are the various AOP terminologies? What is the dif ference between join point and pointcut?

**Answer:**

Aspect, Pointcut, Joinpoints, and Advice.
Aspect is a mechanism where by you can call your method before, after or around some event.
To understand the dif ference between joinpoints and a pointcut , think of a “pointcut” as specifying the weaving rules and “joinpoints” as all situations
satisfying those rules. In the example below:<!-- Spring AOP + AspectJ -->
<dependency >
 <groupId >org.springframework </groupId >
 <artifactId >spring -aop</artifactId >
 <version >${spring .version }</version >
</dependency >
<dependency >
 <groupId >org.aspectj </groupId >
 <artifactId >aspectjrt </artifactId >
 <version >1.6.1 1</version >
</dependency >
<dependency >
 <groupId >org.aspectj </groupId >
 <artifactId >aspectjweaver </artifactId >
 <version >1.6.1 1</version >
</dependency >

Pointcut rule says that an advice (e.g. @Around) should be applied on annotation “@DeadlockRetry” on any method on any class.
Joinpoints will be a list of all methods present in classes with the “@DeadlockRetry” annotation so that advice can be applied on these selected methods.
Advice is the code that runs when program execution reaches one of the jointpoints in your code defined by a pointcut (i.e. weaving rules).
Here is a real life Advice that retries the method when a database deadlock is encountered. The “DeadlockUtil” determines if the exception is deadlock related
by inspecting the exception for presence “encountered a deadlock situation” message or Exception type being “CannotAcquireLockException”. Spring aspects
can work with five kinds of advice:
befor e: Run advice before the a method execution.
after : Run advice after the a method execution regardless of its outcome.
after -returning : Run advice after the a method execution only if method completes successfully .
after -throwing : Run advice after the a method execution only if method exits by throwing an exception.
around : Run advice before and after the advised method is invoked.@Around (value = "@annotation(deadlockRetry)" , argNames = "deadlockRetry" )
public Object invoke (final ProceedingJoinPoint pjp, final DeadlockRetry deadlockRetry ) throws Throwable {
 //................
}
mport java.lang.reflect .Method ;
mport org.aspectj .lang.ProceedingJoinPoint ;
mport org.aspectj .lang.annotation .Around ;
mport org.aspectj .lang.annotation .Aspect ;
mport org.aspectj .lang.reflect .MethodSignature ;
mport org.springframework .core.Ordered ;
@Aspect
public class DeadlockRetryAspect implements Ordered
{
 //In AspectJ, a joint point is really the specification of where/when in your program, your aspect code is running.

 //This runs in any method in any class with annotation - @DeadlockRetry(maxT ries = 10, tryIntervalMillis = 5000) 
 @Around (value = "@annotation(deadlockRetry)" , argNames = "deadlockRetry" )
 public Object invoke (final ProceedingJoinPoint pjp, final DeadlockRetry deadlockRetry ) throws Throwable
 {
 final Integer maxT ries = deadlockRetry .maxT ries();
 long tryIntervalMillis = deadlockRetry .tryIntervalMillis ();
 
 Object target = pjp.getTarget();
 MethodSignature signature = (MethodSignature ) pjp.getSignature ();
 Method method = signature .getMethod ();
 int count = 0;
 
 do
 {
 try
 {
 count ++;
 Object result = pjp.proceed (); // retry
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
 ie.printStacktrace ();
 }
 }
 }
 }
 while (count <= maxT ries);
 
 //gets here only when all attempts have failed
 throw new RuntimeException ("DeadlockRetryMethodInterceptor failed to successfully execute tar get "

The custom annotation that is used as the point cut weaving rule.
Bootstrap AOP into Spring context xml file. + " due to deadlock in all retry attempts" ,
 new DeadlockDataAccessException ("Created by DeadlockRetryMethodInterceptor" , null));
 
 }
 
 @Override
 public int getOrder ()
 {
 return 99;
 }
mport java.lang.annotation .ElementT ype;
mport java.lang.annotation .Inherited ;
mport java.lang.annotation .Retention ;
mport java.lang.annotation .RetentionPolicy ;
mport java.lang.annotation .Target;
@Retention (RetentionPolicy .RUNTIME )
@Target(ElementT ype.METHOD )
@Inherited
public @interface DeadlockRetry
{
 int maxT ries() default 10;
 
 int tryIntervalMillis () default 1000 ;

---

## Q13: What does a typical Spring application contain? What is an @Autowired annotation?

**Answer:**

Spring applications predominantly use POJOs.
1) It will have Java interfaces with method declarations for functions related to business service, data access, etc.
2) Corresponding implementation classes for the interfaces defined.
3) W iring up of the Spring managed beans and dependencies via XML based application context XML files, Anootations, and Java config file with
@Configuration annotation.
4) W iring up of JdbcT emplate, Hibernate, T ransaction Manager , etc
5) wiring up of Spring AOP with aspectj for cross cutting concerns like deadlock retry , service retry , performance profiling, etc.
6) If using SpringMVC, then it needs to be bootstrapped via the web.xml or new JEE annotations.
7) If using some other W eb framework, it needs to be bootstrapped via web.xml file. For example
Pay attention to how the dif ferent artifacts are wired up using both Spring xml files and annotations. The Spring beans can be wired either by name or type.
@Autowir ed by default is a type driven injection. @Autowir ed is a Spring annotation, while @Inject is a JSR-330 annotation. @Inject is equivalent to
@Autowired or @Autowired(required=true). @Qualifier spring annotation can be used to further fine-tune auto-wiring. There may be a situation when you
create more than one bean of the same type and want to wire only one of them with a property , in such case you can use @Qualifier annotation along with
@Autowired to remove the confusion by specifying which exact bean will be wired. It is not a best practice to use @Autowired in lar ge Spring applications as it
can cause confusions.

---

## Q14: Which DI would you favor – Constructor -based or setter -based DI?

**Answer:**

You can use both constructor -based and setter -based Dependency Injection. Favor constructor ar guments for mandatory dependencies and setters for
optional dependencies.

---

## Q15: How can you inject a Java Collection in Spring?

**Answer:**

You can use list, set, map, or prop elements to configure collections.<aop:aspectj -autoproxy />
<context :component -scan base-package ="com.myapp" />
web.xml --> myAppServletContext .xml --> myapp -applicationContext .xml --> MyAppController .java --> MyAppService .Java --> MyAppDaoImpl .java

---

## Q16: What does the @Required annotation mean?

**Answer:**

This annotation indicates that the annotated bean property must be injected at configuration time, and if not set, the container throws
BeanInitializationException if the af fected bean property has not been populated.

---

## Q17: What does the @Qualifier annotation mean?

**Answer:**

When there are mor e than one beans of the same type and only one is needed to be wired with a property , the @Qualifier annotation is used along with
@Autowired annotation to remove the confusion by specifying which exact bean will be wired.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
