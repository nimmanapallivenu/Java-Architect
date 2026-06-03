# 113Client side & Server side view technologies Q&As   Java Success.com

## Table of Contents

- [Q1: What do you understand by client side and server side view technologies?](#q1)
- [Q2: Which view technology would you use?](#q2)
- [Q3: What are the pros & cons of both server & client side technologies?](#q3)
- [Q4: How will you go about building client side view technology with server side micr](#q4)

---

## Q1: What do you understand by client side and server side view technologies?

**Answer:**

There are client side & server side view technologies available.
Server side view technologies
In server -side view technology the most of the page rendering happens on the server side. The server side view technologies are JSPs, Thymeleaf, Facelets for
JSF, Apache velocity , Sitemesh, etc.
Generates the HTML on the server , returns the generated HTML code to the browser . So these technologies are page centric as the browser will be requesting
for dif ferent pages.
Server side templating
Client side view technologies
In client-side view technology the most of the page rendering happens on the client side. The client side templating libraries based on JavaScript based
frameworks like angularjs, reactjs, backbone, ember , etc
The client side view technologies are based on a rich single page application (SPA) where the page rendering happens on the browser (i.e. client side). Client
side will be making RESTful web service calls via ajax requests to the back end server . On the server -side RESTFul web services implemented with say Spring-
MVC will be processing the ajax request and responding with JSON data to the browser , and the client side technology will be rendering or refreshing portions
of a rich page.

Client side templating

---

## Q2: Which view technology would you use?

**Answer:**

If you want a rich web application with lots of user interaction and page state changes, then client side technology like angualjs might be the way to go.
If you want page with simple and finite number of interactions then server side technology like JSP or Thymeleaf might be the way to go.

---

## Q3: What are the pros & cons of both server & client side technologies?

**Answer:**

Server Side T emplating
Pros:
– More search engine friendly .
– Entire response message can be cached.
– Can work without JavaScript
Cons:
– Harder to mix and match dif ferent server side technologies. For example, retrieve data from both Java and Ruby based services
– Harder to send content for multiple devices. E.g. browser , mobile devices, etc.
Client Side T emplating

Pros:
– Faster development and prototyping
– Serves JSON data to multiple devices.
– The responses are smaller as JSON data is less verbose.
– Templates and JSON data can be cached.
– Can work with multiple server side technologies like .Net, JEE, Ruby , etc
Cons:
– Less search engine friendly
– Older browsers may not work properly
– Cross browser compatibility testing is required

---

## Q4: How will you go about building client side view technology with server side micro-services for data in Java?

**Answer:**

Spring boot for microservices and angularjs as the client side templating.
WebJars allow you to manage client-side assets such as AngularJS, Bootstrap (i.e CSS), jQuery (i.e. DOM manipulation), etc and include them in your project
as Java Archive (JAR) via Maven dependencies.
You can kick start the Spring boot application via “Spring Initializr” site http://start.spring.io/ by choosing “ Web” and “ Thymeleaf ” as dependencies.
Once this is done you start creating the Spring MVC controller to service the requests. The thymeleaf based html page with angularjs tags will be sent back to
client, which will be making ajax calls back to the Spring MVC web services.<dependency >
<groupId >org.webjars </groupId >
<artifactId >angularjs </artifactId >
<version >1.3.8 </version >
</dependency >
<dependency >
<groupId >org.webjars </groupId >
<artifactId >bootstrap </artifactId >
<version >3.3.1 </version >
</dependency >

<!DOCTYPE html>
<html xmlns :th="http://www .thymeleaf.or g" lang="en">
<head >
 <meta charset ="utf-8" />
 <link href="http://cdn.jsdelivr .net/webjars/bootstrap/3.3.1/css/bootstrap.min.css"
 th:href="@{/webjars/bootstrap/3.3.1/css/bootstrap.min.css}" rel="stylesheet" media ="screen" />
 <link href="../css/app.css" th:href="@{/css/app.css}" rel="stylesheet" media ="screen" />
</head >
<body ng-app="linkApp" >
<div ng-controller ="UserController" >
 <div class ="center input-group" >
 <input type="text" ng-model ="userDetails" class ="form-control" id="user"
 placeholder ="Enter user details" />
 <span class ="input-group-btn" >
 <button class ="btn btn-default" type="button" ng-click ="login(userDetails)" >Login </button >
 </span>
 </div>
 <div id="result" ng-show ="user" >
 You are logged in as
 <a ng-href="{{user .id}}" >{{user.name }}</a>
 </div>
</div>
<script src="https://cdn.jsdelivr .net/webjars/angularjs/1.3.8/angular .min.js"
 th:src="@{/webjars/angularjs/1.3.8/angular .min.js}" ></script>
<script type="text/javascript" src="../js/app.js"
 th:src="@{/js/app.js}" ></script>
<script type="text/javascript" src="../js/UserController .js"
 th:src="@{/js/UserController .js}"></script>
</body >
</html>

Also look at JHipster https://jhipster .github.io/ for Spring Boot + Angular W eb applications and Spring .

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
