---
layout: post
title:  "Easy Continuous Deployment: Node/Express App on Heroku"
date:   2015-10-17 14:30:00
categories: javascript
---

In this post, I'm going to run through the quickest and easiest steps to deploy your Node/Express app on Heroku and setup continuous deployment from your github repo.

First things first, this article assumes that you already have the following set up:
- A working (more or less) Node/Express project
- A github account
- A github repo for the project that you want to deploy
- A Heroku account
- A desire to spend as little time as possible on deployment


###Setup
#####Create a New Heroku App
The first thing that you're going to want to do is create a new app on heroku. You can think of this as the heroku equivalent of the github repo for your project. Just click the `+` button at the top-right corner of your dashboard and then `Create New App`.

![Alt text](http://dougshamoo.com/assets/images/heroku-new-app.png)

#####Procfile
You're also going to need something called a Procfile, which tells Heroku how to build your app. In this case, we just need to create a file called `Procfile` (no file extension, this will only cause you woe and suffering) in the root directory of our project and put the following line in it:
  
    web: node index.js

Save it but don't commit and push yet, there's still a little more to do...

#####Environmental Variables
Your express server might be listening on a hardcoded port for development purposes. It might also be using a slew of API keys and secrets that are specifically for local testing. The rule of thumb is roughly this: if something should be different on the Heroku server then it is on your local machine, you should store it in a environmental variable. So get used to writing lines like this:

    var port = process.env.PORT || 3000;
    app.listen(port);

Here's an example with some made up API keys for a made up API called ThisIsAnAPI:
    
    var apiKey, apiSecret;

    if (process.env.THIS_IS_AN_API_KEY && process.env.THIS_IS_AN_API_SECRET) {
      apiKey = process.env.THIS_IS_AN_API_KEY;
      apiSecret = process.env.THIS_IS_AN_API_SECRET;
    } else {
      var config = require('./config.js');
      apiKey = config.thisIsAnAPI.key;
      apiSecret: config.thisIsAnAPI.secret;
    }

This pre-supposes that you have your keys for local development stored in a config file, which you should be doing. Aren't you?!? Seriously, just do it.

Aside from the port, which _should_ be automatically handled by Heroku, you will need to specify an "config variable" for each of the above items.

###Get Deployed
Coming Soon...

###Set up Continuous Deployment
Coming Soon...
