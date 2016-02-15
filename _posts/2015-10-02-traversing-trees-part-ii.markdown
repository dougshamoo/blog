---
layout: post
title:  "Traversing Trees: Part II"
date:   2015-10-02 22:00:00
categories: javascript trees traversal breadth-first
disqus: true
---

A while back I wrote a post about [traversing tree data structures with recursion](http://dougshamoo.github.io/javascript/recursion/trees/traversal/depth-first/2015/09/17/traversing-trees-with-recursion.html). For those of you who are especially astute readers, you'll have noticed that I alluded to the possibility of a breadth-first traversal of the tree as opposed to the depth-first traversal that I showed in that post.

Before we begin and for the uninitiated, let's concisely define these two traversal methodologies:

depth-first traversal
: Traversal that starts at the tree root and explores as far as possible along each branch before backtracking. Visually speaking, depth-first traversal prioritizes vertical exploration over horizontal exploration.

breadth-first traversal
: Traversal that starts at the tree root and explores the direct children nodes first, before moving to the next level of children. Visually speaking, breadth-first traversal it prioritizes horizontal exploration over vertical exploration.

Let's use the same example as last time to solidify our understanding. Here's our definition of a tree again:

    var Tree = function(value) {
      this.value = value;
      this.children = [];
    };

Now, let's build out a tree that will be easy to reason about:

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

In the above example, Depth-first traversal will generally be implemented to visit the nodes in the following order: 1-2-4-5-3-6-7. The idea here is that it will go as far as it can down a branch until it gets to a leaf, then backtracks to find another node it hasn't been to yet, and repeats.

Breadth-first traversal on the other hand will generally be implemented to visit the nodes in the following order: 1-2-3-4-5-6-7. The idea in this case, is to check a whole level (ie depth=1), before moving onto the next deepest level, repeating until it has visited the lowest and final level.

We can define a function `logAllNodeValuesBF`, that (just like last time) logs the node value of each node in the tree, except this time with breadth-first traversal. We will achieve this by using a [queue](https://en.wikipedia.org/wiki/Queue_(abstract_data_type)), or rather the methods of a JavaScript array that provide queue-like functionality (namely `shift`).

    var logAllNodeValuesBF = function(node) {
      // instantiate our "queue" with the root node already in it
      var q = [node]

      while (q.length > 0) {
        // dequeue the first node in our queue and log the value
        var current = q.shift();
        console.log(current.value);

        // add the node's children to the queue
        for (var i = 0; i < current.children.length; i++) {
          q.push(current.children[i]);
        }
      }
    };

    logAllNodeValuesBF(treeRoot);  // 1 2 3 4 5 6 7

Because of the functionality a queue gives us, we will visit and log the value of nodes in the order that they are added to the queue. And when we run out of nodes in our queue and nodes to add to our queue, we're done!

Of course, at this point we're still just logging values, which is pretty boring. Stay tuned for another post on how we can tweak our breadth-first *or* depth-first traversal to do anything we want to each node. _psst_ - the secret is higher order functions.
