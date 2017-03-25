---
id: 50
title: Eclecticism
date: 2014-06-04T17:22:12+00:00
author: Clayton Salem
layout: post
guid: http://claytonsalem.com/claytonsalem/?p=50
permalink: /eclecticism/
categories:
  - Uncategorized
---
I have a number of subjects that I&#8217;ve been ruminating but haven&#8217;t sat down to explore further: custom WordPress themes, our home entertainment set-up, merging all contacts, learning ukulele with a 2 year old, etc. Instead I think I&#8217;ll just take a minute to look at how I spend a day.

It&#8217;s now 12:03 on a Wednesday. It has been a slow work week. I finished a project last Friday. I&#8217;m waiting on feedback on two other projects. And I&#8217;ve made contact on a couple leads for new projects. It&#8217;s what I call a &#8216;holding pattern&#8217; week. Actually, I just made that up, usually these work-less weeks are more like freak-out stress weeks. I am learning to lean into them and take the opportunity to expand my knowledge, practice skills and get chores done.

#### Illustrator

I started a class on Skillshare.com, [learning illustrator](http://www.skillshare.com/classes/design/Learn-the-Ins-and-Outs-of-Illustrator/589498286), by Brad Woodard. It&#8217;s a tool that I&#8217;ve used in the past, but I&#8217;m very out of date on the software. I&#8217;ve started recreating a poster for the Swiss lake Silvaplana.[<img class="size-medium wp-image-51 alignleft" alt="silvaplana" src="/images/2014/06/silvaplana-300x204.png" width="300" height="204" srcset="/images/2014/06/silvaplana-300x204.png 300w, /images/2014/06/silvaplana.png 941w" sizes="(max-width: 300px) 100vw, 300px" />](/images/2014/06/silvaplana.png)It&#8217;s super fun and challenging. It&#8217;s certainly a tool that I&#8217;ll use in the future.

#### Cookies

My second task of the day is doing a little free-work on a site I did for Deko-Design. It was a small 3 page HTML site that runs an animation on the index page. I wanted to get it so that a visitor only sees the animation one time. The obvious solution is to use cookies to check if a visitor has been to the page or not. I did a little research on how to use cookies, it doesn&#8217;t seem too complicated and it&#8217;s made even easier by using the jquery.cookie library. This allows me to just say $.cookies to write to the browser cookie. Easy enough.

However, I didn&#8217;t write the animation that opens the site, and since my javascript skills are limited at best I had trouble decoding what the code did. It&#8217;s small and pretty simple, but still Greek to me:

<pre>$(function() {
 $('div.transitional').click(function() {
 $('div.intro').removeClass('hidden');
 $('div.final').off('click');
 });</pre>

<pre>ShowDiv($("div.transitional.hidden")[0]);
});
function ShowDiv(target) {</pre>

<pre>target = $(target);
 target.removeClass('hidden');

 target.delay(500).animate({
 opacity: 1.0
 },300,'easeInExpo',function() {
 ShowDiv($("div.transitional.hidden")[0]);
 })
}</pre>

So I went over to [Stackexchange](http://stackoverflow.com/questions/24018950/how-does-this-animation-work?noredirect=1#comment37022067_24018950) to get advice from the many sages on that site.  I received a couple of good answers, though honestly I haven&#8217;t been able to get it to work yet. That may be this afternoon&#8217;s task.

Of course, the reason I&#8217;m doing this task off-clock is that it&#8217;s taking me hours, when I would only feel comfortable charging for an hour or less to write, debug. If I do it this one time for free it becomes another tool in the box that I can use. Again, future planning, fun right now.

##### Python

My final activity for today is [learnpythonthehardway.org](http://learnpythonthehardway.org/book/) by Zed Shaw. There are a number of ways that I&#8217;m learning to code. First is like I described above, taking on tasks I&#8217;m unsure of at a low or free rate. This is a great way to tackle practical tasks, though often it results in just copy and pasting found script. I&#8217;ve done some rudimentary learning through codeacademy.com, a great resource as a beginner. I&#8217;ve learned the basics of javascript and jQuery there(as well as HTML and CSS). Finally, I&#8217;ve enrolled and started two different MOOC&#8217;s at [coursera](http://coursera.com), both Intro to Python. I have never finished those courses, though I came close. Instead I seem to learn a little of the syntax but am missing the bigger picture, &#8220;how to code&#8221; goal.

Then, I found learnpythonthehardway and was totally intrigued. The title got me at first, then when I looked at his method it dawned on me that he was teaching towards my learning style. (Interestingly, he discounts learning styles in the introduccion.) He sets up the book like an exercise in patience and repetition. The tasks are easy at first and are very repetitive. He only slowly builds skills over the course of many chapters. Every other way that I&#8217;ve learned coding has me writing conditionals or for loop within a dozen chapters. I&#8217;m currently on chapter 21 and he only just brought in the concept of functions.

Will this make me a programmer? No, probably not, but I&#8217;m hoping through 52 chapter I will have a solid grasp of not just the Python syntax, but also those broader concepts I still struggle to differentiate: methods, functions, classes, etc.

Between these tasks, listening to podcasts and doing some household chores I can fill up a week pretty well and feel pretty good about what I&#8217;ve done. Of course the goal is always to do more paid work, but if I can settle down and lean into these off weeks I can learn a little something also.

&nbsp;