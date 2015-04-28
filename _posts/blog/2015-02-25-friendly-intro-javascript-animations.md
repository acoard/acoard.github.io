---
layout: page
header: no
title: "A friendly introduction to making your own animations on the web using JavaScript"
description: "If you're a frontend JavaScript developer, you need to be familiar with multiple ways to make animation happen on the web.  This article is for the complete beginner when it comes to web animation."
category: programming
tags: [programming, javascript, animation, frontend]
---


Back in the days of yore, if you wanted to animate something on the web you'd use Flash.  Flash has largely died out because it was a big black box in the middle of a field of open technologies. Today, there are many different ways to try and animate behaviour on the web.  Generally speaking, there are three "native" ways that don't involve plugins like flash:

1. CSS animations
2. JavaScript animations
3. `<canvas>` animations (a special kind of JavaScript animations).

Generally those three ways scale in complexity.  CSS animations are best for small animations, transitions between elements/state.  JavaScript animations can be more robust, and with `<canvas>` animations you're given complete control (which takes more setup time). Generally, stay away from canvas unless you need to do image manipulation or very graphically rich interactions like games. But let me be clear, you can do some amazing things with just CSS animations, look no further than what's on [codepen](http://codepen.io/popular/).  For this article, I'm giving an introduction into #2.


### JavaScript animations

"JavaScript animations" is in itself an ambigous phrase, and there are different ways to animate using JS.  The proper way is using the native `requestAnimationFrame()`, although it's still entirely possible to animate things by just using `setInterval()`.  [jQuery uses setInterval() as a fallback](https://github.com/jquery/jquery/blob/76df9e4e389d80bff410a9e5f08b848de1d21a2f/src/effects.js#L648), which is best practice [because IE9 doesn't support `requestAnimationFrame()`](http://caniuse.com/#feat=requestanimationframe).

So, what do I mean by `setInterval()` and `requestAnimationFrame()`?

`setInterval(fn, ms)` is a JavaScript function that calls a function every x number of milliseconds.  So you could write a basic animation doing something like:

    var elm = document.getElementById('target');

    function animate(elm){
        elm.style.position = 'relative';
        if (elm.style.left === ''){
            elm.style.left = '1px';
        }
        else {
            //increment 'XXpx', e.g. '11px' into '12px'
            elm.style.left = parseInt( elm.style.left.split('px') ) + 1 + 'px';
        }
        return elm;
    }
    //We need to wrap our function in an anonymous function so it isn't executed right away.
    setInterval(function(){
        animate(elm)
    }, 10);

Using `setInterval()` to animate is not the best way of doing things.  It's hard to maintain, it's not performant... but it works.  One of the bigger draw backs?  It continues to animate even if you're not looking at that tab!  Still, depending on the context (still supporting IE9?) it might be appropriate.  At the very least you should be aware of it.  If you want to learn more, here are some resources:

* [http://javascript.info/tutorial/animation](http://javascript.info/tutorial/animation)
* [https://hacks.mozilla.org/2011/08/animating-with-javascript-from-setinterval-to-requestanimationframe/](https://hacks.mozilla.org/2011/08/animating-with-javascript-from-setinterval-to-requestanimationframe/)


`requestAnimationFrame()` is the default solution for non-legacy browsers.  In most cases you should turn to this option before `setInterval()`.  While there are newer, and arguably superior solutions to JS animation, those solutions are more likely to require [polyfills](https://github.com/web-animations/web-animations-js).  There are a lot of resources out there to put you on your track towards mastering `requestAnimationFrame()`, here are the best ones I've found:


* [http://www.smashingmagazine.com/2013/03/04/animating-web-gonna-need-bigger-api/](http://www.smashingmagazine.com/2013/03/04/animating-web-gonna-need-bigger-api/)
* [http://creativejs.com/resources/requestanimationframe/](http://creativejs.com/resources/requestanimationframe/)

But let's be honest, you want a demo, so here you go.

`requestAnimationFrame()` requires the function you call it to be [recursive](http://en.wikipedia.org/wiki/Recursion_(computer_science)).  Since the function has to call itself, it can incrementally draw whatever animation you're going for.

    var elm = document.getElementById('target');
    
    function animate(){
        requestAnimationFrame(animate, elm); //This is what 

        elm.style.position = 'relative';
        if (elm.style.left === ''){
            elm.style.left = '1px';
        }
        else {
            //increment 'XXpx', e.g. '11px' into '12px'
            elm.style.left = parseInt( elm.style.left.split('px') ) + 1 + 'px';
        }
        
    }

    requestAnimationFrame(animate, elm); //start the animation
    
And to top it off, [here's a link to a collection of very impressive JavaScript animations](http://fff.cmiscm.com/#!/main), that'll leave your jaw on the floor.