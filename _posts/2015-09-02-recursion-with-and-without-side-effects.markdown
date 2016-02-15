---
layout: post
title:  "Recursion With and Without Side Effects"
date:   2015-09-02 18:19:00
categories: javascript recursion side-effects
disqus: true
---

Recursion and I have always had a strained relationship. At times, I've thought that I understood recursion, only to have my misunderstandings about recursion recursively bite me in the rear.

In an effort to further elucidate recursion, both for myself and any other person who wonders by this page, this post will explore two different ways of using recursion. Namely, we will be investigating recursion with side effects and recursion without side effects.

As getting the factorial of a number happens to be one of the canonical examples of basic recursion, this is the problem we will be solving in the following functions.

### Iteration
To start, let's look at the iterative version of the code. This code is fairly unremarkable and obviously not recursive, but I've written it here as a reminder of what our function is supposed to be doing. If we pass in a value of 5, our function should essentially return `5 * 4 * 3 * 2 * 1`, or in other words, `120`.
    
    var iterativeFactorial = function(num) {
      // in case the user is a jerk, return undefined for negative numbers or 0
      if (num < 1) return undefined;
      // initialize a variable to hold our result as we build it up
      var product = 1;
      // iterate through the relevant numbers, mulitplying product by each
      for (var i = num; i > 1; --i) {
        product *= i;
      }
      return product;
    };

### Recursion With Side Effects
At first glance, this may look very different from our iterative solution. However, it works in a similar fashion, albeit using recursion instead of a for loop. We start by defining a variable to keep track of our running result, and an inner function that will do our recursion for us (causing side effects to the result variable each time it is run).

    var factorialWithSideEffects = function(num) {
      // user is a jerk, return undefined
      if (num < 1) return undefined;

      // initialize a variable on which we will be applying side effects
      var product = 1;
      var muliplyAndRecurse = function(n) {
        // base case
        if (n === 1) {
          return;
        }
        //recursive case
        product = product * n;
        muliplyAndRecurse(n - 1);
      }

      // call our inner recursive function to perform side effects
      // on product, then return the newly changed product
      muliplyAndRecurse(num);
      return product;
    };

### Recursion _Without_ Side Effects
Recursion purists will probably tell you that this is the only "true" recursive function, as we do not cause any side effects to external variables. In this case, we instead simply multiply `num` by the result of the recursive call on `num - 1`. This in turn keeps recursing until `num === 1`, at which point the calls bubble back up the stack and return our desired answer.


    var factorialWithoutSideEffects = function(num) {
      // user === jerk, return undefined
      if (num < 1) return undefined;
      // base case
      if (num === 1) return 1;
      // recursive case
      return num * factorialWithoutSideEffects(num - 1);
    };

### What's the Difference?
Well, not that much really. They all get you to the same answer, and can all be used more or less as a black box to get there. Sure, recursion without side effects looks more elegant here, but the reality is that it depends a great deal on the problem you are trying to solve. In my case, I choose whichever seems to lend itself best to the task at hand, and further, which solution provides the best readability. Because when I come back to look at my code in a month or two, I don't care how elegant it is if I can't tell what's going on!
