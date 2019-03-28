---
title: Breaking Down Downshift
date: 2019-03-26T20:34:07+00:00
author: Clayton Salem
layout: post
permalink: /breaking-down-downshift/
---

I was recently tasked with creating a typeahead selector for a field-matching interface that contained around 75 items. The current iteration of the interface featured a `select` with 75 `options`. It was painful to use and implementing a typeahead would allow users to get through the task quicker.

I experimented with writing a custom React Component, or to be specific, taking a currently implemented custom typeahead, generisizing it, and then wrapping it with the particular logic on this page. I got about 80% of the way to complete, but was missing any accessibility, and was making extensive use of `refs` and binding to get it to work.

I changed directions and found the (Downshift project)[https://github.com/downshift-js/downshift]. This library provides all the edge-case interactions, as well as the accesibility tools that I would probably never get right.

Before I go further, I did a fair amount of reading to get started with this component. If you'd like to follow that path here's what I read/watched:

- [READ THE DOCS!](https://github.com/downshift-js/downshift)
- Kent Dodds on the [state reducer pattern](https://youtu.be/AiJ8tRRH0f8?list=PLV5CVI1eNcJgNqzNwcs4UKrlJdhfDjshf)
- Chris Nwamba did a thing on [scotch.io](https://scotch.io/tutorials/5-most-common-dropdown-use-cases-solved-with-react-downshift)
- READ THE DOCS! ([again](https://reactjs.org/docs/render-props.html)): 

<br>

The challenge is that the library uses the render-props pattern. 

_Here's my caveat that yeah, everyone is probably over render-props and has moved onto hooks. That's okay, I'm 2 years behind as usual_ 😉

### The Challenge
One of the challenges of this pattern is that it forces the developer to do most of the heavy-lifting of controlling the element rendering, Downshift only provides the attributes and handlers. 

The example from the documentation looks like this:

```(js)
import React from 'react'
import {render} from 'react-dom'
import Downshift from 'downshift'

const items = [
  {value: 'apple'},
  {value: 'pear'},
  {value: 'orange'},
  {value: 'grape'},
  {value: 'banana'},
]

render(
  <Downshift
    onChange={selection => alert(`You selected ${selection.value}`)}
    itemToString={item => (item ? item.value : '')}
  >
    {({
      getInputProps,
      getItemProps,
      getLabelProps,
      getMenuProps,
      isOpen,
      inputValue,
      highlightedIndex,
      selectedItem,
    }) => (
      <div>
        <label {...getLabelProps()}>Enter a fruit</label>
        <input {...getInputProps()} />
        <ul {...getMenuProps()}>
          {isOpen
            ? items
                .filter(item => !inputValue || item.value.includes(inputValue))
                .map((item, index) => (
                  <li
                    {...getItemProps({
                      key: item.value,
                      index,
                      item,
                      style: {
                        backgroundColor:
                          highlightedIndex === index ? 'lightgray' : 'white',
                        fontWeight: selectedItem === item ? 'bold' : 'normal',
                      },
                    })}
                  >
                    {item.value}
                  </li>
                ))
            : null}
        </ul>
      </div>
    )}
  </Downshift>,
  document.getElementById('root'),
)
```
👀👀! That's a lot of syntax to unpack.

Instead of trying to explain the pattern, the inversion of control or the higher level abstraction this allows, I'm going to merely walk through some simple-simple examples to give a feel for what the results are.

### Keep it simple

```
class App extends React.Component {
    render() {
        return (
            <Downshift>
              {function () {
                    <div className="my-rendered-div"></div>
                }
              }
            </Downshift>
        )
    }
}
```
Yeah, it could be a functional stateless component, but let's be real, it's actually easier for beginners to grok React patterns using class declaration. I don't make the rules, but I know a lot of us come from an OOP background and are more comfortable this way. (And, yes, I've converted an arrow function to a non-arrow, again with the beginner in mind. I will probably drop that as we go on.)

As you can see, I'm putting a function that returns a `div` element inside the Downshift component. I'm adding that class so that it becomes very obvious what get's rendered out the other side. Speaking of the other side, here's what shows up in the browser:

```
<div class="my-rendered-div" role="combobox" aria-expanded="false" aria-haspopup="listbox" aria-labelledby="downshift-0-label">
</div>
```
Holy cow, any knowledge of aria attributes I might've thought I had is already surpassed. And just to recap, Downshift has taken my first element and injected a number of attributes, both `aria` and a `role`.

Moving on:
```
<Downshift>
    {function () {
        return <div className="my-rendered-div">
            <input />
        </div>
    }
    }
</Downshift>
```

Oooooohh, input field! I'm adding an input field, because that's ultimately what we want the user to type into. The output doesn't change much.

```
<div class="my-rendered-div" role="combobox" aria-expanded="false" aria-haspopup="listbox" aria-labelledby="downshift-0-label">
  <input>
</div>
```

### Props to you for reading this far
Let's start adding a props getter.
```
<Downshift>
    {function ({ getInputProps: getInputProps }) {
        return (<div className="my-rendered-div">
            <input {...getInputProps()} />
        </div>)
    }
    }
</Downshift>
```
_[scratching record] wai-whaaaaat?_

Let's take it slow. We are now declaring an object parameter to our function:
```
function ({ getInputProps: getInputProps }) 
```

One thing that drives me batty when learning a new thing is understanding what is a built in named method, (a "proper" method if you will) and what is a method named for the example code, and more imporantly, where do you learn about those "properly" named methods. Well, `getInputProps` _is_ a "proper" method of the Downshift component. It's one of a group of "[prop getters](https://github.com/downshift-js/downshift#prop-getters)" used to get the attributes and handlers for our markup.

_Dumb note you might ignore: I could rename the method to use within my function if we use the non-shorthand object literal syntax: `{getInputProps: myConfusinglyNamedMethod}`, but why would I want to do that?_

Now that we have this prop-getter available in our function, we can call it, and "spread out" the results with the spread(...) operator:

```
<input {...getInputProps()} />
```
This takes the object that is the result of the function call, spreads out each key value pair into props on the input component. The result is this beauty: 
```
<input aria-autocomplete="list" aria-labelledby="downshift-0-label" autocomplete="off" id="downshift-0-input" value="">
```
More awesome aria attributes, a proper id and values. Plus, if you `console.log(getInputProps())`. You'll see the methods we're adding to the input:
```
{
    aria-activedescendant: null
    aria-autocomplete: "list"
    aria-controls: null
    aria-labelledby: "downshift-0-label"
    autoComplete: "off"
    id: "downshift-0-input"
    onBlur: ƒ (event)
    onChange: ƒ (event)
    onKeyDown: ƒ (event)
    value: ""
}
```
### Now we're cooking with fire!

I want to go over one more thing with the prop-getters, you can add and overwrite attributes, methods, by passing them into the function call.
```
<input {...getInputProps({ className: 'mycoolinput', onBlur: () => {console.log('you blurred!')} })} />
```
In this example, I'm adding a `className` and overwriting the `onBlur` handler Downshift supplies. Pretty cool!

In the full example you can see all the use of these prop getters, one each for the menu, label, input and item.

```
<label {...getLabelProps()}>Enter a fruit</label>
<input {...getInputProps()} />
<ul {...getMenuProps()}> /// etc.
```
### Pieces of State
There are a few more thing passed into the example function. These include `isOpen`, `inputValue`, `highlightedIndex`, `selectedItem`. These are [state values](https://github.com/downshift-js/downshift#state) that are useful in rendering the component.