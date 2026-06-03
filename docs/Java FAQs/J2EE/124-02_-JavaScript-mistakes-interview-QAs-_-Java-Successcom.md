# 124 02  JavaScript mistakes interview Q&As   Java Success.com

## Table of Contents

- [Q1: What are the common JavaScript errors or bad practices that you have noticed?](#q1)
- [Q2: What tools would you use to avoid above mentioned pitfalls?](#q2)
- [Q3: What tips would you give to someone requiring to perform computation intensive t](#q3)

---

## Q1: What are the common JavaScript errors or bad practices that you have noticed?

**Answer:**

1) Not having pr oper namespaces and not using AMD (i.e. Asynchronous Model Definition) API to modularize the Java code for improved maintainability .
2) Not using the var to declar e your variables. If you don’ t use “var”, your variable will become global. Y our code will work with global variables, but it can
create strange errors that are harder to debug and fix. It is also imperative to define proper namespaces and declare variables within the scope of that namespace.
3) Not understanding the differ ence between “==” operator and “===” operator .
== operator compare the values but it doesn’ t compare the data type of operands.
=== operator in JavaScript compare not only the value of operands, but also the data type. If the data type of operands is dif ferent, it will always return false.
4) Not de-r eferencing a variable once it has been used . Setting a variable to null once it has been used will allow the garbage collector of the js engine to
reclaim that object.
5) Not understanding the dif ference between innerT ext and innerHTML . The innerHTML gets the html code inside the element and innerT ext gets the text
inside the element. So, if you had
the innerT ext will only return “Some text” without the element “p”, and innerHTML will return
6) Not understanding what the implicit scope “ this” refers to. For example,<p> Some text </p>
<p> Some text </p>

Now , if you try
Why did it thr ow an err or?
The implicit “ this” points to the global Window object, and the W indow object does not have the function getT enPercentOfbalance( ).
The above two lines can be written with the JavaScript head object ‘window’ as shown below .function Account (balance ) {
his.balance = balance ;
his.getTenPercentOfbalance = function () {
 return balance * 0.10;
;
var mortgageAccount = new Account (10000.00 );
mortgageAccount .getTenPercentOfbalance (); // returns 1000.00
var tenPercentMethod = mortgageAccount .getTenPercentOfbalance ();
enPercentMethod (); // throws an error
var window .tenPercentMethod = window .mortgageAccount .getTenPercentOfbalance ();
window .tenPercentMethod (); // throws an error

Important : The value of this, passed to all functions, is based on the context in which the function is called at runtime.
You can fix this by:
7) Not understanding getting the function back versus invoking the function, especially when used in callback functions. The callback functions are not invoked
directly . They are either invoked asynchronously after a certain event like button click or after a certain timeout.
Now , if you do the following, you only get the function back.
But if you add ‘( )’ to it as shown below , you will be actually invoking the function.enPercentMethod .apply (mortgageAccount ); // now it uses this == mortgageAccount
function sayHello (){
 return "Hello caller" ;
}
var varFunction = sayHello ; // stores the function to the variable varFunction
setTimeout (sayHello , 1000 ) // can also pass it to other functions.
 // This is a callback function
 // Will call sayHello a second later .
window .load = sayHello ; // Can attach to objects. W ill call sayHello when the page loads
 // This is a callback function

So, the addition of paranthese to the right invokes the function. So, incorrectly assigning like shown below will callback the function immediately .
jQuery to the r escue with callbacks
So, it is a best practice to favor using proven JavaScript frameworks to avoid potential pitfalls.
8) Not understanding JavaScript scopes. Javascript only has global and function scopes , and does not have block scopes as in other languages like Java. In
JavaScript, functions are values that can be assigned to a variable, including arrays.
9 Not testing the JavaScript code for cross browser compatibility . Trying to reinvent the wheel by writing substandard functions as opposed to reusing functions
from proven frameworks and libraries.sayHello (); //invoke the function
setTimeout (sayHello (), 1000 ); // won't wait for a second
/invokes it straight a way without waiting for onclick event.
<input id="mybutton" onclick ="sayHello();return false;" type="button" value ="clickMe" />
setTimeout (sayHello , 1000 ); // waits for a second
/jQuery to the rescue
$('#mybutton' ).click (function (){
 return "Hello caller" ;
})

---

## Q2: What tools would you use to avoid above mentioned pitfalls?

**Answer:**

If you are writing Java Script code, it is worth using code quality tools like JSLint and JSHint to avoid any pitfalls.
It is also essential to use JavaScript testing frameworks like Jasmine , Selenium + W ebDriver , QUnit , and TestSwarm . QUnit is an easy-to-use, JavaScript test
suite that was developed by the jQuery project to test its code and plugins, but is capable of testing any generic JavaScript code. One of the challenges of
JavaScript rich application is testing it for cross browser compatibility . The primary goal of T estSwarm is to simplify the complicated, and time-consuming
process of running JavaScript test suites in multiple browsers. It provides all the tools necessary for creating a continuous integration work-flow for your
JavaScript rich application. Debugging JavaScripts can be a painful part of web development. There are handy browser plugins, built-ins and external tools to
make your life easier . Here are a few such tools.
— Cross-br owser (Firebug Lite, JS Shell, Fiddler , Blackbird Javascript Debug helper , NitobiBug, DOM Inspector (aka DOMi), W ireshark / Ethereal)
— Firefox (JavaScript Console, Firebug, V enkman, DOM Inspector , Web Developer Extension, T amper Data, Fasterfox, etc)
— Internet Explor er (JavaScript Console, Microsoft W indows Script Debugger , Microsoft Script Editor , Visual W eb Developer , Developer T oolbar , JScript
Profiler , JavaScript Memory Leak Detector)
— Opera (JavaScript Console, Developer Console, DOM Snapshot, etc)
— Safari (“Debug” menu, JavaScript Console, Drosera – W ebkit, etc)
— Google Chr ome (JavaScript Console and Developer T ools)

---

## Q3: What tips would you give to someone requiring to perform computation intensive task using JavaScript?

**Answer:**

Computation intensive JavaScript tasks, for example, in a loop can make a browser unresponsive. Here are some tips to consider .
1. Redesign the functionality by of floading the processing to a back end server .
2. The HTML 5 supports W eb W orker and it brings multithreading to JavaScript. Prior to W eb W orker , developers were creating asynchronous processing by
using techniques like setT imeout(), setInterval(), XMLHttpRequest, and event handlers. The W eb W orkers specification defines an API for spawning
background scripts in your web application. W eb W orkers allow you to do things like fire up long-running scripts to handle computationally intensive tasks, but
without blocking the UI or other scripts to handle user interactions.
3. If you are not on HTML 5 yet, put a wait inside the body of the loop so as to let the browser breath. Don’ t use sleep(5); Instead use setT imeout(..) function,
which uses the non-blocking I/O paradigm.

Note : The above code can be further improved with a queue, dynamic batch sizes, and eliminating the need for a for loop.for (var i = 0, len = items .length ; i < len; i++){
 setTimeout (function (){
 processItem (items [i])
 }, 5)
}

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
