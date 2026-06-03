# 118 06  Web design patterns MVC2, MVP, MVVM & MVW   Java Success.com

## Table of Contents

- [Q1: What’ s wrong with Servlets? What is a JSP? What is it used for? What do you kno](#q1)
- [Q2: Did JSPs make servlets obsolete?](#q2)
- [Q3: What is a model 0 pattern (i.e. model-less pattern) and why is it not recommende](#q3)
- [Q4: What is an MVP architecture, and how does it dif fer from MVC?](#q4)
- [Q5: What is an MVVM architecture, and how does it dif fer from MVP?](#q5)

---

## Q1: What’ s wrong with Servlets? What is a JSP? What is it used for? What do you know about model 0 (aka MVC0 ), model 1 (aka MVC1 ) and model 2 (aka
MVC2 ) patterns? In “model 2” architecture, if you set a request attribute in your JSP , would you be able to access it in your subsequent request within your
servlet code? How do you prevent multiple submits due to repeated “refresh button” clicks? What do you understand by the term JSP translation phase or
compilation phase?

**Answer:**

Writing out.println (…) statements using servlet is cumbersome and hard to maintain, especially if you need to send a long HTML page with little
dynamic code content. W orse still, every single change requires recompilation of your Servlet.

MVC-0

---

## Q2: Did JSPs make servlets obsolete?

**Answer:**

No. JSPs did not make Servlets obsolete. Both Servlets and JSPs are complementary technologies. Y ou can look at the JSP technology from an HTML

designer ’s perspective as an extension to HTML with embedded dynamic content and from a Java developer ’s as an extension of the Java Servlet technology .
JSP is commonly used as the presentation layer for combining HTML and Java code. While Java Servlet technology is capable of generating HTML with
out.println(“<html>….. </html>”) statements, where “out” is a PrintW riter. This process of embedding HTML code with escape characters is cumbersome and
hard to maintain. The JSP technology solves this by providing a level of abstraction so that the developer can use custom tags and action elements, which can
speed up W eb development and are easier to maintain.

---

## Q3: What is a model 0 pattern (i.e. model-less pattern) and why is it not recommended? What is a model-2 or MVC architecture?

**Answer:**

Problem: The example shown above is based on a model 0 (MVC0) pattern. The model 0 pattern is fine for a very basic Jsp page as shown above. But real web
applications would have business logic, data access logic etc, which would make the above code hard to read, dif ficult to maintain, dif ficult to refactor , and
untestable. It is also not recommended to embed business logic and data access logic in a JSP page since it is protocol dependent (i.e. HTTP protocol) and
makes it unable to be reused elsewhere like a wireless application using a W AP protocol, a standalone XML based messaging application etc.
Solution: You can refactor the processing code containing business logic and data access logic into Java classes, which adhered to certain standards. This
approach provides better testability , reuse and reduced the size of the JSP pages. This is known as the “model 1” pattern where JSPs retain the responsibility of a
controller , and view renderer with display logic but delegates the business processing to java classes known as Java Beans. The Java Beans are Java classes,
which adhere to following items:
1) Implement java.io.Serializable or java.io.Externalizable interface.
2) Provide a no-ar guments constructor .
3) Private properties must have corresponding getXXX/setXXX methods.

MVC1
The above model provides a great improvement from the model 0 or model-less pattern , but there are still some problems and limitations.
Problem: In the model 1 architecture the JSP page is alone responsible for processing the incoming request and replying back to the user . This architecture may
be suitable for simple applications, but complex applications will end up with significant amount of Java code embedded within your JSP page, especially when
there is significant amount of data processing to be performed. This is a problem not only for java developers due to design ugliness but also a problem for web
designers when you have lar ge amount of Java code in your JSP pages. In many cases, the page receiving the request is not the page, which renders the response
as an HTML output because decisions need to be made based on the submitted data to determine the most appropriate page to be displayed. This would require
your pages to be redirected (ie. sendRedirect (…)) or forwarded to each other resulting in a messy flow of control and design ugliness for the application. So,
why should you use a JSP page as a controller , which is mainly designed to be used as a template?
Solution: You can use the Model 2 architecture (MVC – Model, V iew, Controller architecture), which is a hybrid approach for serving dynamic content, since it
combines the use of both Servlets and JSPs. It takes advantage of the predominant strengths of both technologies where a Servlet is the tar get for submitting a
request and performing flow-control tasks and using JSPs to generate the presentation layer . As shown in the diagram below , the servlet acts as the controller
and is responsible for request processing and the creation of any beans or objects used by the JSP as well as deciding, which JSP page to forward or redirect the
request to (i.e. flow control) depending on the data submitted by the user . The JSP page is responsible for retrieving any objects or beans that may have been
previously created by the servlet, and as a template for rendering the view as a response to be sent to the user as an HTML.

MVC-2

---

## Q4: What is an MVP architecture, and how does it dif fer from MVC?

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

## Q5: What is an MVVM architecture, and how does it dif fer from MVP?

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



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
