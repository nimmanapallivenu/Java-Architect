# 120 03  21+ Java Servlet interview Q&As   Java Success.com

## Table of Contents

- [Q1: What is a Servlet? Is a Servlet inherently multi-threaded?](#q1)
- [Q2: What is new in Servlet 3.x compared to Servlet 2.5?](#q2)
- [Q3: What is the structure of a war (i.e. W eb ARchive) file?](#q3)
- [Q4: HTTP is a stateless protocol, hence how will you maintain state? What are the pr](#q4)
- [Q5: Explain the life cycle methods of a Servlet?](#q5)
- [Q6: What is the dif ference between doGet () and doPost () or GET and POST?](#q6)
- [Q7: If you want a servlet to take the same action for both GET and POST request, wha](#q7)
- [Q8: What are the ServletContext and ServletConfig objects?](#q8)
- [Q9: What are the Servlet life cycle events?](#q9)
- [Q10: What is the dif ference between request parameters and request attributes?](#q10)
- [Q11: What is the dif ference between using getSession(true) and getSession(false) met](#q11)
- [Q12: How can you set a cookie and delete a cookie from within a Servlet?](#q12)
- [Q13: What is a RequestDispatcher? What object do you use to forward a request?](#q13)
- [Q14: What is the dif ference between forwarding a request and redirecting a request?](#q14)
- [Q15: What are the considerations for Servlet clustering?](#q15)
- [Q16: What is pre-initialization of a Servlet?](#q16)
- [Q17: How to perform I/O operations in a Servlet/JSP?](#q17)
- [Q18: How do you send a file to a browser from your Servlet? i.e. Download a file](#q18)
- [Q19: How do you send a file from a browser to your web application? i.e. Upload a fil](#q19)
- [Q20: If an object is stored in a session and subsequently you change the state of the](#q20)
- [Q21: What is a Servlet filter , and how does it work?](#q21)

---

## Q1: What is a Servlet? Is a Servlet inherently multi-threaded?

**Answer:**

A Servlet is a Java class that runs within a web container in an application server , servicing multiple client requests concurrently forwarded through the server and the web
container . The web browser establishes a socket connection to the host server in the URL, and sends the HTTP request. Servlets can forward requests to other servers and Servlets and
can also be used to balance load among several servers. Servlet 3.0 allowed asynchr onous r equest pr ocessing but only traditional I/O was permitted. In 3.1, asynchronous request
processing is possible with NIO (i.e. non blocking IO).
HttpServlet spawns a lightweight Java thread to handle each HTTP request. Single copy of a type of HttpServlet but N number of threads (thread sizes can be configured in an
application server). So, you need to code in a thread-safe manner .
Q. What are the dif ferent types of Servlets?
A.
GenericServlet HttpServlet
A GenericServlet has a service( ) method to handle requests.The HttpServlet extends GenericServlet and adds support for HTTP protocol based methods like doGet(),
doPost(), doHead() etc. All client requests are handled through the service() method. The service method
dispatches the request to an appropriate method like doGet(), doPost() etc to handle that request. HttpServlet
also has methods like doHead(), doPut(), doOptions(), doDelete(), and doT race().
Protocol independent. GenericServlet is for Servlets that might not use
HTTP (for example FTP service).Protocol dependent (i.e. HTTP).
Q. Which protocol is used to communicate between a browser and an HttpServlet?
A. A browser and a HttpServlet communicate using the HTTP protocol, which is a stateless request/response based protocol.
Q. What are the two objects an HttpServlet receives when it accepts a call from its client?
A. A “ServletRequest ”, which encapsulates client request from the client and the “ ServletResponse ”, which encapsulates the communication from the Servlet back to the client.
In addition to both HTTP request and response, HTTP headers are informational additions that convey both essential and non-essential information. For example: HTTP headers are
used to convey MIME (Multipurpose Internet Mail Extension) type of an HTTP request and also to set and retrieve cookies etc.
Q. What is a MIME type?
A. The “Content-T ype” request or response header is known as MIME type. Server sends the MIME type to client to inform the type of data (e.g. XML, HTML, JSON, CSV , etc) it is
sending, and client notifies the server its content type.Content -Type: text/html 
Set-Cookie :AV+USERKEY =AVSe5678f6c1tgfd ;expires =Monday , 4-Jul-2006 12:00:00; path=/;domain =lulu.com;
response .setContentT ype(“text/html”);
response .addCookie (myCookie );

---

## Q2: What is new in Servlet 3.x compared to Servlet 2.5?

**Answer:**

1. Servlets 3.0 have come up with a set of new Annotations for the declarations of Servlet Mappings, Init-Params, Listeners, and Filters to make the code more readable by making
the use of Deployment Descriptor (web.xml) absolutely optional.
package com.myapp ;
@WebServlet (
asyncSupported = false ,
name = "AccountingServlet" ,
urlPatterns = { "/acount" },
initParams = {
 @WebInitParam (name = "param1" , value = "value1" ),
 @WebInitParam (name = "param2" , value = "value2" )
}
public class AccountingServlet extends HttpServlet {
 //...
package com.myapp ;
@WebFilter (filterName ="countFilter" , urlPattern ={"/account/*" , "/transaction/*" })
public class CountFilter extends Filter {
 ...
}
package com.myapp ;

is better than declaring it verbosely in the web.xml as@WebListener
public class PaymentsListener extends ServletContextListener {
 //....
}
<web-app xmlns ="http://java.sun.com/xml/ns/javaee" version ="2.5" >
 <servlet >
 <servlet -name >accountingServlet </servlet -name >
 <servlet -class >com.myapp .AccountingServlet </servlet -class >
 <init-param >
 <param -name >param1 </param -name >
 <param -value >value1 </param -value >
 </init-param >
 <init-param >
 <param -name >param2 </param -name >
 <param -value >value2 </param -value >
 </init-param >
 </servlet >
 <servlet -mapping >
 <servlet -name > accountingServlet </servlet -name >
 <url-pattern >/account /*</url-pattern >
 </servlet -mapping >
 <filter >
 <filter -name >countFilter </filter -name >
 <filter -class >com.myapp .CountFilter </filter -class >
 </filter >
 <!-- Log for all URLs that use the "accountingServlet" -->
 <filter -mapping >
 <filter -name > countFilter </filter -name >
 <servlet -name > accountingServlet </servlet -name >
 </filter -mapping >
 <listener >
 <listener -class >com.myapp .PaymentsListener </listener -class >
 </listener >
</web-app>

2. Built-in support for File Upload with the @MultipartConfig annotation. Having this annotation on the top of a servlet indicates that the Request that the Servlet is expecting will
have multipart/form-data MIME type. In 2.x, you need to use a third-party framework like Struts or Spring MVC to upload files.
3. Server push and client pull are techniques used to initiating delivery of content from a server to the client. In 2.x, you need to perform a client pull, which get the browser to
request for an updated page in 10 seconds from the server .
Client pull Vs Server push
orresponse .setHeader (“Refresh ”, 10);

or in the section of the HTML
An example for the client pull would be playing an online game of chess. Y ou need to know if the opponent on another machine, in an another country has made his or her move by
periodically refreshing your page.
Server push technique can make use of the Asynchronous support from Servlet 3.x.respose .setHeader (“Refresh ”, “10;URL =http://localhost:8080/myCtxt/crm.do”);
<MET A HTTP -EQUIV =”Refresh ” CONTENT =”5; URL =http://localhost:8080/myCtxt/crm.do” />
@WebServlet (asyncSupported = true, value = "/AsyncServlet" )
public class AsyncServlet extends HttpServlet {
private static final long serialV ersionUID = 1L;
@EJB
private AsyncBean asyncBean ;
protected void doGet (HttpServletRequest request , HttpServletResponse response ) throws ServletException , IOException {
 AsyncContext asyncContext = request .startAsync ();
 asyncBean .doAsyncT ask(asyncContext );
}
@Stateless
public class AsyncBean {

4. W eb Fragments in Servlet 3.0 to modularize the web application. Applications can be broken down into various modules by packaging them into separate JARs and include these
archives into the main application war inside WEB-INF/lib folder . Each jar can use its own web application framework like Struts 2.0, Spring MVC, JSF , etc. These frameworks
come along with their own default Request Processors and/or a Controllers to be configured, and these can be configured in each jar file’ s web-fragment.xml , under the MET A-INF
folder . When the application is deployed, the server also scans JARs within WEB-INF/lib of the war under the MET A-INF directory and looks for web.xml fragments. If found, it will
load the configurations. The web fragments has a number of benefits such as
a) web.xml will be smaller in size and easier to maintain.
b) application structures can be modularized and the configurations can be encapsulated (e.g. for dif ferent frameworks like Struts2, Spring MVC, etc).
c) Loosely couples the modules from the main application, and web frameworks can be easily plugged in and out.
Define the module1-web.jar with MET A-INF/web-fragment.xml
Define the main web application (i.e. war) in the WEB-INF/web.xml by defining the Servlet mapping @Asynchronous
 public void doAsyncT ask(AsyncContext asyncContext ) throws IOException {
 asyncContext .getResponse ().getW riter().write ("chess move made" );
 }
}
<?xml version ="1.0" encoding ="UTF-8" ?>
<web-fragment xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xmlns ="http://java.sun.com/xml/ns/javaee"
 xmlns :web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
 xmlns :webfragment ="http://java.sun.com/xml/ns/javaee/web-fragment_3_0.xsd"
 xsi:schemaLocation ="http://java.sun.com/xml/ns/javaee
 http://java.sun.com/xml/ns/javaee/web-fragment_3_0.xsd"
 id="WebFragment_ID" version ="3.0" >
 <display -name >accounting </display -name >
 <name >mye</name >
 <servlet >
 <servlet -name >accountingServlet </servlet -name >
 <servlet -class >com.myapp .AccountingServlet </servlet -class >
 </servlet >
</web-fragment >

5. Servlet 3.0 has programmatic support for adding web components. So, Servlets, filters, and Servlet/Filter mappings can be added during the runtime.

---

## Q3: What is the structure of a war (i.e. W eb ARchive) file?

**Answer:**

The resources directly under W AR (aka the “document root”) are know as the public resources. Resources under WEB-INF are known as the private resources. Resources that
reside directly or under sub directories of the document root are directly accessible to the user through the URL. If you want to protect your W eb resources then hiding the files like
JSPs behind the WEB-INF directory can protect the files from direct access.<web-app xmlns :xsi="http://www .w3.or g/2001/XMLSchema-instance"
 xmlns ="http://java.sun.com/xml/ns/javaee"
 xmlns :web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
 xsi:schemaLocation ="http://java.sun.com/xml/ns/javaee
 http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
 id="WebApp_ID" version ="3.0" >
 <display -name >module1 </display -name >
 <servlet -mapping >
 <servlet -name >accountingServlet </servlet -name >
 <url-pattern >/account /*</url-pattern >
 </servlet -mapping >
</web-app>

Web ARchive (i.e. W AR) structure

---

## Q4: HTTP is a stateless protocol, hence how will you maintain state? What are the pros and cons of each approach?

**Answer:**

State mechanism Description and Pr os/Cons

HttpSession — There is no limit on the size of the session data kept.
— The performance is good.
— This is the preferred way of maintaining state. If we use the HTTP session with the application server ’s persistence mechanism (server
converts the session object into BLOB type and stores it in the Database) then the performance will be moderate to poor .
When using HttpSession mechanism you need to take care of the following points:
— Remove session explicitly when you no longer require it.
— Set the session timeout value.
— Your application server may serialize session objects after crossing a certain memory limit. This is expensive and af fects performance. So
decide carefully what you want to store in a session.
Hidden fields— There is no limit on size of the session data.
— May expose sensitive or private information to others (So not good for sensitive information).
— The performance is moderate.
URL rewriting— There is a limit on the size of the session data.
— Should not be used for sensitive or private information.
— The performance is moderate.
Cookies— There is a limit for cookie size.
— The browser may turn of f cookies.
— The performance is moderate.
The benefit of the cookies is that state information can be stored regardless of which server the client talks to and even if all servers go down.
Also, if required, state information can be retained across sessions.

---

## Q5: Explain the life cycle methods of a Servlet?

**Answer:**

The W eb container is responsible for managing the Servlet’ s life cycle. The W eb container creates an instance of the Servlet and then the container calls the init() method. At the
completion of the init() method the Servlet is in ready state to service requests from clients. The container calls the Servlet’ s service() method for handling each request by spawning a
new thread for each request from the W eb container ’s thread pool. Before destroying the instance the container will call the destroy() method. After destroy() the Servlet becomes the
potential candidate for garbage collection.
Q. What would be an ef fective use of the Servlet init() method?
A. One ef fective use of the Servlet init() method is the creation and caching of thread-safe resource acquisition mechanisms such as JDBC DataSources, EJB Homes, and W eb
Services SOAP Mapping Registry .

---

## Q6: What is the dif ference between doGet () and doPost () or GET and POST?

**Answer:**

Prefer using doPost() because it is secured and it can send much more information to the server .
GET or doGet() POST or doPost()
The request parameters are transmitted as a query string appended to the request. All the
parameters get appended to the URL in the address bar . Allows browser bookmarks but
not appropriate for transmitting private or sensitive information.The request parameters are passed with the body of the request.
More secured. In HTML you can specify as follows:

This is a security risk. In an HTML you can specify as follows:
GET was originally intended for static resource retrieval.POST was intended for form submits where the state of the model and database are
expected to change.
GET is not appropriate when lar ge amounts of input data are being transferred. Limited to
1024 characters.Since it sends information through a socket back to the server and it won’ t show up in the
URL address bar , it can send much more information to the server . Unlike doGet(), it is
not restricted to sending only textual data. It can also send binary data such as serialized
Java objects.

---

## Q7: If you want a servlet to take the same action for both GET and POST request, what would you do?

**Answer:**

You should have doGet call doPost, or vice versa.http://MyServer:8080/MyServlet?name=Paul
<form name =”SSS” method =”GET ” ><form name =”SSS” method =”POST ” >
protected void doPost (HttpServletRequest req, HttpServletResponse resp)
 throws ServletException , IOException
 
 ServletOutputStream out = resp.getOutputStream ();
 out.setContentT ype(“text/html”);
 out.println ("<html><h1>Output to Browser</h1>" );
 out.println ("<body>W ritten as html from a Servlet<body></html>" );
protected void doGet (HttpServletRequest req, HttpServletResponse resp)
 throws ServletException , IOException
 doPost (req, resp); //call doPost() for flow control logic.

Q. How do you create a deadlock situation in a Servlet?
A. Create a cyclic call by getting doGet(..) to invoke doPost(..), and doPost(..) to invoke doGet(..).

---

## Q8: What are the ServletContext and ServletConfig objects?

**Answer:**

The Servlet Engine uses both interfaces. The servlet engine implements the ServletConfig interface in order to pass configuration details from the deployment descriptor
(web.xml) to a servlet via its init() method.
ServletConfig ServletContext
The ServletConfig parameters are for a particular Servlet. The parameters are specified in
the web.xml (i.e. deployment descriptor) or via annotation. It is created after a Servlet is
instantiated and it is used to pass initialization information to the Servlet.The ServletContext parameters are specified for the entire W eb application. The
parameters are specified in the web.xml (i.e. deployment descriptor). Servlet context is
common to all Servlets. So all Servlets share information through ServletContext with
setAttribute(…) and getAttribute(….) methods.
A ServletContext object is initialized once while a web application is being deployed, and this object can be accessed from anywhere within the same web application.
Q.How can you invoke a JSP error page from a controller Servlet?
A. The following code demonstrates how an exception from a Servlet can be passed to an error JSP page.public class CRMServlet extends HttpServlet {
 //initializes the servlet
 public void init(ServletConfig config )throws ServletException {
 super .init(config );
 }
}
String strCfgPath = getServletConfig ().getInitParameter ("config" );
String strServletName = getServletConfig ().getServletName ();
String strClassName = getServletContext ().getAttribute ("GlobalClassName" );

---

## Q9: What are the Servlet life cycle events?

**Answer:**

Servlet lifecycle events work like the Swing events. Any listener interested in observing the ServletContext lifecycle can implement the ServletContextListener interface and in
the ServletContext attribute lifecycle can implement the ServletContextAttributesListener interface. The session listener model is similar to the ServletContext listener model (Refer
Servlet spec 2.3 or later). Servlet contexts’ and and sessions’ listener objects are notified when servlet contexts and sessions are initialized and destroyed, as well as when attributes
are added or removed from a context or session.
Real life examples of using a ServletContextListener
Example 1: A typical example where a ServletContextListener is used is in bootstarpping a spring applicationContet.xml file into a web application that is using RESTEasy for
RESTful web services.
Example 2: Another scenario that I came across recently where I had to provide my own implementation of the ServletContextListener when using yammer metrics to provide web
based monitoring.
You can also define your own custom listeners. The server creates an instance of the listener class to receive events and uses introspection to determine what listener interface (or
interfaces) the class implements.protected void doGet (HttpServletRequest req, HttpServletResponse resp) throws
 ServletException , IOException {
 try {
 //doSomething
 }
 catch (Exception ex) {
 req.setAttribute ("javax.servlet.ex" ,ex);//store the exception as a request attribute.
 ServletConfig sConfig = getServletConfig ();
 ServletContext sContext = sConfig .getServletContext ();
 sContext .getRequestDispatcher ("/jsp/ErrorPage.jsp" ).forward (req, resp);// forward the
 //request with the exception stored as an attribute to the “ErrorPage.jsp”.
 ex.printStackT race();
 }
@WebListener
public class MyJDBCConnectionManager implements ServletContextListener {
public void contextInitialized (ServletContextEvent event ) {
 Connection con = // create a connection
 event .getServletContext ().setAttribute ("con" , con);
}

---

## Q10: What is the dif ference between request parameters and request attributes?

**Answer:**

Request parameters Request attributes Parameters are form data that are sent in the request from the HTML page. These parameters are generally form fields in an HTML
form like:
Form data can be attached to the end of the URL as shown below for GET requests
or sent to the sever in the request body for POST requests. Sensitive form data should be sent as a POST request. Once a servlet gets a request, it can add additional attributes, then
forward the request of f to other servlets or JSPs for processing. Servlets and JSPs can communicate with each other by setting and getting attributes.
You can get them but cannot set them.public void contextDestroyed (ServletContextEvent e) {
 Connection con = (Connection ) e.getServletContext ().getAttribute ("con" );
 try { con.close (); } catch (SQLException ignored ) { } // close connection
}
<input type=”text” name =”param1 ” />
<input type=”text” name =”param2 ” />
http://MyServer:8080/MyServlet? param1=Peter&param2=Smith
request .setAttribute (“calc-value ”, new Float (7.0));
request .getAttribute (“calc-value ”);

You can both set the attribute and get the attribute. Y ou can also get and set the attributes in session and application scopes.
Q. What are the dif ferent scopes or places where a servlet can save data for its processing?
A. Data saved in a request-scope goes out of scope once a response has been sent back to the client (i.e. when the request is completed).
Data saved in a session-scope is available across multiple requests. Data saved in the session is destroyed when the session is destroyed (not when a request completes but spans
several requests).
Data saved in a ServletContext scope is shared by all servlets and JSPs in the context. The data stored in the servlet context is destroyed when the servlet context is destroyed.request .getParameter ("param1" );
request .getParameterNames ();
/save and get request-scoped value
request .setAttribute (“calc-value ”, new Float (7.0));
request .getAttribute (“calc-value ”);
/save and get session-scoped value
HttpSession session = request .getSession (false );
f(session != null) {
 session .setAttribute (“id”, “DX12345 ”);
 value = session .getAttribute (“id”);
}
/save and get an application-scoped value
getServletContext ().setAttribute (“application -value ”, “shopping -app”);

---

## Q11: What is the dif ference between using getSession(true) and getSession(false) methods?

**Answer:**

getSession(true): This method will check whether there is already a session exists for the user . If a session exists, it returns that session object. If a session does not already exist then
it creates a new session for the user .
getSession(false): This method will check whether there is already a session exists for the user . If a session exists, it returns that session object. If a session does not already exist then
it returns null.

---

## Q12: How can you set a cookie and delete a cookie from within a Servlet?

**Answer:**



---

## Q13: What is a RequestDispatcher? What object do you use to forward a request?

**Answer:**

A Servlet can obtain its RequestDispatcher object from its ServletContext.value = getServletContext ().getAttribute (“application -value ”);
/to add a cookie
Cookie myCookie = new Cookie (“aName ”, “aValue”);
esponse .addCookie (myCookie );
/to delete a cookie
myCookie .setValue(“aName ”, null);
myCookie .setMax (0);
myCookie .setPath (“/”);
esponse .addCookie (myCookie );
/inside the doGet() method
ServletContext sc = getServletContext ();
RequestDispatcher rd = sc.getRequestDispatcher (“/nextServlet ”);//relative path of the resource
/forwards the control to another servlet or JSP to generate response. This method allows one //servlet to do preliminary processing of a request and another resource to generate the
/response.

Q. What is the dif ference between the getRequestDispatcher(String path) method of “ServletRequest” interface and ServletContext interface?
A. javax.servlet. ServletRequest .getRequestDispatcher(String path) accepts path parameter of the servlet or JSP to be included or forwarded relative to the request of the calling
servlet. If the path begins with a “/” then it is interpreted as relative to current context root.
javax.servlet. ServletContext .getRequestDispatcher(String path) does not accept relative paths and all path must start with a “/” and are interpreted as relative to current context root.

---

## Q14: What is the dif ference between forwarding a request and redirecting a request?

**Answer:**

Both methods send you to a new resource like Servlet, JSP etc, but there is a key dif ference.
forward Vs sendRedirect
Forward action takes place within the server without the knowledge of the browser . Accepts relative path to the servlet or context root. So, there is no extra network trip.
sendRedir ect sends a header back to the browser , which contains the name of the resource to be redirected to. The browser will make a fresh request from this header information.
Need to provide absolute URL path. This has an overhead of extra remote trip but has the advantage of being able to refer to any resource on the same or dif ferent domain and also
allows book marking of the page.d.forward (request ,response );
/ or includes the content of the resource such as Servlet, JSP , HTML, Images etc into the
/ calling Servlet’ s response.
d.include (request , response );

---

## Q15: What are the considerations for Servlet clustering?

**Answer:**

The clustering promotes high availability and scalability . The considerations for servlet clustering are:
1) Objects stored in a session should be serializable to support in-memory replication of sessions. Also consider the overhead of serializing very lar ge objects. T est the performance
to make sure it is acceptable.
2) Design for idempotence . Failure of a request or impatient users clicking again can result in duplicate requests being submitted. So the Servlets should be able to tolerate duplicate
requests.
3) Avoid using instance and static variables in read and write mode because dif ferent instances may exist on dif ferent JVMs. Any state should be held in an external resource such as a
database.
4) Avoid storing values in a ServletContext. A ServletContext is not serializable and also the dif ferent instances may exist in dif ferent JVMs.
5) Avoid using java.io.* because the files may not exist on all backend machines. Instead use getResourceAsStream() .

---

## Q16: What is pre-initialization of a Servlet?

**Answer:**

By default the container does not initialize the Servlets as soon as it starts up. It initializes a Servlet when it receives a request for the first time for that Servlet. This is called
lazy loading . The servlet deployment descriptor (web.xml) defines the element, which can be configured to make the servlet container load and initialize the servlet as soon as it
starts up. The process of loading a servlet before any request comes in is called pre-loading or pre-initializing a servlet. W e can also specify the order in which the servlets are
initialized.

---

## Q17: How to perform I/O operations in a Servlet/JSP?

**Answer:**

Problem: Since web applications are deployed as W AR files on the application server ’s web container , the full path and relative paths to these files vary for each server .
Solution -1: You can configure the file paths in web.xml using tags or via annotation @W ebInitParam to retrieve file paths in your Servlets/JSPs.
Solution -2: You can overcome these configuration issues by using the features of java.lang.ClassLoader and javax.servlet.ServletContext classes. There are various ways of reading
a file using the ServletContext API methods such as getResour ce(String r esour ce), getResour ceAsStr eam(String r esour ce), getResour cePaths(String path) and getRealPath(String
path) . The getRealPath(String path) method translates virtual URL into real path.
Alternatively you can use the APIs from ClassLoader as follows. The file “products.xml” should be placed under WEB-INF/classes directory where all web application classes reside.<load-on-startup >2</load-on-startup >
/Get the file “products.xml” under the WEB-INF folder of your application as inputstream
nputStream is = config .getServletContext ().getResourceAsStream (“/products .xml”);

OR

---

## Q18: How do you send a file to a browser from your Servlet? i.e. Download a file

**Answer:**

Files can be downloaded from a web application by using the right combination of headers.
use Content-Disposition “ attachment ” to invoke “Save As” dialog and “ inline ” for displaying the file content on the browser without invoking the “Save As” dialog.

---

## Q19: How do you send a file from a browser to your web application? i.e. Upload a file

**Answer:**

There are better and more secured ways to upload your files instead of using the W eb. For example FTP , secure FTP etc. But if you need to do it via your web application then
your default encoding and GET methods are not suitable for file upload and a form containing file input fields must specify the encoding type “ multipart/form-data ” and the POST
method in the/Get the URL for the file and create a stream explicitly
URL url = config .getServletContext ().getResource (“/products .xml”);
BufferedReader br = new BufferedReader (new InputStreamReader (url.openStream ));
/use the context class loader
URL url = Thread .currentThread ().getContextClassLoader ().getResource (“products -out.xml”);
BufferedW riter bw = new BufferedW riter(new FileW riter(url.getFile ());
response .setContentT ype(“application /x-download ”);
response .setHeader (“Content -disposition ”, “attachment ;filename =” + fileName );

tag as shown below:
When the user clicks the “Upload” button, the client browser locates the local file and sends it to the server using HTTP POST . When it reaches your server , your implementing
servlet should process the POST data in order to extract the encoded file. In Servlet 3.0, you can use @MultipartConfig annotation.
If using Servlet 2.5, there are a number of libraries available such as Apache Commons File Upload, which is a small Java package that lets you obtain the content of the uploaded file
from the encoded form data. The API of this package is flexible enough to keep small files in memory while lar ge files are stored on disk in a “temp” directory . You can specify a size
threshold to determine when to keep in memory and when to write to disk.

---

## Q20: If an object is stored in a session and subsequently you change the state of the object, will this state change replicated to all the other distributed sessions in the cluster?

**Answer:**

No. Session replication is the term that is used when your current service state is being replicated across multiple application instances. Session replication occurs when we
replicate the information (i.e. session attributes) that are stored in your HttpSession. The container propagates the changes only when you call the setAttribute(……) method . So
mutating the objects in a session and then by-passing the setAttribute(………) will not replicate the state change.
Example: If you have an ArrayList in the session representing shopping cart objects and if you just call getAttribute(…) to retrieve the ArrayList and then add or change something
without calling the setAttribute(…) then the container may not know that you have added or changed something in the ArrayList. So, the session will not be replicated.

---

## Q21: What is a Servlet filter , and how does it work?

**Answer:**

A filter dynamically intercepts requests and responses to transform or use the information contained in the requests or responses but typically do not themselves create
responses. Filters can also be used to transform the response from the Servlet or JSP before sending it back to client. Filters improve re-usability by placing recurring tasks in the filter
as a reusable unit.<form enctype =”multipart /form -data” method =”POST ” action =”/MyServlet ”>
 <input type=”file” name =”products ” />
 <input type=”submit ” name =”Upload ” value =”upload ” />
</form >
@WebServlet (name = "StudentRegistrationUsn" , urlPatterns = {"/account/details" })
@MultipartConfig (maxFileSize = 10*1024 *1024 , maxRequestSize = 20*1024 *1024 ,f ileSizeThreshold = 5*1024 *1024 )
public class ActionRegistrationServlet extends HttpServlet {
 protected void doPost (HttpServletRequest request , HttpServletResponse response ) throws ServletException , IOException {
 //handle file upload
 }
}

Servlet Filter
A good way to think of Servlet filters is as a chain of steps that a request and response must go through before reaching a Servlet, JSP , or static resource such as an HTML page in a
Web application.The filters can be used for caching and compressing content, logging and auditing, image conversions (scaling up or down etc), authenticating incoming requests,
XSL transformation of XML content, localization of the request and the response, site hit count etc.
Design Pattern: Servlet filters use the slightly modified version of the chain of responsibility design pattern. Unlike the classic (only one object in the chain handle the request) chain
of responsibility where filters allow multiple objects (filters) in a chain to handle the request. In Servlet 3.0, you can use the @W ebFilter annotation instead of declaring it in the
web.xml file.
Here is an example that logs the time request/response times.<web-app>
 <filter >
 <filter -name >HitCounterFilter </filter -name >
 <filter -class >myPkg .HitCounterFilter </filter -class >
 </filter >
 <filter -mapping >
 <filter -name >HitCounterFilter </filter -name >
 <url-pattern >/usersection /*</url-pattern >
 </filter -mapping > 
 ...
</web-app>

mport javax .servlet .*;
@WebFilter (filterName ="timerFilter" , urlPattern ={"/account/*" , "/transaction/*" })
public class TimerFilter implements javax .servlet .Filter
{
 private FilterConfig filterConfig ;
 public void doFilter (ServletRequest request , ServletResponse response ,
 FilterChain chain )
 throws java.io.IOException , javax .servlet .ServletException
 {
 //on the way up the request chain
 long start = System .currentT imeMillis ();
 System .out.println ("Milliseconds in: " + start);
 chain .doFilter (request , response );
 //on the way down the response chain
 long end = System .currentT imeMillis ();
 System .out.println ("Milliseconds out: " + end);
 }
 public void init(final FilterConfig filterConfig )
 {
 this.filterConfig = filterConfig ;
 }
 public void destroy ()
 {
 filterConfig = null;
 }

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
