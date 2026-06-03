# JavaScript Closures

> **Module**: JavaScript  
> **Topic**: JavaScript Closures

---

## 📋 Table of Contents



- [Q31: What is a Closure in JavaScript?](#q31)
- [Q32: Where are closures used?](#q32)
- [Q34: Why did it print just 4, and not each number from 1 to 3?](#q34)
- [Q35: How to fix the above code?](#q35)
- [Q36: Is there any other way to achieve the similar results as above?](#q36)

---

## Q31: What is a Closure in JavaScript?

**Answer:**

A closure is an “inner function” that has access to the “outer function’ s” variables—scope chain. The closure has three scope chains:
1) It has access to variables defined between its curly brackets.
2) It has access to the outer function’ s variables.
3) It has access to the global variables.
Output :function sum(num1, num2 ) {
 var ressultT ext = "Sum of " + num1 + " and " + num2 + " = " ;
 // this inner function has access to the outer function's variable "resultT ext", num1, and num2
 // So the function calc() "close over" the variable, hence called closure
 function calc() {
 var result = num1 + num2; 
 return ressultT ext + result ;
 }
 return calc();
um(6, 7); // 13

JavaScript Closure

---

## Q32: Where are closures used?

**Answer:**

Closures are used in Node.js and they key behind the Node.js’ asynchronous, non-blocking architecture. Closures are also frequently used in jQuery and
just about every piece of JavaScript code you read.
Q33 What will be the output of the following code?
A33. The “setT imeout(callback, time)” is an asynchronous call. The callback function is: “function() {console.log(“printing…” + i);}”
Output:for(var i = 1; i <= 3; i++) {
 setTimeout (function () {
 console .log("printing..." + i); 
 }, 5);
}

---

## Q34: Why did it print just 4, and not each number from 1 to 3?

**Answer:**

JavaScript execution has a call stack, W ebAPI, and a event queue as shown below. Any asynchronous and callback processing is passed to the W epAPI to
be processed asynchronously, and then put on to the “event queue” to be picked up by the stack when the stack is empty. The for loop is executed within the
stack first, and the value of i reaches 4, when the loop exits. Then the “setT imeout(callback, time)” is pushed to the stack from the “event queue” and executes.
Hence it prints “printing…4”.printing ...4

JavaScript Processing

---

## Q35: How to fix the above code?

**Answer:**

You can fix it by NOT using a closure. Use an IIFE (Immediately Invoked Function Expression) like (function(i){….})(i). The “(i)” means invoke the
function wrapped in “(function(i){….})”. It will create its own scope and you can pass i to the function. In this case i will be a local variable and value of the i in
every loop will be preserved. Here is the revised code with IIFE .
Output:
So, IIFE is

---

## Q36: Is there any other way to achieve the similar results as above?

**Answer:**

Yes, using the “bind” function.for(var i = 1; i <= 3; i++) {
 setTimeout ((function (i) {
 console .log("printing..." + i); 
 })(i), 5);
}
printing ...1
printing ...2
printing ...3
function (i) {
 console .log("printing..." + i); 
})(i)

Q37 What is currying, and how will you implement it in JavaScript?
A37 Currying is an invocation of a function with partial arguments passed. A first few arguments of a function is pre-processed and a “curried function” is
returned. Y ou can add more arguments to the curried function. For example,
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »for(var i = 1; i <= 3; i++) {
 setTimeout (console .log.bind(console, "printing...." + i), 10);
}
function addInitial (initial ){
return function (num){
 return initial + num;
}
var addFive = addInitial (5);
/addFive is Curried
addFive (6); //11
addFive (10); //15
addFive (-5); //0

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03