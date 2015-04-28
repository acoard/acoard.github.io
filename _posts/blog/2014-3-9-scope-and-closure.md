---
layout: page
header: no
title: "Understanding Scope and Closure, A Design Pattern for Data Encapsulation"
description: "In this post I will explain what scope is and how it works in JavaScript.  Then I will go on to show a design pattern that allows one to use scope in order to encapsulate data in an object making it only visible inside that object, as well as through getter and setter methods."
category: programming
tags: [programming, javascript, snippet, gist]
---

In this post I will explain what scope is and how it works in JavaScript.  Then I will go on to show a design pattern that allows one to use scope in order to encapsulate data in an object making it only visible inside that object, as well as through getter and setter methods. If you feel like you have a firm grasp of scope, skip to the bottom section.


Programming languages have something called a *namespace*.  The namespace is the set names currently used to identify a value or address in a language, i.e. the variables and the reserved words from the language.  As the size of a program grows so does the need to manage the namespace. 

You may have experienced this yourself.  When writing your first JavaScript progams it's easy to keep the names of the dozen or so variables separate.  But when you find your .js files spanning thousands of lines, across numerous files, it's near impossible to remember if the variable "sum" has been used somewhere else.  The problem is only compounded when you use libraries or codebases that populate the namespaces with variables you're unfamiliar with.

Scope provides enclosures in which variables can be defined inside of them, but they remain undefined outside of the scope. In JavaScript, scope is congruent with the {} brackets of a function.  That is to say, that variable is only accessible within the function it has been defined in, as well as all inner functions also defined in that function (more on these inner functions later).  Here's a quick example to help it make sense:


    var hello = "Hi there";
    var scopeFunc = function() {
    	var stranger = "stranger";
    	var internalFunc = function(){
    		console.log(stranger.toUpperCase())
    	}
    	internalFunc(); //"STRANGER"
    	console.log(hello + " " + stranger); // "Hi there stranger"
    }
    scopeFunc() // ""Hi there stranger"
    typeof(stranger) // "undefined" because it's outside of the function where it has been defined.

The important things to note from this:  `hello` is accessible everywhere.  `stranger` is accessible only within scopeFunc() and internalFunc().  Also, as internalFunc() is only called inside of scopeFunc(), when you call scopeFunc() the output will look like this:

	STRANGER
	Hi there stranger!

Douglas Crockford in "JavaScript: the Good Parts" lists two main advantages of scope:

1. As we talked about earlier, it helps keep the namespace cleaner.  I would be free to use `stranger` in another function somewhere else again with absolutely no conflicts.
2. This allows JavaScript to provide automatic memory management.

Since variables defined within a scope are limited to that function, the **best practice** is to define all the necessary variables at the beginning of the function.

While JavaScript isn't as object-oriented as other languages, there is still the value to the programmer of encapsulating data inside of an object.  Encapsulation helps isolate variables from another and make the logical flow of the program clear.  But there's a problem: scope in JavaScript isn't defined by objects, but by functions!  Take a look.

	var myObj = {
       prop: "is this internal?",
       anotherObj: {
       	anotherProp: "what about this?"
       }
	}
	myObj['prop'] //"is this internal?" - clearly not!
	myObj['anotherObj']['anotherProp'] //"what about this?" - another resounding no!

If you think about it, this makes sense.  We usually want objects to be transparent to us, it's what makes them such an effective and easy way to store information.  Nonetheless, there are times when we want to encapsulate data so that it is visible only to that object.
We can accomplish this by using a function to return an object. This is an incredibly intelligent design pattern for JavaScript, and all credit must go to Douglas Crockford for making me aware of it (p.37 of the first edition of JavaScript: the Good Parts).

##Using function closures to encapsulate data in objects##

Let's say we want to implement a traffic light.  It has three states: green, yellow (or orange if you're into that type of thing), and red.  We want to make it impossible to direcly change the state, instead the changes are to only be in order, i.e. you can't go from red to yellow, or from green to red.  It's always green->yellow->red->green->... 


	var trafficLight = (function(){
		var light = 'green';
		return {
			changeLight: function(){
				if (light=='green'){light='yellow'}
				else if (light=='yellow'){light='red'}
				else if (light=='red'){light='green'}
			},
			getLight: function(){
				return light;
			}
		};
	}());

	typeof(light) // "undefined"
	trafficLight.getLight() //"green"
	trafficLight.changeLight()
	trafficLight.getLight() //"yellow"
	trafficLight.changeLight()
	trafficLight.getLight() //"red"

It's important to realize that `trafficLight` is not assigned to a function, but rather it is assigned to the result of that function.  That occurs because of the following pattern:

	var foo = (function(){
		...
	}());

Note how it ends with (), which calls the function and returns the object. In the trafficLight example, trafficLight still has access to `light` itself, not just a copy.  This is what is meant by **closure**, for the `getLight()` and `changeLight()` methods still have access to the variable even though the original function (which created the object) has finished running and is no longer active.


##Credit##

I know I've mentioned him a bunch already, but I just wanted to thank Douglas Crockford again.  His experience and clarity is invaluable in the long road towards truly grokking JavaScript.

<a href="http://www.amazon.com/gp/product/0596517742/ref=as_li_qf_sp_asin_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0596517742&linkCode=as2&tag=acoardcom-20">You can buy JavaScript: The Good Parts on Amazon.</a>

