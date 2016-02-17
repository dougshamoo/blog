---
layout: post
title:  "React components and JSX"
date:   2016-1-29 21:45:00
categories: es2015 react jsx
disqus: true
---

In my [last post](http://dougshamoo.github.io/webpack/es2015/react/2016/01/15/how-to-get-started-with-webpack.html), I talked about how to use webpack to get started on that React project you've been putting off. Certainly by now you've started it. So it's obviously the perfect time to talk a little bit about React and JSX, while implementing some components!

First, why React? Of course you know already, that's why you started that React project I already alluded to, but just in case your friends ask... React does some things pretty well:

* React generates a lightweight representation of your view on the initial `render`. When `render` is called again (perhaps when your data changes), React creates a diff between the old return value and the new one in order to generate a minimal set of changes to be applied to the DOM.

* React doesn't use HTML templates, it relies entirely on JavaScript, meaning that you're still just writing JavaScript code. By virtue of JavaScript's versatility, we're able to build abstractions into our React applications the same way we would do it in other JavaScript programs.

* Because it's all JavaScript, we're writing our view markup and view logic in the same place, making our application's views easier to understand, extend, and maintain.

What about JSX? Do you have to write JSX in order to use React? Absolutely not, but I find it pretty awesome and intuitive, and I think you might, too. JSX has the added benefit of looking pretty similar to HTML (even though it's JavaScript), which means that it's not much of a change from what you're used to looking at.

Here's an example from the React docs of JSX and the corresponding JavaScript that it will be translated into:

~~~javascript
/*====================================/
/                JSX                  /
/====================================*/
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        Hello, world! I am a CommentBox.
      </div>
    );
  }
});
ReactDOM.render(
  <CommentBox />,
  document.getElementById('content')
);

/*====================================/
/            Raw JavaScript           /
/====================================*/
var CommentBox = React.createClass({displayName: 'CommentBox',
  render: function() {
    return (
      React.createElement('div', {className: "commentBox"},
        "Hello, world! I am a CommentBox."
      )
    );
  }
});
ReactDOM.render(
  React.createElement(CommentBox, null),
  document.getElementById('content')
);
~~~

The differences here are pretty small, however, when you start building bigger and bigger components (with sub-components nested inside them), JSX tends to be a lot easier to parse quickly and comprehend.

Recently, I've been working on [Brainado](http://brainado.herokuapp.com), a visual brainstorming app that uses React and d3 to render a web of associated words. React + d3 was an interesting challenge as they would both really like to control the DOM. That's a post for another day, but I'll leave you with a link to the [source code on github](http://github.com/dougshamoo/brainado) if you'd like to check out some interesting interactions between React components, all of which are written using JSX of course.