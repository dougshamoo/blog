---
layout: post
title:  "How to get started with Webpack"
date:   2016-1-15 17:45:00
categories: webpack es2015 react
disqus: true
---

So let's say you've heard all the awesome things about React and you want to get in on the action. Let's say you also want to use es2015 (es6) to write your awesome stateless React components. Enter [webpack](https://webpack.github.io/), a module bundler that wraps up all those fun dependencies so that they can be served in the browser!

If you've never used webpack before, then you might imagine some complex, fire-breathing monstrosity of a library that so much time to figure out that it isn't worth integrating into your projects. But you're wrong! It's actually pretty simple to get started with webpack and there are plenty of options for extending the basic functionality as well.

So let's get started on that React project that you are thinking about starting. Stop procrastinating already! The first thing we need to do is create our new project and go on a short `npm install` spree. Now, we're definitely going to need React...

~~~
npm install --save react react-dom
~~~

Next, we'll need webpack, of course, and we'll use babel to transpile all our React code into standard es5. Also, we'll probably want to be able to access es2015 features, just in case we're in the mood later (and because they're awesome)...

~~~
npm install --save-dev babel-core babel-loader babel-preset-react babel-preset-es2015 webpack
~~~

Quick aside about loaders: Loaders are basically functions that transform modules from one language into JavaScript. They also allow you to do cool things like `require` css stylesheets in your JavaScript modules.

Additionally, we're probably going to want to install webpack globally as well, so that we can use the cli to bundle our modules together and build our app. So...

~~~
npm install -g webpack
~~~

Now, we're ready to get started... and we're almost done! The first thing we need to do is create a `webpack.config.js` file in the root directory of our project. It will end up looking something like this:

~~~ javascript
module.exports = {
  entry: './app/entry.js',
  output: {
    filename: 'public/bundle.js',
  },
  module: {
    loaders: [
      { 
        test: /\.js$/,
        loader:'babel',
        exclude: /(node_modules|bower_components)/,
        query: {
          presets: ['react', 'es2015']
        }
      },
    ]
  }
};
~~~

In its simplest form, all we need is:
* An entry point for our app, which for react is where we will render our main app component.
* A location/name for our bundled output file, which we will load in our `index.html` once we want to display something.
* And our loaders that are responsible for transpiling all our various modules into JavaScript.

From here, we can do a bunch of different things, but we'll always need to build our app with webpack after we make changes, so we can see the changes reflected in the bundled output. To do this, we'll use the following command:

~~~
webpack
~~~

Or, if you like pretty colors, progress updates, and automatic bundling when you save changes in your app:

~~~
webpack --watch --colors --progress
~~~

Stay tuned for the next post, in which we'll start talking about putting together a basic React apps and a component or two. Happy webpacking!