---
id: 101
title: Easy Peasy React Cheesy
date: 2018-07-04T08:06:00+00:00
author: Clayton Salem
layout: post
guid: http://claytonsalem.com/claytonsalem/?p=101
permalink: /easy-peasy-react-cheesy/
categories:
  - Technology
---
Last week on twitter [Dan Abramov](https://twitter.com/dan_abramov/status/1011995700748521477) was highlighting some of the easy ways to get your feet wet with React. I've been playing with React for a while, and had heard the idea that one could add React as an individual component, or widget, to an already existing site. This looked like a good opportunity to try it out.

Caveat before we begin, the docs are great, there's nothing I'm expanding here on what's already covered in the docs. If you want to learn React, go read [the docs](https://reactjs.org/docs/hello-world.html).

## Easy Incrementor

![simple counter](/images/2018/counter1.gif)

Oh, hi, you're still here. I'm going to start with something close to the [example](https://reactjs.org/docs/add-react-to-a-website.html). Essentially, a small "like" button. However, I am upping the ante and instead of a simple, "You liked it", we'll increment the "likes" on the button.

The beauty of this process is that we're including React libraries directly by `script` tag. No preprocessors, no webpack, we can even avoid `npm`.

>I was trying to decide if I should include my semi-controversial opinion about using CDN's, that is, don't use them, when I loaded the `unpkg` links in the site and... they were throwing 502 errors. So, here's my opinion: why trust a third party to always have the resources you need? Serve it yourself.

But.... this is a demo project, and avoiding webpack, task runners, watchers... let do it! Here is a for real-life-already-existing website:
```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>For Real Website</title>
</head>
<body>
<h1>A Real Website</h1>
<p>With lot's of content, and other cool stuff.</p>
<div id="like_button_container"></div>
<footer>ouch, I stubbed my footer</footer>
<!-- Here's React! -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/react/16.4.1/umd/react.development.js" crossorgin></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/16.4.1/umd/react-dom.development.js" crossorgin></script>
<!-- and here's the code -->
<script src="./dist/like_button.js"></script>
</body>
</html>
```

First we're going to try React without JSX. I had some vague notion that JSX and React were different, but I haven't really considered one without the other. It turns out JSX looks a lot like a wrapper for `React.createElement`. `createElement` takes three arguments: the element type, the configuration (which in this case it going to be simply the click handler), and the element's children (in this case the button text).

Our LikeButton class.
<pre><code>
class LikeButton extends React.Component {
</code></pre>

In the constructor we'll set the counter to 0.
```
constructor(props) {
  super(props);
  this.state = { counter: 0 };
}
```
Next the render, which sets the text for the button, and returns the `createElement`.
```
  render() {
    let buttonText = 'Like'
    if (this.state.counter > 0) {
      buttonText = 'Liked ' + this.state.counter + ' times';
    }
    return React.createElement( // createElement takes 3 arguments
      'button', // the element type
      { onClick: () => this.setState({ counter: this.state.counter + 1 }) }, // config
      buttonText // children, in this case text, but it could be another element.
    );
  }
```

  Finally, we select the widget container and render in our LikeButton
  <pre><code>
    const domContainer = document.querySelector('#like_button_container');
    ReactDOM.render(React.createElement(LikeButton), domContainer)
  </code></pre>

## Uping the Game

![squad goals](/images/2018/counter2.gif)

I have some problems with this widget. First, it only goes up, thanks Facebook, and second, it's ephemeral and the values disappear as soon as the page is refreshed. I'm going to up the game: add two button, increment and decrement (plus and minus to you plebians), and store the values in a database.

### Add JSX
As simple as the `React.createElement` appears, once we start nesting elements it's going to get messier. I like JSX for the reason that it nests like HTML or XML. In the docs we're told, though not recommended, that we can add JSX support from the CDN... I guess that means it will compile the JSX on-the-fly. That seems a little nuts, so we're going to add a little compiler on the development side. You might compare this to compiling Sass. This part is covered in the docs, though my config ended up a bit different.

<pre><code>
npm i babel-cli
npm i babel-preset-react
</code></pre>

then `npx` will watch the source directory and output to the dist directory.
```
npx babel --watch src --out-dir dist --presets react
```

Update the render, we're wrapping a div around two buttons and some text. If you run the following code through the [online babel compiler](https://babeljs.io/repl/#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=MYGwhgzhAEDCD2BXAdgFwKYCdroB4eQBMYAldMYVAOgQFsAHeZdNaAbwChppMXCsAFAEp2XbtBDpU0YElYBeaKgAWASwhUIqMBiqyUqANwcx3XqkSZk0AafEAeQqoBuAPjvjo9gEaJUqJmgmWBBVYABreTYBdBF5VyU1DWUwIkkQsPCBAEYhAF9XAGp7AHpffyZ3T082fTQ8jwdygOtg0IiomLiElXUqFLT0DIiBAQBaXPzXMYBIUubKxtKnNzshY24GvKA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&sourceType=module&lineWrap=true&presets=react&prettier=false&targets=&version=6.26.0&envVersion=) you can start to see the connection between JSX and `React.createElement`.
```
render() {
    let count = this.state.count;

    return (
      <div>
        <button onClick={(e) => this.handleClick(1)}>+</button>
        {count}
        <button onClick={(e) => this.handleClick((-1))}>-</button>
      </div>
    );
  }
  ```
And in order to handle the click and update the state:
```
handleClick(num) {
    let newCount = this.state.count != null ? this.state.count + num : 1;
    this.setState( { count: newCount } )
  }
```
I'm working backward here, just to be confusing.

### Firebase it up!

At this point I have a two button counter that works in one session on one page. Let's up the game with a firebase backend. To keep it simple I'm setting the thing to open read-write, and basically it has one field, `count`. And to connect to said database I'm using `fetch`. [Firebase](https://console.firebase.google.com/?pli=1) is great, I'm not going to explore it here, let's just look at it like a dummy endpoint.

My first try, I put it in
```
componentDidMount() {
    fetch('https://myfirebasebackend')
      .then(res => res.json())
      .then(data => this.setState({ count: data }));
}
```
More about [componentDidMount](https://reactjs.org/docs/react-component.html#the-component-lifecycle) and the lifecycle methods.

In the render I want to check if the count is set yet, if not, on first load, I'm putting in a placeholder.
```
let count = this.state.count != null ? <span>{this.state.count}</span> : <span>_</span>;
```

Next, update the click handler to update the database with a new value.
```
handleClick(num) {
    let newCount = this.state.count != null ? parseInt(this.state.count) + num : 1;
    this.setState( { count: newCount } )
    fetch('https://myfirebasebackend', {method: 'PUT', body: JSON.stringify(newCount)})
  }
```
And, yes, I'm doing two things at once in a function. How non-functional of me! Seriously, though, this _feels_ wrong, if you know a better, or more functional way to do this, let me know.

We're close, but the value from one browser won't update in another browser, we need to get data from the database in real time. In this case, every second I'll fetch data. First move the fetch `GET` into a function:
```
tick() {
    fetch(COUNTERAPI)
    .then(res => res.json())
    .then(data => this.setState({ count: data }));
  }
```
Then call the tick through `setInterval` from within `componentDidMount`, and clear it in `componentWillUnmount`:
```
componentDidMount() {
    this.timerID = setInterval(() => this.tick(), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```
And... I think that's about it. I may have missed a couple things, so here's the whole shebang:

```
'use strict';

const COUNTERAPI = 'https://myfirebasebackend'
class Counter extends React.Component {
  constructor(props) {
    super(props);

    this.state = {};
    this.handleClick = this.handleClick.bind(this);
  }

  componentDidMount() {
    this.timerID = setInterval(() => this.tick(), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    fetch(COUNTERAPI).then(res => res.json()).then(data => this.setState({ count: data }));
  }

  handleClick(num) {
    let newCount = this.state.count != null ? parseInt(this.state.count) + num : 1;
    this.setState( { count: newCount } )
    fetch(COUNTERAPI, {method: 'PUT', body: JSON.stringify(newCount)})
  }

  render() {
    let count = this.state.count != null ? {this.state.count} : '_';

    return (
      <div>
        <button onClick={(e) => this.handleClick(1)}>+</button>
        {count}
        <button onClick={(e) => this.handleClick((-1))}>-</button>
      </div>
    );
  }
}

let domContainer = document.querySelector('#like_button_container');
ReactDOM.render(<Counter />, domContainer);
```

Thanks, dear reader, if you got this far. The question I set out to answer was, is it easy to add React to an existing site? I think the answer is, yes, it is. But if you want to do anything more complicated than a simple counter, you'll probably want to add JSX, and the compiler. While it's nice that I can compile JSX with relative ease. I think the actual use-case for this in production is pretty limited. After all, I think most JavaScript developers could implement a simple counter without the excess weight of the React libraries. Maybe if I wanted to add a very complicated widget to an existing site, or a situation where I want to slowly transition a site from server-render to client site render.

And this leaves un-expolored the actual challenging part of a real website, which will have some Javascript functionality already in place, probably jQuery. Keeping those the jQuery seperate from React would be a whole other interesting challenge.

What this process _is_ great for is learning. Getting up and running in react in just a few minutes is a real pleasure. And it was fun to write some "plain React" without JSX.
