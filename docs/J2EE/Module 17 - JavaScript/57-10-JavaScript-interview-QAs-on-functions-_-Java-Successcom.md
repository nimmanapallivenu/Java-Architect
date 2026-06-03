# 57 10 JavaScript interview Q&As on functions   Java Success.com

## Table of Contents

- [Q21: What will be the output of the following JavaScript code?](#q21)
- [Q22: What is a self-executing or self-invoking functions in JavaScript?](#q22)
- [Q23: In what scenarios to use the self-invoking functions?](#q23)
- [Q24: Do you envisage any problems with the above code?](#q24)
- [Q25: Can you write a function in JavaScript that checks if a particular “numberT oFin](#q25)
- [Q26: Can you write a function that prepends the text “Now Printing…..” to any message](#q26)
- [Q27: Is “ar guments” an array in JavaScript? Can you list some useful properties of t](#q27)
- [Q28: Can you write a function to add one day to today?](#q28)
- [Q29: Can you write a function that will repeat the given text “n” number of times?](#q29)
- [Q30: Can you see any issue(s) with the above implementation of repeatNoOfT imes ?](#q30)

---

## Q21: What will be the output of the following JavaScript code?

**Answer:**

The “module” is an object defined within “{….}”. It has a property “x” and a function “getX”.
It is important understand the magic variable “this”.
Because in //2 case, “this” refers to the global window object, which has the value of 9. T o fix the issue, you need to create a new function with ‘this’ bound to module.
“bind()” method is in action again.x = 9;
var module = {
 x: 18,
 getX : function () {
 return this.x;
 }
;
module .getX ();//1
var getX = module .getX ;
getX (); //2
module .getX (); //1 : returns 18
/...
getX (); //2 : returns 9, why?
/ create a new function with 'this' bound to module
var boundGetX = getX .bind(module ); //binding getX to "module"

---

## Q22: What is a self-executing or self-invoking functions in JavaScript?

**Answer:**

In, JavaScript, the functions run immediately when you put () after their names (). For example,
Here is an anonymous (i.e. no name) self-invoking function that runs straight away .

---

## Q23: In what scenarios to use the self-invoking functions?

**Answer:**

A typical example would be to run any periodic tasks, say every 10 seconds.

---

## Q24: Do you envisage any problems with the above code?

**Answer:**

Yes. It keeps running every 10 seconds regardless of the previous run was completed or not. This is where self-invoking functions come in handy . We can do the same
task with the help of self-invoking function along with “setT imeout” as shown below .boundGetX (); // 18
handleRequest (); //runs immeduiately
handleRequest ; // DOES NOT run ummediately
function foo(){
 console .log("Running the anonymous self-invoking function straight away .....")
})()
setInterval (handleRequest , 10000 );

Prints, “Executing………” every 10 seconds if the previous execution is completed.
Another advantage of self-invoking functions is security , as the data will be wrapped, and will be only available within the scope of the function.

---

## Q25: Can you write a function in JavaScript that checks if a particular “numberT oFind” is present in a list of numbers supplied?
where numT oFind is 1, and given list of numbers are [4, 8, 9]. This returns false as 1 is not found in [4,8,9]

**Answer:**

In the above example,
1) numT oFind=1, and then numT oFind=5.
2) Every JavaScript function has an implcit “ arguments ” object. So, you should not name any of your local or global variables as ar guments. In the above example, the
arguments=[4, 8, 9] and [3, 1, 2, 5] in the respective calls. function handleRequest (){
 console .log("Executing ..........." )
setTimeout (handleRequest , 10000 );
}();
sNumFound (1,[4, 8, 9]) 
function isNumFound (numT oFind ){
console .log(arguments );
var args = Array .prototype .slice.call(arguments ); // convert the ar gs to array
return args.indexOf (numT oFind ) != -1;
}
sNumFound (1,[4, 8, 9]) //false, as 1 is not found
sNumFound (5, [3, 1, 2, 5]) //true, as 5 is found

3) Array .prototype.slice.call(ar guments); converts the ar guments to an Array .

---

## Q26: Can you write a function that prepends the text “Now Printing…..” to any messages passed?
For example,

**Answer:**

We used the “ slice” method of the Array in the last answer , and here we will use the “ unshift ” method.
1) The “unshift” method prepends to the current array .
2) The “ar guments” is an implicit object in the function object that takes variable number of ar guments like ‘Some more’, ‘ messages’, ….

---

## Q27: Is “ar guments” an array in JavaScript? Can you list some useful properties of the “ar guments” object?

**Answer:**

“length ” and “ callee “.
The ar guments. callee property contains the currently executing function.
The ar guments. length gives the passed in ar guments count.Some message text --> Now Printing .....Some message text
function print (){
var args = Array .prototype .slice.call(arguments );
args.unshift ('Now Printing.....' );
console .log.apply (console , args);
}
print ('Some message text' ); // Now Printing.....Some message text
print ('Some more' , ' messages' ); //Now Printing.....Some more messages

jsfiddle screenshot on ar guments object properties

---

## Q28: Can you write a function to add one day to today?

**Answer:**

i.e. evaluate the next day .
Date .prototype .evalNextDay = function (){
var today = this.getDate ();
return new Date (this.setDate (today +1));
}
var date = new Date ();
date; //today
date.evalNextDay (); //next day

---

## Q29: Can you write a function that will repeat the given text “n” number of times?

**Answer:**

Using the JavaScript object property “ prototype ” used for inheritance . This function basically extends the native String class functions.

---

## Q30: Can you see any issue(s) with the above implementation of repeatNoOfT imes ?

**Answer:**

Yes. What if this method is already implemented by the String class. T o ensure that the function is not already implemented, you need to define it as
Hence, the above code can be rewritten asString .prototype .repeatNoOfT imes = function (times ) {
 var input = '';
 for (var i = 0; i < times ; i++) {
 input += this;
 }
 return input ;
;
console .log("JS is Great!!!" .repeatNoOfT imes (2)); // JS is Great!!!JS is Great!!!
String .prototype .repeatNoOfT imes = String .prototype .repeatNoOfT imes || function (times ) {/* code here */ };
String .prototype .repeatNoOfT imes = String .prototype .repeatNoOfT imes || function (times ) {
 var input = '';
 for (var i = 0; i < times ; i++) {
 input += this;
 }
 return input ;

Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »};

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
