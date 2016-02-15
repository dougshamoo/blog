---
layout: post
title:  "Traversing Trees with Recursion"
date:   2015-09-17 21:30:00
categories: javascript recursion trees traversal depth-first
disqus: true
---

Today, I want to explore a simple and effective way to visit every node of a tree using recursion.

Let's say that we have the following definition of a Tree:

    var Tree = function(value) {
      this.value = value;
      this.children = [];
    };

Now, let's instantiate one and add some branches.

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



We can define a function `logAllNodeValues`, that... you guessed it, logs the node value of each node in the tree. We'll achieve this by recursing through all of the nodes and console.logging the value 

    var logAllNodeValues = function(node) {
      console.log(node.value);
      if (node.children.length === 0) return;
      for (var i = 0; i < node.children.length; ++i) {
        logAllNodeValues(node.children[i]);
      }
    };

    logAllNodeValues(treeRoot);  // 1 2 4 5 3 6 7
    

And that's all there is to visiting all the nodes in a tree. Notice the order in which the nodes are logged. For those that are interested, this is a depth-first traversal of the tree, but with some tweaks we could accomplish the same traversal in breadth-first fashion. Stay tuned for my next post, in which I will upgrade the function above to perform an arbitrary operation on each node of the tree