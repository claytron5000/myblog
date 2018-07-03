---
id: 101
title: Easy Peasy React
date: 2018-07-02T19:06:00+00:00
author: Clayton Salem
layout: post
guid: http://claytonsalem.com/claytonsalem/?p=97
permalink: /slide-n-scroll-fun/
categories:
  - Technology
---
Last week on twitter [Dan Abramov](https://twitter.com/dan_abramov/status/1011995700748521477) was highlighting some of the easy ways to get your feet wet with React. I've been playing with React for a while, and had heard the idea that one could add React as an individual component, or widget, to an already existing site. This looked like a good opportunity to try that out.

Caveat before we begin, the docs are great, there's nothing I'll do here that isn't covered in the docs. Instead of reading this you should go read [the docs](https://reactjs.org/docs/hello-world.html).

Oh, hi there, you're still here. I'm going to start with something very close to the [example](https://reactjs.org/docs/add-react-to-a-website.html). Essentially, a small "like" button widget. We'll start with one that increments clicks.

I was trying to decide if I should include my semi-controversial opinion about using CDN's. That is, don't use them. When I loaded the `unpkg` links in the site and... they were throwing 502 errors. So, here's my opinion: why trust a third party to always have the resources you need? Serve it yourself.

But.... this is a demo project, and avoiding webpack, task runners, watchers... let do it!
<pre>
  src="https://cdnjs.cloudflare.com/ajax/libs/react/16.4.1/umd/react.development.js" crossorgin>
  src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/16.4.1/umd/react-dom.development.js" crossorgin>
</pre>

First we're going to try React without JSX. I had some vague notion that JSX and React were different, but all the todo apps, or side project apps I'd built, I hadn't really considered one without the other. It turns out JSX looks a lot like a wrapper for `React.createElement`. But, let's start at the top:

Our LikeButton class.
<pre>
class LikeButton extends React.Component {
</pre>

Let's take the docs example, which is a button that changes into "You liked it", we'll make it count up as clicked.
<pre>
constructor(props) {
  super(props);
  this.state = { counter: 0 };
}
</pre>

And the render:
<pre>
  render() {
    let buttonText = 'Like'
    if (this.state.counter > 0) {
      buttonText = 'Liked ' + this.state.counter + ' times';
    }
    return React.createElement( // createElement takes 3 arguments
      'button', // the element type
      // The element's configuration, in this case the click handler
      { onClick: () => this.setState({ counter: this.state.counter + 1 }) },
      buttonText // and the element's children, here we're adding text
    );
  }
</pre>

  Finally, we select the widget container and render in our LikeButton
  <pre>
    const domContainer = document.querySelector('#like_button_container');
    ReactDOM.render(React.createElement(LikeButton), domContainer)
  </pre>
