# JavaScript Variable Scopes

> **Module**: JavaScript  
> **Topic**: JavaScript Variable Scopes

---

## 📋 Table of Contents



- [Q1: Can you list which variables are accessible without any errors in locations mark](#q1)
- [Q2: Is there a way to list all the global variables defined in the code?](#q2)
- [Q3: Is there a “block level” scope in JavaScript like in other languages like Java? ](#q3)
- [Q4: Are JavaScript implicit variable “ this” referring to the context and “scope” th](#q4)
- [Q5: What do you think will be the output of the following code? // 3 inside block
 }](#q5)
- [Q6: What is the difference between the .call(….) and .apply(…) methods?](#q6)
- [Q7: What is the difference between undefined and null?](#q7)
- [Q8: What is the difference between “==” and “==” in JavaScript?](#q8)
- [Q9: How does “===” dif fer from “==” when checking for null or undefined?](#q9)
- [Q10: What is the purpose of “use strict” directive in JavaScript?](#q10)

---

## Q1: Can you list which variables are accessible without any errors in locations marked //1, //2, //3, and //4 in the code shown below?

**Answer:**

It is important to understand global variables and functional variables.
//1
1. var1 is accessible as it is defined in a global context outside the function. The “this” variable points to the window object of the browser window. Even if it is defined with “var” keyword.
2. var2 is locally defined within the function a(), and is accessible.
3. var3 is accessible as it is defined within function a(), but var3 is globally accessible outside as well as it is defined without the “ var” keyword. var3 is a global variable. This is bad.
4. The block variable “ var5 ” is accessible as well. If it were defined with the newly introduced “set” keyword for the block only scope, it would not have been accessible.var var1 = "I am var1"
function a() {
 var var2 = "I am var2"
 var3 = "I am var3"
 function b() {
 var var4 = "I am var4"
 // 2 - inside function b()
 }
 if(var2){
 var var5 = "I am var5"
 // 3 inside block
 }
 // 1 inside function a()
 b()//invoking function b()
a(); //invoking function a()
/ 4 outside

//2
In this position the accessible variables are
1. global varibles var1 and var3.
2. functional variables defined within “function b()” var4
3. any outer functional variables encclosing “function b()”, in this case “funaction a()” is enclosing “function b()”, hence var2 is accessible as well. But function a() can’ t access any variables inside
“function b()”.
//3
is a block scope. This is a “if” block. In this position the accessible variables are
1. global varibles var1 and var3.
2. functional variables defined within “function a()”, i.e. var2 and var5.
//4
Only global variables var1 and var3 are accessible.

JavaScript variable scopes

---

## Q2: Is there a way to list all the global variables defined in the code?

**Answer:**

Yes.

Output when run on Google Chr ome console with F12 or Ctrl+Shift+J or Mor e Tools -> JavaScript console.
JavaScript Global varaibles

---

## Q3: Is there a “block level” scope in JavaScript like in other languages like Java? For example, In Java, if you define a varaible within an “if” block, it is not accessible outside that block.

**Answer:**

The new “ECMAScript 2015 (ES6)” has introduced a “ let” keyword. let allows you to declare variables that are limited in scope to the block, statement, or expression on which it is used. This is
unlike the var keyword, which defines a variable globally, or locally to an entire function regardless of block scope. So, in the above code, if you had definedObject .keys(window );
f(var2){
 let var5 = "I am var5"

The var5 would have been accessible only in //3. The //1 can’ t see it when defined with “let”. This keyword “let” is not still supported by the browsers at the time of writing. The support may be provided
in the future.

---

## Q4: Are JavaScript implicit variable “ this” referring to the context and “scope” the same thing

**Answer:**

No. Scope is function-based, and context is object-based. In other words, scope pertains to the variable access of a function when it is invoked and is unique to each invocation. Context is always the
value of the this keyword which is a reference to the object that “owns” the currently executing code.
If you take the above example, the code runs in the “window” object context.
JavaScript Context

---

## Q5: What do you think will be the output of the following code? // 3 inside block
 }

**Answer:**

An alternative approach to calling a function is with the the “call(object)” method. It takes the object to call the function on as an argument.
In the above snippet “this” refers to the object “window”. In other words, the context is “window”. The “function a()” is invoked on object “arrayObject” as the new context. Hence, within the “function
a()” the implcit varible “this” points to the “arrayObject”. Even within “function b()”, the context is “arrayObject” as it was invoked with “b.call(arrayObject)”. It is the same result if you did call
“b.call(” again ” + this) “. But if you had called with just “b()”, inside “function b()”, the context would be a “object window”.var arrayObject = ["Java", "JEE" ]
var var1 = "I am var1"
function a() {
 var var2 = "I am var2"
 var3 = "I am var3"
 function b() {
 var var4 = "I am var4"
 //2 - inside function b()
 console .log("inside b(): " + this);
 }
 if(var2){
 var var5 = "I am var5"
 //3 inside block
 }
 //1 inside function a()
 console .log("inside a(): " + this);
 b.call(arrayObject ) //invoking function b()
 b.call(" again " + this) //invoking function b().
his.a.call(arrayObject ); //invoking function a() on a "window" object by passing "arrayObject" to the call(..) method
/ 4 outside
console .log("outside: " + this); 
his.a.call(arrayObject );

JavaScript contexts

---

## Q6: What is the difference between the .call(….) and .apply(…) methods?

**Answer:**

The difference is that apply(argArray) lets you invoke the function with arguments as an array whereas call(arg1, arg2, arg3) requires the parameters be listed explicitly .
call(…) and apply(…) methods of function object
var arrayObject = ["Java", "JEE" ]
function a(arg1, arg2, arg3) {
 console .log(this); // ["Java", "JEE"]
 console .log(arg1 + " " + arg2 + " " + arg3); // Hello Mr John
}

---

## Q7: What is the difference between undefined and null?

**Answer:**

The value of a variable with no value is undefined (i.e.it has been declared, but has not been initialized).
Variables can be emptied by setting their value to null. For example, to get a variable or closure to be garbage collected to release the memory .

---

## Q8: What is the difference between “==” and “==” in JavaScript?

**Answer:**

The == operator will compare for equality after doing any necessary type conversions. The === operator will not do the conversion, so if two values are not the same type === will simply return
false.a.call(arrayObject, "Hello", "Mr", "John" ); //passing ar gs individually
a.apply (arrayObject, ["Hello", "Mr", "John" ]); //passing an array
var a;
console .log(a) // undefined
function doSomething (arg1) {
 var local = "Some T ext" 
 //a closure that has access to ar g1 & local
 return function () {
 console .log(arg1 + " " + local ); 
 }
 
var fn1 = doSomething ("Printing..." ); //closure 1 taking memory with ar g1 & local
var fn2 = doSomething ("Outputting..." ); //closure 1 taking memory with ar g1 & local
fn1(); //invoking - Printing... Some T ext
fn2(); //invoking - Outputting... Some T ext
/now clear the closure memory. Will be Garbage Collected
fn1 = null;
fn2 = null;
fn1(); //error fn1 is not a function

We are comparing a string literal with a string object. Hence “===” returns false.

---

## Q9: How does “===” dif fer from “==” when checking for null or undefined?

**Answer:**

===
==
Executes the block if the the “ref” is either null or undefined.

---

## Q10: What is the purpose of “use strict” directive in JavaScript?

**Answer:**

Strict mode makes it easier to write “ secur e” JavaScript. Strict mode changes previously accepted “bad syntax” into real errors. For example, with strict mode you cannot use undeclared variables.
If “use strict ” directive is declared at the top of the JavaScript file, it has a global scope. If declared within a function, it has a functional scope."abc" == new String ("abc" ) // true
"abc" === new String ("abc" ) // false
f (nullRef === null) {
// executes this block only if null
}
f (undefinedRef === undefined ) {
 // executes this block only if undefined
}
f (nullRef === null) {
 // executes this block
}
f (undefinedRef === undefined ) {
 // executes this block
}

Key Points on JavaScript Interview Questions and Answers
1. Global variables are bad as they can conflict with third party library variable names.
2. Functions ar e objects in JavaScript as they can be nested, passed to another function, and you can invoke methods like call(…) and apply(…) on a function.
3. JavaScript has 3 scopes: global, functional, and block. The block scope was recently introduced.
4. Scope and context are dif ferent concepts. A context refers to an object that invokes a function with the keyword “ this“. The “window” object is a global context. When you create an object in
JavaScript, and invoke a function on that object, then “this” refers to the object that declares the function.
5. You can pass a dif ferent object to a function with the call(…) and apply(…) methods defined in the object function.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »"use strict" ;
x = 3.14; // ReferenceError. assignement to undeclared variable x
var y = 3.14; //good

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03