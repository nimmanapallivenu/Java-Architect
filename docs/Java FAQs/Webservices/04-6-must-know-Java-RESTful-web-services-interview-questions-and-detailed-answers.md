# 4 6 must know Java RESTful web services interview questions and detailed answers

## Table of Contents

- [Q1: What is RESTful W eb service, and why is it favored over SOAP W eb service?](#q1)
- [Q2: What is the RESTFul W eb Service URI conventions? Can you discuss verbs and noun](#q2)
- [Q3: What are the various implementations of JAX-RS available to choose from in Java?](#q3)
- [Q4: How would you go about implemnting the RESTful W eb service using the framework ](#q4)
- [Q5: What happens if RestFul resources are accessed concurrently by multiple clients ](#q5)
- [Q6: What are some of the annotations used in JAX-RS?](#q6)

---

## Q1: What is RESTful W eb service, and why is it favored over SOAP W eb service?

**Answer:**

REST stands for REpresentational State T ransfer (REST), which is a stateless software architecture that reads webpages containing XML, JSON, Plain text,
etc. REST is a simpler alternative to Simple Object Access Protocol (SOAP) and W eb Services Description Language W eb services (WSDL), and has become a
popular W eb application program interface (API) model. A RESTful API, or RESTful W eb service, uses both HTTP and REST .
1) REST is very lightweight, and does not have all the complexity of SOAP
2) REST is a very simple in that it uses HTTP GET , POST , PUT , and DELETE methods to update resources on the server .
3) REST typically is best used with Resource Oriented Architecture (ROA). In this mode of thinking everything is a resource, and performs CRUD (Create,
Read, Update, and Delete) operations on those resources. HTTP POST to Create a resource on the server , HTTP GET to Read a resource from the server , HTTP
PUT to Update a resource on the server , and HTTP DELETE to delete a resource on the server . The resource could be account, transaction, etc.
For example:
RESTful W eb service is easy , straightforward, supports multiple data formats like XML, JSON, etc and easily testable. For example,
4) It can be tested by
a. Directly invoking the service from the browser by typing a URL if the RESTFul service supports GET request with query parameters. For example,http(s)://myserver .com:8080/app-name/{version-no}/{domain}/{rest-reource-convetion}
# to list all the accounts:
http(s)://myserver .com:8080/accounting-services/1.0/forecasting/accounts
# creates a new transaction for account 123
http(s)://myserver .com:8080/accounting-services/1.0/forecasting/account/123/transaction
http://localhost:8080/executionservices/execution/1.0/order/create?accountId=123&qty=25

b. You could use a Firefox plugin like “poster” to post an XML request to your service.
c. You could write a W eb Service client to consume the W eb service from a test class or a separate application client. Y ou could use libraries like HttpClient,
CXF Client, URLConnection, etc to connect to the RESTful service.

---

## Q2: What is the RESTFul W eb Service URI conventions? Can you discuss verbs and nouns and singular Vs plural resources?

**Answer:**

The high level pattern for the RESTful URI is
to list all the accounts:
This is a plural resource returning a collections of accounts. The URI contains nouns representing the resources in a hierarchical structure. For example, if you
want a to get a particular transaction value under an account you can do
Where 123 is the account number and 567 is the transaction number or id. This is a singular resource returning a single transaction.
if you want to list a collection of transactions that are greater than a particular date?http(s)://myserver .com:8080/app-name/{version-no}/{domain}/{rest-reource-convetion}
http(s)://myserver .com:8080/accounting-services/1.0/forecasting/accounts
http(s)://myserver .com:8080/accounting-services/1.0/forecasting/account/123/transaction/567

The verbs are defined via the HTTP methods GET , POST , PUT , and DELETE. The above examples are basically GET requests returning accounts or
transactions. If you want to create a new transaction request, you do a POST with the following URL.
The actual transaction data will be sent in the body of the request as JSON data. The above URI will be used with a PUT http method to modify an existing
transaction record.
You can also control which method gets executed with the help of HTTP headers or host names in the URL. Here is a Spring MVC example:http(s)://myserver .com:8080/accounting-services/1.0/forecasting/account/123/transactions/search?txn-date=20120201
http(s)://myserver .com:8080/accounting-services/1.0/forecasting/account/123/transaction
@Controller
@RequestMapping ("/forecasting" )
public class CashForecastController
{
 @RequestMapping (
 value = "/accounts,
 method = RequestMethod.GET ,
 produces = " application /json")
 @ResponseBody
 public AccountResult getAllAccounts(HttpServletResponse response) throws Exception
 { 
 //get the accounts via a service and a dao layers

Q. Dos and Don’ts:
— Don’ t use query parameters to alter state. Use query parameters for sub-selection of a resource like pagination, filtering, search queries, etc
— Don’ t use implementation-specific extensions in your URIs (.do, .py , .jsf, etc.). Y ou can use .csv , .json, etc.
— Don’ t ever use GET to alter state. Use GET for as much as possible. Favor POST over PUT when in doubt.
— Don’ t perform an operation that is not idempotent with PUT .
— Do use DELETE in preference to POST to remove resources.
— Don’ t clutter your URL with verbs or stuf f that should be in a header or body . Move stuf f out of the URI that should be in an HTTP header or a body .
Whenever it looks like you need a new verb in the URL, think about turning that verb into a noun instead. For example, turn ‘activate’ into ‘activation’, and
‘validate’ into ‘validation’.

---

## Q3: What are the various implementations of JAX-RS available to choose from in Java?

**Answer:**

When you’re developing with REST in Java, you have a lot of options to choose from in terms of the frameworks. There’ s Jersey , the reference
implementation from Oracle, then you have RestEasy , the JBoss choice, and there is CXF , the Apache choice.

---

## Q4: How would you go about implemnting the RESTful W eb service using the framework of your choice?

**Answer:**

} 
 @RequestMapping(
 value = " /account /transaction ",
 method = RequestMethod.POST)
 public @ResponseBody T ransaction addT ransaction(@RequestBody T ransaction txn, HttpServletResponse response)
 throws Exception
 {
 //logic to create a new T ransaction records via service and dao layers 
 }
@RequestMapping(
 value = " /account /transaction ",
 method = RequestMethod .PUT )
 public @ResponseBody Transaction modifyT ransaction (@RequestBody Transaction txn, HttpServletResponse response )
 throws Exception
 {
 //logic to modify a T ransaction record via service and DAO layers 
 }

Step 1: In the pom.xml define the JAX-RS library
Step 2: Implement the RESTful W eb service
Step 3: Bootstrap the jboss resteasy via web.xml deployment descriptor<dependencies >
 <dependency >
 <groupId >org.jboss .resteasy </groupId >
 <artifactId >resteasy -jaxrs </artifactId >
 <version >2.2.1.GA </version >
 </dependency >
</dependencies >
mport javax .ws.rs.GET ;
mport javax .ws.rs.Path;
mport javax .ws.rs.PathParam ;
mport javax .ws.rs.core.Response ;
@Path("/hello" )
public class HelloServiceImpl {
 @GET
 @Path("/{name}" )
 public Response printMessage (@PathParam ("name" ) String msg) {
 String result = "Restful hello: " + name ;
 return Response .status (200).entity (result ).build ();
 }

<web-app id="WebApp_ID" version ="2.4"
 xmlns ="http://java.sun.com/xml/ns/j2ee"
 xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xsi:schemaLocation ="http://java.sun.com/xml/ns/j2ee
 http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd" >
 <display -name >Restful Web Application </display -name >
 <!-- Auto scan REST service -->
 <context -param >
 <param -name >resteasy .scan</param -name >
 <param -value >true</param -value >
 </context -param >
 <!-- this need same with resteasy servlet url-pattern -->
 <context -param >
 <param -name >resteasy .servlet .mapping .prefix </param -name >
 <param -value >/rest</param -value >
 </context -param >
 <listener >
 <listener -class >
 org.jboss .resteasy .plugins .server .servlet .ResteasyBootstrap
 </listener -class >
 </listener >
 <servlet >
 <servlet -name >resteasy -servlet </servlet -name >
 <servlet -class >
 org.jboss .resteasy .plugins .server .servlet .HttpServletDispatcher
 </servlet -class >
 </servlet >
 <servlet -mapping >
 <servlet -name >resteasy -servlet </servlet -name >
 <url-pattern >/rest/*</url-pattern >
 </servlet -mapping >
</web-app>

Step 4: The URL to hit the resource
Step 5: The output:
“Restful hello: Peter”

---

## Q5: What happens if RestFul resources are accessed concurrently by multiple clients ?

**Answer:**

Since a new Resource instance is created for every incoming Request, there is no need to make it thread-safe or add synchronization. Concurrent clients can
safely access the RestFul resources.

---

## Q6: What are some of the annotations used in JAX-RS?

**Answer:**

@GET , @POST , @PUT , @DELETE to specify what type of verb this method (or web service) will perform
@Pr oduces to specify the type of output this method (or web service) will produce.http://localhost:8080/RESTfulExample/rest/hello/Peter
@DELETE
@Produces ("application/json" )
@Path("{accountId}" )
public RestResponse <Account > delete (@PathParam ("accountId" ) int accountId ) {
..
}

@Consumes to specify the MIME media types a REST resource can consume
@Path to specify the URL path on which this method will be invoked
@PathParam to bind REST style parameters to method ar guments. For example http://localhost:8080/context/accounting-services/PeterAndCo@GET
@Produces ("application/json" )
public Account getAccount () {
...
}
@PUT
@Consumes ("application/json" )
@Produces ("application/json" )
@Path("{accountId}" )
public RestResponse <Account > update (Account account ) {
..
}
@GET
@Produces ("application/xml" )
@Path("accounting-services/{accountName}" )
public Account getAccount () {
...
}

@QueryParam to access parameters in query string (http://localhost:8080/context/accounting-services?accountName=PeterAndCo).
@FormParam to read parameters sent in a POST request. REST resources usaually consume XML/JSON, but at times you want to read the paramewters in
POST .@GET
@Produces ("application/xml" )
@Path("accounting-services/{accountName}" )
public Acount getAccount (@PathParam ("accountName" ) String accountName ) {
Account account = accountService .findByAccountName (accountName );
return account ;
}
@GET
@Produces ("application/xml" )
@Path("accounting-services" )
public Acount getAccount (@QueryParam ("accountName" ) String accountName ) {
Account account = accountService .findByAccountName (accountName );
return account ;
}
@POST
public String save(@FormParam ("accountName" ) String accountName ,
 @FormParam ("accountNumber" ) String accountNumber ) {

 ...
}

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
