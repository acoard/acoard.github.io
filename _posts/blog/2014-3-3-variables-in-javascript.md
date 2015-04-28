---
layout: post-no-feature
title: "Variables in JavaScript. Pass by reference - or is it?"
description: "Are variables in JavaScript pass by reference or pass by value?  The answer is more complicated than you'd think."
category: programming
tags: [javascript, programming]
---

Are variables in JavaScript pass by reference or pass by value?  The answer is more complicated than you'd think. But before I get to the nitty gritty, let's cover the basics of variables first.  If you already are comfortable with variables, feel free to skip this section.


A Primer on Variables
--------------

Variables are a core part of any programming language, and JavaScript is no different.  Variables can be assigned to the five type of JS primatives (numbers, booleans, strings, null, undefined), as well as objects and functions.  That is to say, basically anything you create in JavaScript can be assigned to a variable for ease of use as well as clarity in code.  Here's a simple example.
	

	var x = 123;
	console.log(x + 5);
	>128	


A way of thinking about a variable is to think it's a little box in your computer that points to another box (each box representing a portion of your computer's memory).  The box it points to is the value of the variable.  So in `x = 123`; there are two boxes.  The x box points to another box that contains the number 123.  It's important to note that the x box itself doesn't contain any values, it merely points to the value.

When a variable has been initialized but not declared, (e.g. `var x;`), then the value of the variable is `undefined`.

There are a few restrictions with naming variables.  They  have to start off with a letter, underscore, or $, numbers can be any of the following characters.  Also, there are some restricted words that can't be used for variables as they're already used or otherwise reserved by the JavaScript language itself.  You can find the list [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Reserved_Words)


Pass By Reference - or is it?
-----------------
 

Languages differ in how they exactly implement variables.  One of the key differences is whether a language operates on *pass by value* or *pass by reference*.  In very simple programs the difference might never come up, but in more complicated programs or programatic structures they can crop up.  If you don't know about the difference between pass by value and pass by reference the errors can be a real headache to solve.

So, what is the difference? I'll show some code and then explain it.


	var x = 123;
	var z = x; //z == 123
	x = 1234
	z == x
	>false

After executing all these statements, x will be equal to 1234, and z will only be equal to 123.  You might be surprised by this.  Perhaps you think that since `z = x` the two should stay equal.  But this is not the case, because JavaScript is a pass by value language.  Let's put this in the context of boxes.  When z = x is executed, what happens is the box that z represents points now to another box containing the numbers 123.  Z and x, both point to 123, but they do not in any way point to or connect with each other.This is what is meant by pass by value.  When varaibles are assigned, only the value of the variable is passed along, not the reference.

If JavaScript was a pass by reference language, then at the end of the codeblock z and x would both be equal to 1234.

Unfortunately there is an exception to all of this.  If, instead of passing a primative you pass an objects, then the variables within those objects (called properties) become pass by reference.  Confused?  Let's cover it in a bit more detail.

	x = {item: "unchanged"}
	z = x
	z["item"] // "unchanged"
	z["item"] = "changed"
	x === "changed" //true

In the first line I pass in an object literal with one property, the key being `item` and the value being `unchanged`.  If I had just made `x = "unchanged"`, rather than an object containing that value, then nothing would have changed.  This is because the value of the object literal is itself a reference.  If you want to read more about this, check out this [illuminating thread](http://stackoverflow.com/questions/518000/is-javascript-a-pass-by-reference-or-pass-by-value-language) on StackOverflow.

Global vs. Local
----------------

Because I just couldn't stop writing today, as a bonus I've included a section on global and local variables. 

Like most programming languages, variables in JavaScript can be global or local.  A global variable is one which can be accessed anywhere within the program, and a local one is restricted to the scope it has been defined in.  Global variables should only be used when it is necessary.  Overly employing global variables is considered bad practice both because it often indicates the structure of the program is messy, leading to spaghetti code, and because it clutters up the namespace.  Imagine you have a global variable named `user`, and then you use a JavaScript library that also has a global variable named the same thing.  What would follow would be errors and headaches.  Nonetheless, global variables are useful in lots of situations, just use them with caution.

So, what determines whether a variable is local or global?  There are multiple factors, one of which is scope.  I won't be going into scope in detail in this post, because I have to leave more content for me to write about tomorrow!  Suffice to say, if a variable is defined within a function it can only be accesed *locally* within that function. If you want a proper answer to scope and just can't wait for me to write about it, [here](http://msdn.microsoft.com/en-us/library/bzt2dkta(v=vs.94).aspx) is some material on the matter.

Crucial in defining variables is the keyword `var`.  Without it, all variables defined are global.  With it, variables can be local or global depending on the scope they are declared in.

	x = 123 //global
	var i = 123 //global, because it is defined in the uppermost scope.
	function myFunc(){
		z = 'global';
		var y = 'local';
	};
	myFunc(); //Before the function is called, both z and y are undefined.
	z
	>"global"
	y
	>ReferenceError: y is not defined


So to reiterate, local variables are...

* Generally preferable to global variables.
* Created by using the `var` keyword inside of a scope of e.g. a function.