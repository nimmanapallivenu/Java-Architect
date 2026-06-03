# Java vs JavaScript

> **Module**: JavaScript  
> **Topic**: Java vs JavaScript

---

## 📋 Table of Contents



- [Q1: What is the difference between Java and JavaScript?](#q1)
- [Q2: Does JSE provide support for JavaScript?](#q2)
- [Q3: Java has packages to or ganize your code. How would you or ganize your code in J](#q3)

---

## Q1: What is the difference between Java and JavaScript?

**Answer:**

Don’ t be fooled by the term Java in both. Both are quite differ ent technologies. The key differences can be summarized as follows:
1) JavaScript variables ar e dynamically typed, whereas the Java variables are statically typed.
2) In JavaScript properties and methods ar e dynamically added, whereas Java uses a template called a class. The myV ar3 empty object dynamically adds
properties and a method.
3) JavaScript function can take variable arguments. You can call the function shown below as myFunction( ), myFunction(20), or myFunction(20,5).var myV ar1 = "Hello"; //string type
var myV ar2 = 5; //number type
var myV ar3 = new Object ( ); //empty object type
var myV ar4 = {}; //empty object type -- JSON (JavaScript Object Notation) style.
var myV ar3 = new Object ( );
myV ar3.firstName = "John"; // add a property to object
myV ar3.lastName = "Samuel"; // add a property to object
/ add a method
myV ar3.someFunction = function ( ) {
 document .write (this.firstName + " " + this.lastName );
}

JavaScript has an implicit keyword known as the “ arguments “, which holds all the passed arguments. It also has a “length” property as in arguments.length to
display the number of arguments. T echnically an “arguments” is not an array as it does not have the methods like push, pop, or split that an array has. Here is an
example.
4) JavaScript objects are basically like name/value pairs stor ed in a HashMap with string key and object values. For example, a JavaScript object is
represented in JSON style as shown below .function myFunction ( value ) {
 //.…. do something here
}
myFunction (5,10,15,20);
function myFunction (value ) {
 //value is 5;
 //arguments[0] is 5
 //arguments[1] is 10
 //arguments[2] is 15
 //arguments[3] is 20
 //arguments.length is 4
}
var personObj = {
 firstName : "John" ,
 lastName : "Smith" ,
 age: 25,

You can invoke the methods as shown below
5) JavaScript functions ar e objects as well. Like objects, the functions can be stored to a variable, passed as arguments, nested within each other, etc. In
the above example, nameless functions are attached to variables “printFullName” and “printAge” and invoked via these variables. A function that is attached to
an object via a variable is known as a “ method “. So, printFullName and printAge are methods.
In the example shown below, technically, what is done with the “add” and “sum” functions is that we have created a new function object and attached them to
the variables “add” and sum. As you can see in the example below, the “add” variable is assigned to variable “demo”, and the function is invoked via demo(2,5)
within the “sum” function. printFullName : function () { //function
 document .write (this.firstName + " " + this.lastName );
 } ,
 printAge : function () { //function
 document .write ("My age is: " + this.age);
 } 
 }
personObj .printFullName ();
personObj .printAge ();
function add(val1, val2) {
 var result = val1 + val2;
 alert("The result is:" + result );
 return result ;
var demo = add;

Now the above temp.js under tutorial/js folder can be invoked from an HTML file under tutorial/html as shown below .
6) Now we know that functions in JavaScript are objects, and can be passed around. Every function in JavaScript also has a number of attached (or implicit)
methods including toString( ), call( ), and apply( ) .
a) toString() implicit method examplefunction sum() {
 var output = demo (5, 2);
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 T ransitional//EN" "http://www .w3.or g/TR/html4/loose.dtd" >
<html>
<head >
<meta http-equiv ="Content-T ype" content ="text/html; charset=UTF-8" >
<script language ="javascript" type="text/javascript" src="../js/temp.js" >
</script>
<title>Insert title here</title>
</head >
<body >
 <form id="evaluate1" >
 <input type="button" value ="evaluate" onclick ="sum()" /> 
 <input type="button" value ="evaluate2" onclick ="demo(3,2)" /> 
 <input type="button" value ="evaluate3" onclick ="add(2,2)" /> 
 </form >
</body >
</html>

b) In JavaScript, functions can be invoked 5 dif ferent ways.
1) function_name(param1, param2, etc); //”this” refers to global object like window .
2) obj1.function_name(param1,param2,etc); //”this” refers to obj1.
3) new Object(); // The constructor .
4) function_name.call(objRef, param1); // function object implicit method
5) function_name.apply(objRef, params[parama1,param2, etc]); // function object implicit method
So, why use function_name. call(…) or function_name. apply ( … ) as opposed to just function_name( … )? Let’ s look at this with some examples.function add(val1, val2) {
 var result = val1 + val2;
 alert("Result is:" + result );
 return result ;
var printAdd = add.toString (); //converts the "add" function to string.
function demo () { 
 alert(printAdd ); //alerts the whole source code of the "add" function
var x = 1; //global variable x;
var obj1 = {x:3}; //obj1 variable x
var obj2 = {x:9}; //obj2 variable x
function function_name (message ) {
 alert(message + this.x) ;
function_name ("The number is " ); //alerts the global x --> The number is 1
/the first argument is the obj reference on which to invoke the function, and the
/the second argument is the argument to the function call
function_name .call(obj1, "The number is " ); //alerts the obj1's x --> The number is 3

The purpose is of call and apply methods are to invoke the function for any object without being bound to an instance of the this object. In the above example,
the this object is the global object with the x value of 1. In a function called directly without an explicit owner object, like function_name(), causes the value of
this to be the default object ( window in the browser). The call and apply methods allow you to pass your own object to be used as the “ this” reference. In the
above example, the obj1 and obj2 were used as “ this” reference.
7) JavaScript variables need to be tr eated like r ecords stor ed in a HasMap and r eferenced by name, and not by memory address or pass-by-reference as in
Java. The following code snippet demonstrates this.
8) Java does not support closure till version 8. A closur e is a function plus a binding environment. closures can be passed downwards (as parameters) or
returned upwards (as return values). This allows the function to refer to variables of its environment, even if the surrounding code is no longer active. JavaScript
supports closure.
In JavaScript a closure is created every time you create a function within a function. When using a closure, you will have access to all the variables in the
enclosing (i.e. the parent) function.function_name .call(obj2, "The number is " ); //alerts the obj2's x --> The number is 5
/the first argument is the obj reference on which to invoke the function, and
/the second argument is the argument to the function call as an array
function_name .apply (obj1, ["The number is " ]); //alerts the obj1's x --> The number is 3
function_name .apply (obj2, ["The number is " ]); //alerts the obj2's x --> The number is 5
var x = function () { alert("X"); }
var y = x;
x = function () { alert("Y"); };
y(); // alerts "X" and NOT "Y"
x();

---

## Q2: Does JSE provide support for JavaScript?

**Answer:**

Yes. Until Java SE 7, JDKs shipped with a JavaScript scripting engine based on Mozilla Rhino. Java SE 8 will instead ship with a new engine called Oracle
Nashorn, which has a bin/jjs command-line tool to get started with JavaScript. For example, learnjs.jsvar calculate = function (x) { 
 var myconst = 2;
 return function (y) {
 return x + y + myconst; // has visibility to parent variable 'x' and myconst
 };
var plus5 = calculate (5); //plus5 is now a closure
alert(plus5 (3)); //returns 10 i.e. x=5, y=3, myconst=2
alert(plus5 (7)); //returns 14 i.e x=5, y=7, myconst=2
alert(plus5 (10)); //returns 17 i.e x=5, y=10, myconst=2
var hello = function () {
print ("Start learning JavaScript!" );
};
hello ();
$ jjs learnjs .js

---

## Q3: Java has packages to or ganize your code. How would you or ganize your code in JavaScript?

**Answer:**

The concept of namespaces does not exist in JavaScript. T o add insult to injury, everything you create in JavaScript is by default global. Now obviously ,
this is a recipe for disaster. In JavaScript, you can use a number of techniques to modularize your code.
1) Nested objects acting as name spaces
The above pattern is fairly simple to avoid name collisions, but useful only for smaller projects.
2) Creating a general purpose namespace method that allow us to create namespaces.
3) Using a JavaScript library like AMD (Asynchronous Model Definition) API and Requir eJS to modularize your JavaScript files.
Option 3 is recommended. Beware that JavaScript is very powerful, but not properly applying best practices and patterns can lead to maintenance nightmare.
Use of proper name spacing pattern or API like AMD is very important.var MYAPPLICA TION = {
 MODEL : {
 product : function (cost) {
 this.cost = cost;
 this.getCost = function (){
 return this.cost;
 };
 }
 },
 LOGIC : {
 calculateGST : function (baseCost ) {
 return baseCost * 1.10;
 },
 performCalc : function () {
 var p = new MYAPPLICA TION .MODEL .product (200);
 alert(this.calculateGST (p.getCost ()));
 }
 }

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03