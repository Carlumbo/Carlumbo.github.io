---
layout: post
title:      "Lessons on Hoisting"
date:       2019-11-22 18:21:22 +0000
permalink:  lessons_on_hoisting
---

In JavaScript you have access to a very important mechanism called hoisting. Though what is hoisting really doing for you? Hoisting allows variables and function to be moved to the top of the scope the are in before the execution of code. This is important because it means you can declare the code after you have used it. This mechanism matters because it allows you to plan out where you need to use this /function throughout your code before actually declare its value. 

An example would be something like this:

```
 p = 1;

example = document.querySelector(‘h3’);
example.innerHTML = p; 
 var p; 
```

this is the same as:
```
let p;
p = 1;

example = document.querySelector(‘h3’);
example.innerHTML = p;


```
Hoisting is a default behavior of JavaScript, and it moves all declarations to the top of the scope you are declaring in. variables using let or const will not be hoisted neither will initializations. These will also return different errors you will get ReferenceError for using a let or const because they are not hoisted. If you use a variable or function before you declare it, you will get undefined as a result. 

Hoisting is a very useful concept to have knowledge of and is sometimes overlooked or unknown to developers. If you don’t understand how hoisting works you can create unnecessary errors through out your program. 


