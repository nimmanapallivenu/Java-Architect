# 5 4 JEE bean validation, asynchronous processing & web fragments interview Q&As   Java Success.com

## Table of Contents

- [Q1: How does the new bean validation framework avoid duplication across multiple Jav](#q1)
- [Q2: Can you have custom user defined bean validations?](#q2)
- [Q3: What are the benefits of the asynchronous processing support that was introduced](#q3)
- [Q4: What are benefits of web fragements introduced in Servelt 3.0 spec?](#q4)

---

## Q1: How does the new bean validation framework avoid duplication across multiple Java EE layers?

**Answer:**

Developers often code the same validation logic in multiple layers of an application, which is time consuming and error -prone. At times they put the
validation logic in their data model, cluttering it with what is essentially metadata. JEE 6 Improves validation and duplication with a much improved annotation
based bean validation. Bean V alidation of fers a framework for validating Java classes written according to JavaBeans conventions. Y ou use annotations to
specify constraints on a JavaBean. For example,
public class Contact {
 @NotEmpty @Size(max=100)
 private String firstName ;
 @NotEmpty @Size(max=100)
 private String surname ;
 
 @NotEmpty @Pattern ("[a-zA-Z]+" )
 private String category ;
 
 @ShortName
 private String shortName ; //custom validation
 ...
 public String getFirstName () {
 return firstName ;
 }
 public void setFirstName (String firstName ) {
 this.firstName = firstName ;
 }
 ...

---

## Q2: Can you have custom user defined bean validations?

**Answer:**

Yes.
Firstly define a custom annotation
and then provide the validation logic@ConstraintV alidator (ShortNameV alidator .class )
@Documented
@Target({ElementT ype.METHOD , ElementT ype.FIELD , ElementT ype.ANNOT ATION_TYPE })
@Retention (RUNTIME )
public @interface ShortName {
 String message () default "Wrong name" ;
 String [] groups () default {};
public class ShortNameV alidator implements ConstraintV alidator <Contact , String > {
 private final static Pattern SHOR TNAME_P ATTERN = Pattern .compile ("[a-zA-Z]{5,30}" );
 public void initialize (Contact constraintAnnotation ) {
 // nothing to initialize
 }
public boolean isValid(String value , ConstraintV alidatorContext context ) {
 return SHOR TNAME_P ATTERN .matcher (value ).matches ();
 }

---

## Q3: What are the benefits of the asynchronous processing support that was introduced in Servlet 3.0 in JEE 6?

**Answer:**

1. If you are building an online chess game or a chat application, the client browser needs to be periodically refreshed to reflect the changes. This used to be
achieved via a technique known as the server -polling (aka client pull or client refresh). Y ou can use the HTML tag for polling the server . This tag tells the client
it must refresh the page after a number of seconds.
The URL newPage.html will be refreshed every 10 seconds. This approach has the downside of wasting network bandwidth and server resources. W ith the
introduction of this asynchronous support, the data can be sent via the mechanism known as the server push as opposed to server polling. So, the client waits for
the server to push the updates as opposed to frequently polling the server .
2. The Ajax calls are integral part of any web development as it provides richer user experience. This also means that with Ajax, the clients (i.e. browsers)
interact more frequently with the server compared to the page-by-page request model. If an Ajax request needs to tap into server side calls that are very time
consuming (e.g. report generation), the synchronous processing of these Ajax requests can degrade the overall performance of the application because these
threads will be blocked as the servers generally use a thread pool with finite number of threads to service concurrent requests. The asynchronous processing will
allow these time consuming requests to be throttled via a queue, and the same thread(s) to be recycled to process queued requests without having to chew up the
other threads from the server thread-pool. This approach can be used for non Ajax requests as well.
Note: In JEE 6, The EJB 3.1 can also specify a Session Bean to be asynchronous.

---

## Q4: What are benefits of web fragements introduced in Servelt 3.0 spec?

**Answer:**

Web applications use frameworks like JSF , Struts, Spring MVC, T apestry , etc. These frameworks normally bootsrap (i.e register) via the web.xml file using
the <servlet> and <listener> tags. For example
Old way: The web.xml file<MET A http-equiv ="Refresh" content ="10"; url="newPage.html" />

If a particular application uses more than one framework, the above approach is not modular as you will have to bootstrap all the frameworks within the same
web.xml file, making it lar ge and dif ficult to isolate framework specific descriptors. The Servlet 3.0 specification addresses this issue by introducing web
fragments. A web fragment can be considered as one of the segment of the whole web.xml and it can be thought of as one or more web fragments constituting a
single web.xml file. The fragment files are stored under /MET A-INF/web-fragment.xml, and it is the responsibility of the container to scan the fragement files
during the server start-up.
New way: The web-fragment.xml file<?xml version ="1.0" encoding ="UTF-8" ?>
<web-app version ="2.5"
 xmlns ="http://java.sun.com/xml/ns/javaee"
 xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://java.sun.com/xml/ns/javaee
 http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" >
 <servlet >
 <servlet -name >My JSFServlet </servlet -name >
 <servlet -class >javax .faces .webapp .FacesServlet </servlet -class >
 <load-on-startup >1</load-on-startup >
 </servlet >
 <servlet -mapping >
 <servlet -name >My JSFServlet </servlet -name >
 <url-pattern >/faces /*</url-pattern >
 </servlet -mapping >
</web-app>
<web-fragment >
 <servlet >
 <servlet -name >myFrameworkSpecificServlet </servlet -name >
 <servlet -class >myFramework .myFrameworkServlet </servlet -class >

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit » </servlet >
 <listener >
 <listener -class >myFramework .myFrameworkListener </listener -class >
 </listener >
</web-fragment >

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
