---
layout: post
title: "Foolproof Guide to Hoisting in Javascript"
date: 2014-09-23 11:09:07 -0400
comments: true
categories: 
---
Javascript is often made fun of for its idiosynchrasies (if you haven't seen Gary Bernhardt's "Wat" talk, stop right now and watch it: http://vimeo.com/94881698). Today I want to focus on one issue that can cause problems in Javascript - hoisting.

<!--More-->

Whenever you open up a new scope in Javascript - for instance, when you're defining a new function- the first thing that happens is a clearing out of memory space for all variables, which it initializes as undefined. This concept can be strange because no matter what order your javascript variables and functions present themselves in your code, your variables will be defined first. So given the following Javascript:
```javascript
function bigOuterFunction(){
  function imWrittenFirst(){
    console.log("but hoisted second");
  }
  var firstVariable = 4;
}
```
The load order will actually look like this:
```javascript
function bigOuterFunction(){
  var firstVariable = undefined;
  function imWrittenFirst(){
    console.log("but hoisted second");
  }
  firstVariable = 4;
}
```
Next, declared functions are hoisted (by the way, this is a declared function:
```javascript
function helloWorld(name){
  console.log("Hello, " + name + "!");
}

helloWorld("Will")
//=> "Hello, Will!"
```
This is a function expression (anonymous function):
```javascript
var imAnonymous = function(name){
  print "Hello, " + name + "!";
};

imAnonymous("Will")
//=> "Hello, Will!"
```
It is important to note the distinction between the two types of functions. Since the function expression was stored in a variable, the variable itself would be hoisted in step 1 and assigned as undefined. Then, it wouldn't be until `imAnonymous` is called that the anonymous functioned would be invoked.

The third thing that happens is all of your executable code runs. This includes variable assignment. However, it's easier to explain in terms of a simple list.

1) All declared variables are first manually initialized to an undefinedvalue and come first in the load order.
2) All loaded functions that end up being overwritten by other functions with the same name will just disappear from their current place in the load order.
3) Declared functions that end up replacing other functions, however, will NOT take the order place of the previous version, but instead will just fall into the load order behind existing loaded functions.
4) All declared variables receiving either new values or function expression assignments will lose the var keyword in the executable code portion of your answer (since they will already be declared through criteria 1).
5) Any function expression assignment is treated here as executable code, and does not change the load order of declared functions in your answer, even if they later replace that function.
6) All unreachable executable code that follows an unavoidable return statement (where the function ends) will disappear from your answer.

If you follow these steps you'll never get the dreaded UNDEFINED or ERRORs that keep your functions from running smoothly.
