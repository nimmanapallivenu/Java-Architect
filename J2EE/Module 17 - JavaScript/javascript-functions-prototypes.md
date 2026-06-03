# JavaScript Functions & Prototypes

> **Module**: JavaScript  
> **Topic**: JavaScript Functions & Prototypes

---

## 📋 Table of Contents



- [Q11: What are the dif ferent ways can you define a function on JavaScript?](#q11)
- [Q12: What is the difference between the two approaches of declaring a function and a](#q12)
- [Q13: What are the dif ferent ways can you invoke a function?](#q13)
- [Q14: What are the dif ferent ways to set the magic and dynamic context variable “this](#q14)
- [Q15: What will be the output of the following code snippet?
/user object
var user = {](#q15)
- [Q16: What are the dif ferent ways to create objects in JavaScript?](#q16)
- [Q17: What is a “prototype” property in JavaScript?](#q17)
- [Q18: What is a “prototype” property in JavaScript?](#q18)
- [Q19: Can you explain the following code, and what will be the output?/ Example showin](#q19)
- [Q20: What will be the output of the following code where bind is used with an additio](#q20)

---

## Q11: What are the dif ferent ways can you define a function on JavaScript?

**Answer:**

2 ways
1. Function Declaration
2. Function Expr ession
By assigning an anonymous function to a variable, as a function in JavaScript is an Object.

---

## Q12: What is the difference between the two approaches of declaring a function and assigning a functional expression to a variable?

**Answer:**

The “function declaration” can be hoisted. Hoisting is a term in JavaScript where the variable and function declarations are automatically moved to the
top by the JavaScript engine. Hence, you ca rewrite the above function declaration code asfunction foo() {
 console .log("Executing a function...." ); 
}
foo(); //invoking foo -- Executing a function....
var foo = new function () {
 console .log("Executing a function...." ); 
}
foo(); //invoking foo -- Executing a function....

The above code works because the function declaration is hoisted (i.e. moved above foo()) by the engine. Function expressions are not hoisted, and you will get
an error. “TypeError: foo is not a function”

---

## Q13: What are the dif ferent ways can you invoke a function?

**Answer:**

There are a number of ways you can invoke a function.
1) foo() as shown in the last question.
2) with call(…) and apply(….) methods.
3) Immediately Invoking Function Expression ( IIFE )foo(); //invoking foo -- Executing a function....
/declaring the function
function foo() {
 console .log("Executing a function...." ); 
}
var foo = function () {
 console .log("Executing a function...." ); 
}
foo.call(); //executing a function....
foo.apply (); //executing a function....
function () {
 console .log("Executing a function...." ); 

Executes immediately and outputs: “Executing a function….”

---

## Q14: What are the dif ferent ways to set the magic and dynamic context variable “this” in a function?

**Answer:**

There are 4 ways to set “this”.
1) By calling a method on an object. “var1” is variable with a value of 10, and “a” is a function. A JavaSCript object is simply a name/value pair with “:”
separating both, and a value can be a function as well.
The output will be
})()
/define a JavaSCript object with name:value pair .
/the value can be a function as well.
var myObject = {
 var1:10,
 a: function (){
 var var1 = 1;
 console .log(this);
 console .log(var1, this.var1); // outputs 1, 10
 }
myObject .a(); // outputs 1, 10

JavaScript Object
2) Call a function and pass “this” in with .call or .apply
Every JavaScript function has a call and apply methods. As you can see below, the function a() is called on object “anotherObject” hence this.var1 will be 20.
But just var1 is from the “myObject”, which is 1.
The output will bevar myObject = {
 var1:10,
 a: function (){
 var var1 = 1;
 console .log(this);
 console .log(var1, this.var1); // outputs 1, 10
 }
var anotherObject = {
 var1:20,
 a: function (){
 var var2 = 2;
 console .log(this);
 console .log(var2, this.var1); // outputs 2, 20
 }
myObject .a.call(anotherObject ); // outputs 1, 20

JavaScript Call method and this reference
3) use new to create a brand new function context
4) .bind methods on the object prototype.
We will discuss 3) & 4) in detail bit later on.

---

## Q15: What will be the output of the following code snippet?
/user object
var user = {
 callee : "Java", 
 callT echnology : function () {

**Answer:**

The output will be “ JEE” as “this” passed is a new object with a value of “callee:JEE”.

---

## Q16: What are the dif ferent ways to create objects in JavaScript?

**Answer:**

There are 5 ways you can create objects in JavaScript.
#1. Using an Object constructor .
#2. Literal constructor as shown above in the last question. console .log(this.callee );
 }
};
user.callT echnology .call({callee : "JEE" }); //invoke a function
var user = new Object ();
user.callee = "Java" ;
user.callT echnology = function () {
 console .log(this.callee );
}
user.callT echnology ();
/user object
var user = {
 callee : "Java", 
 callT echnology : function () {
 console .log(this.callee );
 }
};

#3. Function constructor .
#4. Prototype.
#5. Singleton.user.callT echnology ();
function User (callee ) {
 this.callee = callee ;
 this.callT echnology = function () {
 console .log(this.callee );
}
}
user.callT echnology ();
function User (){};
User .prototype .callee = "Java" ;
User .prototype .callT echnology = function () {
 console .log(this.callee );
}
user.callT echnology ();

---

## Q17: What is a “prototype” property in JavaScript?

**Answer:**

Like every JavaScript function object has call and apply methods, every JavaScript function object has a property known as the “prototype”. This property
is empty by default, and you attach properties and methods on this prototype property when you want to implement inheritance. This prototype property is not
enumerable, which means it isn’ t accessible in a for/in loop. As shown in the above code example, you add methods and properties on a function’ s prototype
property to make those methods and properties available to instances of that function.
Now, how can you extend the User class to create a new AppUser .var user = new function () {
 this.callee = "Java"
 this.callT echnology = function () {
 console .log(this.callee );
 }
}
user.callT echnology ();
function User (callee ) {
 // Instance properties can be set on each instance of the class
 this.callee = callee ;
/ Prototype properties are shared across all instances of the class.
/However, they can still be overwritten on a per -instance basis with the magic `this` keyword.
User .prototype .callT echnology = function () {
 console .log(this.callee );
;
var user = new User ('Java' );
user.callT echnology (); // Java 

In the above code we are setting AppUser ’s prototype to an instance of User, so that AppUser inherits all of User ’s properties. W e’re also using User .call to
inherit the User ’s constructor. As we have seen before, the “.call” lets you call a function with the value of “this”. So when this.callee is set inside the User
constructor, it’s the AppUser ’s “callee” property being set, not the User ’s.
function AppUser (callee ) {
 User .call(this, callee );
}
AppUser .prototype = new User ();
var jeeAppUser = new AppUser ('JEE' );
eeAppUser .callT echnology (); // JEE

JavaScript inheritance with the “prototype” property .

---

## Q18: What is a “prototype” property in JavaScript?

**Answer:**

Functions are objects in JavaScript. W e already looked at “.call” and “.apply” methods can be invoked on the function object. Bind is used for setting the
“this” value in methods and for currying functions. So, bind() allows us to easily set which specific object will be bound to this when a function or method is
invoked.
Binding parameters as shown below. Bind creates a new function that will have “this” set to the first parameter passed to bind()

---

## Q19: Can you explain the following code, and what will be the output?/ Example showing binding some parameters
var sum = function (a, b) {
return a + b;
};
var add5 = sum.bind(null, 5);
console .log(add5 (10)); //outputs 15
var user = {
 name : 'John Smith' ,
 print : function () {
 getCommentCount (function (data) {
 console .log(this.name + ' retrieved ' + data.comments + ' comments' );
 });
 }
;
var getCommentCount = function (callback ) {
 callback ({ comments : 10 });
;

**Answer:**

When you call the print() function on the “user” object, it executes the “getCommentCount” function by passing the callback closure:
and the “data” passed is
The output will be:
Q. Why didn’ t it print the this.name?
A. The new closure is created inside “getCommentCount” has its own scope and the variable “this” doesn’ t point to our original object “user”. The “this” in the
result callback is not pointing to the User but to a newly created object.
Q. How can you fix this issue?
A. One way to fix this is to assign the “this” to another variable like “self” before creating a new closure as shown below .user.print ();
function (data) {
console .log(this.name + ' retrieved ' + data.comments + ' comments' );
}
{ comments : 10 }
retrieved 10 comments

The above change will output
The above approach can get messy by filling the codebase with assignements like “self” or “that”. The “ bind ” method to the rescue.print : function () {
 var self = this;
 getCommentCount (function (data) {
 console .log(self.name + ' retrieved ' + data.comments + ' comments' );
 });
}
John Smith retrieved 10 comments
var user = {
 name : 'John Smith' ,
 print : function () {
 getCommentCount (function (data) {
 console .log(this.name + ' retrieved ' + data.comments + ' comments' );
 }.bind(this));
 }
;
var getCommentCount = function (callback ) {
 callback ({ comments : 10 });
;

Notice the .bind(this).

---

## Q20: What will be the output of the following code where bind is used with an additional suf fix param?

**Answer:**

The output will be:
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »user.print ();
var user = {
 name : 'John Smith' ,
 print : function () {
 getCommentCount (function (suffix, data) {
 console .log(this.name + ' retrieved ' + data.comments + ' comments' + suffix);
 }.bind(this, ' & more comments to come....' ));
 }
;
var getCommentCount = function (callback ) {
 callback ({ comments : 10 });
;
user.print ();
John Smith retrieved 10 comments & more comments to come ....

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03