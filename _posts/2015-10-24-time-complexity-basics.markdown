---
layout: post
title:  "Time Complexity: the Basics"
date:   2015-10-24 14:00:00
categories: javascript cs theory
---

It's time to talk about everyone's favorite subject, time complexity. In this post, we're just going to quickly cover the basics so you can say something interesting at your next project meeting.

### Quick Intro

Time complexity is a solid way to theoretically measure the efficiency of an algorithm. Sure, you could write the code and then test how long it takes to run against different input sets. But wouldn't it be awesome to be able to estimate the efficiency before you implement the actual code itself? Yes, yes it would.

Big O notation is one way that we can represent time complexities and compare them with one another. In particular, Big O notation categorizes functions according to the rate of growth as you add larger input sets. In computer science, time complexity is usually measured relative to the size of input, which we'll call `n` below.

### The Big Ones
Here are some of the more common time complexities (there are others like n! that come up every once in awhile, but don't worry about it for now).
Roughly in oder of best to worst (with explanation of the more pertinent ones):

    O(1) - Constant time. Always takes the same amount of time, no matter the size of the input.

    O(log(n)) - Better than O(n). Worse than O(1). Likely at least part of the time complexity for any algorithm with "binary" in the name.

    O(n) - Linear time. Each new item added to input carries the same time impact/increase as the one before it. Ex. Doing something O(1) in a single for loop that loops through all inputs.

    O(nlog(n)) - Better than O(n^2). Worse than O(n).

    O(n^2) - Quadratic time. If the input size doubles, time goes up by a factor of 4. Ex. Doing something constant time inside a for loop nested inside another for loop. Or doing something with linear time inside of a for loop.

Anything after this is pretty undesirable, so I am omitting it for brevity.

The big takeaway is constant time is better than linear time is better than polynomial time.

### Examples
Let's get into some real life examples.

##### Retrieval of value from a hash table
    /*
     Complexity: This function has O(1) time complexity because hash table lookups are done in constant time. No matter how big the hash table is, the key simply needs to be converted to a hash and used to lookup the value. The conversion of the key to the hash depends on the size of the key, not the size of the hash table.
     */

    var retrieve = function(key){
      var hash = 0;
      for (var i = 0; i < key.length; i++){
        hash = (hash + Math.pow(i,hash)) % array.length;
      }
      return array[hash];
    };
##### Checking a sorted array for an item
    /*
     Complexity: This function has O(log(n)) time complexity. Each time we recurse, we cut the array in half, which we are able to do because it is sorted. In the worst case, we divide by 2 until we only have 1 element left, which is log(n) steps.
     */

    var sortedArrayContainsItem = function(array, item){
      var center = Math.floor(array.length / 2);
      if (array[center] === item){
        return true;
      }
      var halfOfArray = item < array[center] ? array.slice(0, center) : array.slice(center);
      return sortedArrayContainsItem(halfOfArray, item);
    };

##### Checking an array for duplicates
    /*
     Complexity: This function has O(n^2) time complexity. First we have the outer for loop that loops through every item in the array (O(n)). Then we have an inner slice that creates a new array starting from the next item and going to the end (O(n)), in addition to an indexOf which iterates through each item of the sliced array (O(n)). O(2n) can be simplified to O(n) for the inside block of the for loop. O(n) happening O(n) times => O(n^2).
     */

    var hasDuplicates = function(array){
      for(var i = 0; i < array.length; i++){
        var item = array[i];
        if(array.slice(i+1).indexOf(item) !== -1){
          return true;
        }
      }
      return false;
    };

### Conclusion
And there you have it, the basics aren't that hard. Now go forth and argue with your co-workers about premature optimization! I believe in you.
