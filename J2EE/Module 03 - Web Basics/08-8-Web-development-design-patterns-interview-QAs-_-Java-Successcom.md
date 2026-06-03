# 8 8 Web development design patterns interview Q&As   Java Success.com

## Table of Contents

- [Q1: What do you know about model 0, model 1 and model 2 MVC design patterns?](#q1)
- [Q2: How do you prevent multiple submits due to repeated “refresh button” clicks?](#q2)
- [Q3: What is a Front Controller pattern with command objects?](#q3)
- [Q4: Briefly discuss the following patterns — Composite view , View helper , Dispatch](#q4)
- [Q5: What is an MVP architecture, and how does it dif fer from MVC?](#q5)
- [Q6: What is an MVVM architecture, and how does it dif fer from MVP?](#q6)
- [Q7: Can you describe optimistic concurrency control (i.e. OCC) with respect to web a](#q7)
- [Q8: How do you get your servlet to stop timing out on a really long database query?](#q8)

---

## Q1: What do you know about model 0, model 1 and model 2 MVC design patterns?

**Answer:**

In the model 0 pattern, which is also known as the model-less pattern , business logic is embedded in the JSP pages. The model 0 pattern is fine for a very
basic JSP page, but real web applications would have business logic, data access logic etc, which would make the JSP code hard to read, dif ficult to maintain,
difficult to refactor , and untestable. It is also not recommended to embed business logic and data access logic in a JSP page since it is protocol dependent (i.e.
HTTP protocol) and makes it unable to be reused elsewhere like a wireless application using a W AP protocol, a standalone XML based messaging application,
etc.
You can refactor the processing code containing business logic and data access logic into Java classes, which adhered to certain standards. This approach
provides better testability , reuse and reduced the size of the JSP pages. This is known as the “ model 1 ” pattern where JSPs retain the responsibility of a
controller , and view renderer with display logic but delegates the business processing to java classes known as Java Beans. The Java Beans are Java classes,
which adhere to following items:
1) Implement java.io.Serializable or java.io.Externalizable interface.
2) Provide a no-ar guments constructor .
3) Private properties must have corresponding getXXX/setXXX methods.
Model-1 MVC
The above model provides a great improvement from the model 0 or model-less pattern, but there are still some problems and limitations.
Problem with model-1 : In the model 1 architecture the JSP page is alone responsible for processing the incoming request and replying back to the user . This
architecture may be suitable for simple applications, but complex applications will end up with significant amount of Java code embedded within your JSP page,
especially when there is a significant amount of data processing to be performed. This is a problem not only for Java developers due to design ugliness but also
a problem for web designers when you have lar ge amount of Java code in your JSP pages. In many cases, the page receiving the request is not the page, which
renders the response as an HTML output because decisions need to be made based on the submitted data to determine the most appropriate page to be displayed.

This would require your pages to be redirected (i.e. sendRedirect (…)) or forwarded to each other resulting in a messy flow of control and design ugliness for
the application. So, why should you use a JSP page as a controller , which is mainly designed to be used as a template for the view?
Solution: You can use the Model 2 ar chitectur e (MVC – Model, V iew, Controller architecture), which is a hybrid approach for serving dynamic content, since
it combines the use of both Servlets and JSPs. It takes advantage of the predominant strengths of both technologies where a Servlet is the tar get for submitting a
request and performing flow-control tasks and using JSPs to generate the presentation layer . As shown in the diagram below , the Servlet acts as the contr oller
and is responsible for request processing and the creation of any beans or objects used by the JSP as well as deciding, which JSP page to forward or redirect the
request to (i.e. flow control) depending on the data submitted by the user . The JSP page is r esponsible for retrieving any objects or beans that may have been
previously created by the Servlet, and as a template for r endering the view as a response to be sent to the user as an HTML.
MVC 2 design pattern

---

## Q2: How do you prevent multiple submits due to repeated “refresh button” clicks?

**Answer:**

Problem: Very often a user is completely unaware that a browser re-sends information to the server when a “refresh button” in Microsoft Internet Explorer
or a “reload button” in Netscape/Mozilla is clicked. Even if a browser warns user , a user cannot often understand the technical meaning of the warning. This
action can cause form data to be resubmitted, possibly with unexpected results such as duplicate/multiple purchases of a same item, attempting to delete the
previously deleted item from the database resulting in a SQLException being thrown. Non-idempotent methods are methods that cause the state to change. But
some operations like reading a list of products or customer details, etc are safe because they do not alter the state of the model and the database. These methods
are known as idempotent methods .
Solution-1: You can use a Post/Redir ect/Get (aka PRG) pattern . This pattern involves the following steps:
Step-1: First a user filled form is submitted to the server (i.e. a Servlet) using a “POST”. Servlet performs a business operation by updating the state in the
database and the business model.

Step-2: Servlet replies with redirect response (i.e. sendRedirect() operation as opposed to the forward() operation) for a view page.
Step-3: Browser loads a view using a “GET” where no user data is sent. This is usually a separate JSP page, which is safe from “multiple submits”. For e.g.
reading data back from a database to display , a confirmation page, etc.

PRG (Post-Redirect-Get) pattern to prevent duplicate page submits
Advantages: Separates the view from model updates and URLs can be bookmarked.
Disadvantage: Extra network round trip.
Solution-2 : The solution-1 has to make an extra network round trip. The synchr onizer token pattern can be applied in conjunction with request forward (i.e.
instead of redirect) to prevent multiple form submits with unexpected side ef fects without the extra round trip.
The basic idea of this pattern is to set a use once only token in a “session”, when a form is requested and the token is stored in the form as a hidden field. When
you submit the form the token in the request (i.e. due to hidden field) is compared with the token in the session. If tokens match, then reset the token in the
session to null or increment it to a dif ferent value and proceed with the model & database update. If you inadvertently resubmit the form by clicking the refresh
button, the request processing Servlet (i.e. PurchaseServlet) first tests for the presence of a valid token in the request parameter by comparing it with the one
stored in the session. Since the token was reset in the first submit, the token in the request (i.e 123) would not match with the token in the session (i.e. null or
124). Since the tokens do not match, an alternate course of action is taken like forwarding to an error .jsp page with an error message like “duplicate page
submission detected”.

Synchronizer token pattern to prevent duplicate form submits

---

## Q3: What is a Front Controller pattern with command objects?

**Answer:**

The model-2 MVC pattern can be further improved and simplified by using the Front Contr oller pattern with command objects . In a complex W eb site
there are many similar input control operations like security , internationalization, controlling and logging user ’s progress through the site, etc you need to
perform while handling a request. If these input control operations are scattered across multiple objects, much of these behaviors can end up duplicated resulting

in maintenance issues. The Front Controller pattern uses a single Servlet, which acts as initial point of contact for handling all the requests, including invoking
services such as security (authentication and authorization), logging, gathering user input data from the request, gathering data required by the view , etc by
delegating to the helper classes, and managing the choice of an appropriate view with the dispatcher classes. These helper and dispatcher classes are generally
instances of a command design pattern and therefore usually termed as command objects.
MVC-2 with Front controller and command objects
The popular MVC 2 frameworks like Struts, Spring MVC, etc use the Front Contr oller pattern , where a centralized single Servlet is used for channeling all
requests and creating instances of command classes for processing user requests.

---

## Q4: Briefly discuss the following patterns — Composite view , View helper , Dispatcher view and Service to worker?

**Answer:**

Composite V iew: Creates an aggregate view from atomic sub-views. The Composite view entirely focuses on the view . The view is typically a JSP page,
which has the HTML and JSP T ags. The JSP display pages mostly have a side bar , header , footer and main content area. These are the sub-views of the view .
The sub-views can be either static or dynamic. The best practice is to have these sub-views as separate JSP pages and include them in the whole view . This will
enable reuse of JSP sub-views and improves maintainability by having to change them at one place only .

Composite V iews
In JSF , you can create reusable composite components .
View Helper: When processing logic is embedded inside the controller or view it causes code duplication in all the pages. This causes maintenance problems,
as any change to piece of logic has to be done in all the views. In the view helper pattern the view delegates its processing responsibilities to its helper classes.
The helper classes JavaBeans, T ags, etc.
Service to W orker and Dispatcher V iew: These two patterns are a combination of Front Controller and V iew Helper patterns with a dispatcher component.
One of the responsibilities of a Front Controller is choosing a view and dispatching the request to an appropriate view . This behavior can be partitioned into a
separate component known as a dispatcher . But these two patterns dif fer in the way they suggest dif ferent division of responsibility among the components.
Service to W orker combines the front controller and dispatcher , with views and view helpersto handle client requests and dynamically prepares the response.
Controllers delegate the content retrieval to the view helpers, which populates the intermediate model content for the view . Dispatcher is responsible for the
view management and view navigation. This pattern promotes more up-front work by the front contr oller and dispatcher for the authentication, authorization,
content retrieval, validation, view management and navigation.
Dispatcher V iew pattern is structurally similar to the service to worker , but the emphasis is on a dif ferent usage pattern. This combines the Front controller and
the dispatcher with the view helpers but the contr oller does not delegate content r etrieval to view helpers because this activity is deferr ed to view
processing . Dispatcher is responsible for the view management and view navigation. This pattern promotes lightweight front controller and dispatcher with
minimum functionality and most of the work is done by the view .

---

## Q5: What is an MVP architecture, and how does it dif fer from MVC?

**Answer:**

MVP stands for “ Model View Presenter”. MVP is derived from MVC, where a “Presenter” assumes the role of a middle man, and a view is responsible for
handing the UI events like button click, etc which used to be the controller ’s job in MVC. The “Model” in MVP is strictly a domain model.

MVP
Presenter will have a 2 way communication with the view (i.e. Presenter <---> View) whereas in MVC a controller has one way communication with the view
(i.e. Controller –> View). The presenter takes care of the user tasks by communicating with the view by talking to an interface implemented by the view . At
times, a view communicates with the presenter by directly calling functions on an instance of the presenter . There will be a single presenter for each view ,
whereas in MVC a single controller will manage multiple views. This means that the Controller and the V iews are more ‘tightly coupled’.
Now a days, JavaScript based W eb frameworks like Backbone.js, AngularJS, Ember .JS, Knockout, etc are popular and they make use of the MVW ( Model
View Whatever) design patterns. “Whatever” means whatever works for you like MVC, MVP , MVVM ( Model View ViewModel), etc.
The main goal with MVP is decoupling of dif ferent aspects in the code. A JavaScript based MVP architecture will have:
1) EventHandling = Presenter
2) DOM Manipulation = View
3) AJAX calls = Model

---

## Q6: What is an MVVM architecture, and how does it dif fer from MVP?

**Answer:**

MVVM stands for Model V iew ViewModel. Popular JavaScript based frameworks like angularjs and knockoutjs use this pattern with so called 2 way

binding.
MVVM
The V iewModel will have 2 way communication with the view , which means the fields in a view model usually match up more closely with the view than with
the model. There will be a single V iewModel for each view .
View binds directly to the V iewModel. Because of the binding, changes in the view are automatically reflected in the V iewModel and changes in the
ViewModel are automatically reflected in the view .
So, don’ t get too bogged down with all these dif ferent patterns, and implement “ Whatever ” works for you with MVW . The key is to separate concerns without
tightly coupling them. Your goal is not to make an MVP , MVVM, or MVC system. Your goal is to separate the view , the model, and the logic that governs both
of them. Knowing the semantics help you in job interviews and understanding the frameworks a bit better .

---

## Q7: Can you describe optimistic concurrency control (i.e. OCC) with respect to web applications

**Answer:**

Step 1 : User A opens a Product catalog page, and makes some changes.
Step 2 : User B opens the same Product catalog page, and makes some changes.
Step 3 : Both users submit their changes the same time without being unaware of each others changes.
Step 4 : Only one of the users change should go through (i.e. commit), and the other user should get an error message indicating “Concurrent modification
detected to Product catalog with id 123, please refresh the page to view the update content, and retry .”
This can be accomplished on the back-end by having a verison flag or a timestamp column to the database table (e.g. Product) to detect concurrent updates.
ORM tools like Hibernate supports “optimistic locking” out of the box with the help of a version or timestamp column to the underlying database tables.
In theory , the timestamp is a little less safe than a version number . This is because a timestamp can, in theory , be updated by two transactions at the exact same
time (depends on the precision of the database) and allow both to update violating the optimistic lock. It can also be ar gued that timestamps also take up more
space in the database.

---

## Q8: How do you get your servlet to stop timing out on a really long database query?

**Answer:**

There are situations despite how much database tuning ef fort you put into a project, there might be complex queries or a batch process initiated via a
Servlet, which might take several minutes to execute. The issue is that if you call a long query from a Servlet or JSP , the browser may time out before the call
completes. When this happens, the user will not see the results of their request.
The solution is to use an asynchronous Servlet.
@WebServlet (asyncSupported = true, value = "/UnblockingServlet" )
public class UnblockingServlet extends HttpServlet {
protected void doGet (HttpServletRequest request , HttpServletResponse response ) throws ServletException , IOException {
 //doGet returns without blocking after delegating the processing to AsyncW ork class
 //The context of the request is stored using an AsyncContext from request.startAsync()
 AsyncW ork.add(request .startAsync ());
}

public class AsyncW ork implements ServletContextListener {
private static final BlockingQueue queue = new LinkedBlockingQueue ();
private volatile Thread thread ;
public static void add(AsyncContext c) {
 queue .add(c);
}
@Override
public void contextInitialized (ServletContextEvent servletContextEvent ) {
 thread = new Thread (new Runnable () {//run in a separate thread
 @Override
 public void run() {
 while (true) {
 try {
 Thread .sleep (5000 );//emulate long running database operation
 AsyncContext context ;
 while ((context = queue .poll()) != null) {
 try {
 //all the blocked tasks in the queue are processed by a single worker thread
 //by constructing and sending the responses.
 ServletResponse response = context .getResponse ();
 response .setContentT ype("text/plain" );
 PrintW riter out = response .getW riter();
 out.printf ("Thread %s completed the task" , Thread .currentThread ().getName ());
 out.flush ();
 } catch (Exception e) {
 throw new RuntimeException (e.getMessage (), e);
 } finally {
 context .complete ();
 }
 }
 } catch (InterruptedException e) {
 return ;
 }
 }
 }
 });
 thread .start();
}
@Override

Instead of blocking hundreds of web container threads to wait for the long running queries to finish, the blocked tasks are queued by batching the similar groups
together and processing the requests in a single thread.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »public void contextDestroyed (ServletContextEvent servletContextEvent ) {
 thread .interrupt ();
}

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
