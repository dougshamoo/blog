---
layout: post
title:  "Angular Services vs Factories"
date:   2015-09-26 16:30:00
categories: javascript angular frontend
---

Angular is super auto-magical and awesome. But let's be honest, the Angular ecosystem can be a large, confusing, and somewhat irritating place when things don't work as you might expect them to. In this post, we'll uncover the mysterious secret about angular factories and services that will finally allow you to sleep at night again.

The secret that Angular doesn't want you to know is that services and factories are basically the same thing with different syntax. In most use cases, they behave identically, you just have to define them differently.

For example, let's take a look at the following barebones Angular app:

    angular.module('myApp', [])

    .controller('mainCtrl', function($scope) {
      $scope.display = 0;
    })

Now, let's say we want to keep track of a count variable to display with the controller, but we don't want to store it in our controller directly (lest we be less angular-y). Enter the factory!

    angular.module('myApp', [])

    .factory('countFactory', function() {
      var obj = {};
      obj.count = 0;
      return obj;
    })

    .controller('mainCtrl', function($scope, countFactory) {
      $scope.display = countFactory.count;
    })

But did you know that you can do the exact same thing with a service?!? Just change a couple words and remove the extra lines.

    angular.module('myApp', [])

    .service('countService', function() {
      this.count = 0;
    })

    .controller('mainCtrl', function($scope, countService) {
      $scope.display = countService.count;
    })

You might notice some similarity to the difference between functional and pseudoclassical class patterns in javascript.

Bonus TODO: Demystifying all the weird brackets in angular controller, service, factory definitions.