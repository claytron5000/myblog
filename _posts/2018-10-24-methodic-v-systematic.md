---
title: Methodical versus Systematic
date: 2018-10-24T08:06:00+00:00
author: Clayton Salem
layout: post
permalink: /methodical-versus-systematic/
categories:
  - Technology
---
I recently was assigned a CSS layout problem. As I was working on it I realized I hadn't done CSS for a while. I also noticed I was thinking in a slightly different manner then when I'm writing PHP or JavaScript. I was thinking _systematically_ about the problem, instead of thinking _methodically_.

The CSS layout system (I'm talking here about last-generation pre-grid CSS layout) centers around the Box Model. What this means is an element is comprised of the content, padding, border and margins, in that order from inside out. An element takes up the space of its content's width and height (plus padding/border/margin), or its explicitly defined width and height. It's layout on the page is determined by the element preceding it in the same positional scope, or if there is no preceding element, its parent element. A common method for creating layouts in this system is to use the "float-based" layout system. This is the same as you might find under-girding the Bootstrap 3 framework.

This means, if you want to place the left hand side of an element 25% from the side of its container, and there's a preceding element, you will make the preceding element 25% wide. (Yes, you could position absolute, or you could use inline-block, or flex-box, or whatever layout system. But whatever you chose, you better stick with that system, lest you be cursed by the developer who comes after you.)

This is, if not counter-intuitive, probably not the most natural way to think about page layout. In most graphics programs, and certainly the real-world based systems, in order to put an element at 25% to the left, I would _place the element at the 25%_. Instead, I know to use the width of the preceding element because I've dealt with float-based layouts numerous times, and struggled with placing an element properly without messing up other parts of a layout. This is what I'm referring to as "systematic thinking", i.e. thinking inside a pre-defined system.

In PHP or JavaScript, I find I'm often using a different type of thinking. I'm methodically walking through a function in order to glean what goes in, what goes out, and any side-effects that might occur.

This is a much more unstable way of thinking, unmoored by strict rules and rigid frameworks. It's what most people think of when they think of programming. You might start at the top of the function, assign values to the arguments, then walk line-by-line through the function, mutating the values, or keeping track of side-effects in your head.

Advanced programming paradigms, such as Object Oriented seek to bring a more systematic approach to programming. When done right, class names and methods will have limited scope. They'll only do what they say they'll do. This allows a programmer to start thinking systematically about a problem instead of having to keep the mental threads all untangled.

I suspect the neural pathways are similar between these two styles of thinking. The difference is, with systematic thinking, I'm able to dis-embark much quicker. Once the domain solution is clear, I can quickly implement the desired code without having to keep to as much in my head. This leads to faster feature development, and fewer mistakes.
