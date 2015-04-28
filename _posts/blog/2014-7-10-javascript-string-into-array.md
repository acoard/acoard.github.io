---
layout: page
header: no
title: "Turn a JavaScript string into an array of characters"
description: ""
category: programming
tags: [javascript, programming]
---

One JavaScript JavaScript feature you may not know all the useful details about is String.prototype.split().  If you pass it no paramaters you get your entire string returned as the single element of an array.  *But* if you pass it "", you get an array of characters that comprise the string. 

    > "hey there".split("");
    < ["h", "e", "y", " ", "t", "h", "e", "r", "e"]


The "" is necessary, if you just run `split()` you get the whole string as one element in an array.

This is useful if you want to iterate over each letter and build a new string (remember, [strings are immutable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type)). 

What separates this from the regular string square brackets lookup (e.g. `'foo'[0]` == `f`), is that you are returned in Array object.  That means you can use Array methods like `.forEach()` and `.sort()`.

    > "foo bar".split("").sort();
    < [" ", "a", "b", "f", "o", "o", "r"]



