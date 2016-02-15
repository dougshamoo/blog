---
layout: post
title:  "Traversing Trees: Part II"
date:   2015-10-02 22:00:00
categories: javascript trees traversal breadth-first
disqus: true
---


It's been awhile since I last talked about trees, but I have one more quick post for those that are interested in generalized functions for tree traversal and manipulation.

The last two posts ([Part I](http://dougshamoo.github.io/javascript/recursion/trees/traversal/depth-first/2015/09/17/traversing-trees-with-recursion.html), [Part II](http://dougshamoo.github.io/javascript/trees/traversal/breadth-first/2015/10/02/traversing-trees-part-ii.html)) went over depth-first and breadth-first traversal, respectively. This time, we'll throw in a small tweak to ensure we can do something more interesting than simply logging values to the console. In fact, we'll make sure that we can do anything we want to each node, by implementing our traversal functions as [higher order functions](https://en.wikipedia.org/wiki/Higher-order_function).

Once again, we define our tree:

    var Tree = function(value) {
      this.value = value;
      this.children = [];
    };

Now, let's build one out:

    var treeRoot = new Tree(1);
    var branchTwo = new Tree(2);
    var branchThree = new Tree(3);

    treeRoot.children.push(branchTwo);
    treeRoot.children.push(branchThree);

    var branchFour = new Tree(4);
    var branchFive = new Tree(5);

    branchTwo.children.push(branchFour);
    branchTwo.children.push(branchFive);

    var branchSix = new Tree(6);
    var branchSeven = new Tree(7);
    
    branchThree.children.push(branchSix);
    branchThree.children.push(branchSeven);

This will create a tree that looks something like this:

          1
        /   \
       2     3
      / \   / \
     4   5 6   7

And here are our breadth-first and depth-first traversal-logging functions:

    function logAllNodeValuesDF(node) {
      console.log(node.value);
      
      if (node.children.length === 0) return;
      for (var i = 0; i < node.children.length; ++i) {
        logAllNodeValues(node.children[i]);
      }
    };

    function logAllNodeValuesBF(node) {
      var q = [node]

      while (q.length > 0) {
        var current = q.shift();
        console.log(current.value);

        for (var i = 0; i < current.children.length; i++) {
          q.push(current.children[i]);
        }
      }
    };

Now, all we really want to do is switch out our hardcoded `console.log`s with something that will allow us to perform arbitrary operations on a given node. Fortunately, JavaScript has [first-class functions](https://en.wikipedia.org/wiki/First-class_function), which essentially means we can pass them around willy-nilly just like any other variable. Let's see what we can do to integrate this into our functions.

    function doSomethingDF(node, callback) {
      callback(node);
      
      if (node.children.length === 0) return;
      for (var i = 0; i < node.children.length; ++i) {
        logAllNodeValues(node.children[i]);
      }
    };

    function doSomethingBF(node, callback) {
      var q = [node]

      while (q.length > 0) {
        var current = q.shift();
        callback(current);

        for (var i = 0; i < current.children.length; i++) {
          q.push(current.children[i]);
        }
      }
    };

Now, if we simply wanted to console.log our nodes again in depth-first fashion, we would simply pass our new function an anonymous function that does so:

    doSomethingDF(treeRoot, function(node) {
      console.log(node.value)
    });

But, we could also do other more interesting things... say we wanted to traverse our nodes, breadth-first, and add 1 to each node's value (also super contrived, but you get the idea):

    doSomethingBF(treeRoot, function(node) {
      node.value = node.value + 1;
    });

Hopefully, you can see that the possibilities are endless now that we have our generalized higher-order tree traversal functions. Time to go forth and do awesome things with your trees.
