# 11( 04  7 JSP interview questions and answers   Java Success.com

## Table of Contents

- [Q1: What is a JSP? How does it dif fer from a Servlet?](#q1)
- [Q2: What do you understand by the term JSP translation phase or compilation phase?](#q2)
- [Q3: What are the dif ferent scope values in a JSP?](#q3)
- [Q4: What are implicit objects and list them?](#q4)
- [Q5: What are the enhancements made to the EL in JEE 7?](#q5)
- [Q6: What Is the dif ference between JSP and JSPF ?](#q6)
- [Q7: What are custom tags in JSPs?](#q7)

---

## Q1: What is a JSP? How does it dif fer from a Servlet?

**Answer:**

JSP stands for Java Server Pages. JSP technology extends the Servlet technology , which means anything you can do with a Servlet you can do with a JSP
as well.W riting out.println (…) statements using a Servlet is cumbersome and har d to maintain, especially if you need to send a long HTML page with little
dynamic code content. W orse still, every single change r equir es recompilation of your Servlet.
Both Servlets and JSPs are complementary technologies. Y ou can look at the JSP technology from an HTML designer ’s perspective as an extension to HTML
with embedded dynamic content and from a Java developer ’s as an extension of the Java Servlet technology . JSP is commonly used as the presentation layer for
combining HTML and Java code. While Java Servlet technology is capable of generating HTML with out.println(“…..”) statements, where “out” is a
PrintW riter. This process of embedding HTML code with escape characters is cumbersome and hard to maintain. The JSP technology solves this by providing a
level of abstraction so that the developer can use custom tags and action elements, which can speed up W eb development and are easier to maintain.

---

## Q2: What do you understand by the term JSP translation phase or compilation phase?

**Answer:**

As shown below in the figure the JSPs have a translation or a compilation process where the JSP engine translates and compiles a JSP file into a JSP
Servlet. The translated and compiled JSP Servlet moves to the execution phase (run time) where they can handle requests and send responses.
JSPs are translated to JSP Servlets before serving requests.

Unless explicitly compiled ahead of time, JSP files are compiled the first time they are accessed. On lar ge production sites, or in situations involving
complicated JSP files, compilation may cause unacceptable delays to users first accessing the JSP page. The JSPs can be compiled ahead of time (i.e.
precompiled) using application server tools/settings or by writing your own script.

---

## Q3: What are the dif ferent scope values in a JSP?

**Answer:**

Page, Request, Session, and Application.
Scope Object Comment
Page PageContext Available to the handling JSP page only .
Request Request Available to the handling JSP page or Servlet and forwarded JSP page or Servlet.
Session Session Available to any JSP Page or Servlet within the same session.
Application Application Available to all the JSP pages and Servlets within the same W eb Application.

---

## Q4: What are implicit objects and list them?

**Answer:**

Implicit objects are the objects that are available for the use in JSP documents without being declared first. These objects are parsed by the JSP engine and
inserted into the generated Servlet. The implicit objects are:
Implicit
objectScope Comment
request Request Refers to the current request from the client.
response Page Refers to the current response to the client.
pageContext Page Refers to the page’ s environment.
session Session Refers to the user ’s session.
application Application Same as ServletContext. Refers to the web application’ s environment.
out Page Refers to the outputstream.
config Page same as ServletConfig. Refers to the servlet’ s configuration.
page Page Refers to the page’ s Servlet instance.
exception Page xception created on this page. Used for error handling. Only available if it is an errorPage with the following directive:
<%@ page isErrorPage ="true" %>

The “exception” implicit object is not available for global error pages declared through web.xml. Y ou can retrieve the
java.lang.Throwable object as follows:

---

## Q5: What are the enhancements made to the EL in JEE 7?

**Answer:**

Java EE 7 is an overhaul of the Java Expression Language API known as Expression Language. The enhancements to the EL API includes support for
lambda expressions, static field and method access, as well as improvements for collection processing. Prior to Java EE 7, the Java Expression Language was a
tightly coupled component of the JSF and JSP APIs. The EL API now of fers developers the ability to invoke ad-hoc Java EL with “ ELPr ocessor “.

---

## Q6: What Is the dif ference between JSP and JSPF ?

**Answer:**

JSPF is JSP Fragment, which is a fragment included into another JSP page with the include directive, but not the include action. JSP Fragments can be
compared to server side includes. These fragments are not compiled on their own, however there are compiled along side the page in which its included.

---

## Q7: What are custom tags in JSPs?

**Answer:**

Custom tags allow a cleaner separation between the look-and-feel of your application and its logic, compared to the original scriptlet syntax of fered by JSP .<%= request .getAttribute ("javax.servlet.error .exception" ) %>
ELProcessor el = new ELProcessor ();
assert ((Integer )(el.eval("((x,y) -> x+y)(3, 2)" ))) == 5; // with Lambda expression
<%@include file="/WEB-INF/jspf/example.jspf" %>

There are two ways to implement a tag library: tag files , and tag classes . Tag files use a syntax that is nearly the same as JSP , but can be parameterized with
attributes in the tag.
Tag class example:
To create a custom tag class, we need three things:
1) Tag handler class: In this class we specify what our custom tag will do when it is used in a JSP page.
2) TLD file: T ag descriptor file where we will specify our tag name, tag handler class and tag attributes.
3) JSP page: A JSP page where we will be using our custom tag.
TLD File: This file should present at the location: WEB-INF/ and it should have a .tld extension.
message.tldpackage myapp .com;
mport javax .servlet .jsp.tagext .*;
mport javax .servlet .jsp.*;
mport java.io.*;
public class Message extends SimpleT agSupport {
 public void doTag() throws JspException , IOException {
 
 JspW riter out = getJspContext ().getOut ();
 out.println ("Sample custom tag......" );
 }

Using custom tag in JSP: Above we have created a custom tag named “Msg”. Here we will be using it.
Using tag files in JSP 2.2 or later container<taglib >
<tlib-version >1.0</tlib-version >
<jsp-version >2.0</jsp-version >
<short -name >My Custom Tag</short -name >
<tag>
 <name >Msg</name >
 <tag-class >myapp .com.Message </tag-class >
 <body -content >empty </body -content >
</tag>
</taglib >
<%@ taglib prefix ="msgprefix" uri="WEB-INF/message.tld" %>
<html>
<head >
 <title>Custom Tags in JSP Example </title>
</head >
<body >
 <msgprefix :Msg/>
</body >
</html>
<%@ tag body -content ="empty" %> 
"Sample custom tag......"

put the tag file in WEB-INF/tags/mytags/message.tag
<%@ taglib prefix ="msgprefix" tagdir ="/WEB-INF/tags/mytags" %>
<html>
<head >
 <title>Custom Tags in JSP Example </title>
</head >
<body >
 <msgprefix :message />
</body >
</html>

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
