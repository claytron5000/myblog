---
id: 97
title: Slide 'n Scroll fun
date: 2017-03-08T19:06:00+00:00
author: Clayton Salem
layout: post
guid: http://claytonsalem.com/claytonsalem/?p=97
permalink: /slide-n-scroll-fun/
categories:
  - Technology
---
I was tasked with creating a scroll animation, something that I&#8217;ve done before, but in a very ad-hoc fashion. I think the first time I did a &#8220;parallax&#8221; style scroll I did something like this:

<pre>window.on("scroll", function(){
  var scrollDistance = window.scrollY;
  var moveDistance = scrollDistance * 1.25;
  element.css("top", moveDistance);
);</pre>

That worked fine on an overpowered PC and when it was moving a tiny image up and off the page, but the next task was to move an image across the page in response to scrolling, and I wanted to make sure it was performant on all devices.

The first step was to avoid using \`top\` or \`left\` to define the image position. Paul Irish and Paul Lewis have a canonical[ post](https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/) on this. The quick take away is the least &#8220;expensive&#8221; properties to animate are transform, scale, rotate and opacity. For this example we&#8217;ll use opacity.

The entire premise of this post is that I&#8217;m making performant animations. There is a legitimate question whether this level of detail was premature optimization. However, I had some time and wanted to make the most butter-smooth animations I could.

The second issue was to use the broadly supported [requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)(rAF). This allows us to make an animation change(frame), only when the computer will actually change pixels on the screen. Otherwise you could end up with repaints occurring whenever and a resulting jumpy animation.

Finally, I incorporated Paul Lewis&#8217;s debounce on the rAF [post](https://www.html5rocks.com/en/tutorials/speed/animations/). Basically this is using the rAF to separate the scroll event from the animation update, and prevent the browser from stacking up rAF calls.

<pre>function update(timestamp) {
  // This rAF callback gets the High Resolution Timestamp of the sub-millisecond time of painting as an argument. I ended up using this to check if we'd stopped scrolling yet.
}
function requestTick() {
 if (!ticking) { // This if block prevents the stack up of excess animation calls.
 requestAnimationFrame(update);
 ticking = true;
 }
}
function onScroll() {
 lastScrollY = window.scrollY; // We get the distance scrolled at the scroll event instead of inside the animation.
 requestTick();
}
window.addEventListener('scroll', onScroll, false); // Listener
</pre>

Above is the basic skeleton that I took from Paul Lewis. One thing he does that I didn&#8217;t get around to is he separates reflows from adding classes. If the animation shows signs of jitters or performance slow-downs in the real-world this is something I might re-address.

## Scroll End

Part of the animation requested was to add a &#8220;jiggle&#8221; or &#8220;wobble&#8221; to the image when scrolling stopped. It turns out there is no &#8220;scrollEnd&#8221; event, so we have to find it ourselves. My solution involved the High Resolution Timestamp.

First I added a timeCheck variable outside the update function. Then I set the timeCheck variable to equal the timestamp at the bottom of the update function.

<pre>timeCheck = timestamp;</pre>

This way on my second(or third or fourth..) time through update(), we can compare the difference between the time the animation starts, and the last time through. This should give us a pretty accurate idea if we&#8217;ve stopped scrolling or not

<pre>if (timestamp - timeCheck &lt; 17 && timestamp !== 0) {
  // Clear the last so we don't add classes.
  clearTimeout(timer[i]);
  // If the timer isn't cleared next frame, add the class.
  timer[i] = setTimeout(function () {
    image.addClass(wobbleClass);
  }, 17);
}
else {
  // remove wobble class.
}</pre>

Why 17? Well, this is optimized for screens that are painting at 60fps(frames per second). Since 1 / 60 = 16.7 that&#8217;s the lowest possible time between two frames. It would be interesting to see if a slower screen, say 30fps, would trigger the added class even while continuing to scroll.

## Some Math

Finally, I wanted to define the movement of the image based on both scrolling, width of the image, and height of the screen. For a while I struggled to figure out the math, and through sheer will I came up with an algorithm that worked. But then the designer looked at it and decided he wanted the movement to start and stop a little differently.

Stumped, I took a walk. I knew there was some easy way to solve for a two variable problem&#8230; Somewhere on the walk I realized that was I was doing was defining a line.<figure id="attachment_100" style="width: 225px" class="wp-caption aligncenter">

<a href="/images/2017/03/a_line.jpg" rel="attachment wp-att-100"><img class="wp-image-100 size-medium" src="/images/2017/03/a_line-225x300.jpg" alt="a_line" width="225" height="300" srcset="/images/2017/03/a_line-225x300.jpg 225w, /images/2017/03/a_line-768x1024.jpg 768w" sizes="(max-width: 225px) 100vw, 225px" /></a><figcaption class="wp-caption-text">This took many hours</figcaption></figure>

Once I&#8217;d figure that out, I could dig deep in my memories of Highschool and drudge up the old

<pre>y = mx + b</pre>

from Algebra. For those of you rusty m is the slope of the line (rise over run) and b is where the line intersects the y axis. From there I just had to define where the image was at page bottom, and where it was at page top, then draw a line between two corners and figure out the slope. (If you look closely you can see my line in the illustration is significantly off, ie. it doesn&#8217;t intersect the correct parts of the image rectangle.)

<pre>function translateLTR(windowY, imageOffset, windowHeight, image, windowWidth) {
  // y is scroll distance or vertical displacement.
  var y = windowHeight - (imageOffset - windowY);
  // m is the slope of the line along which the image moves.
  var m = (windowWidth + image.width()) / (windowHeight + image.height());
  var x = y * m;
  // Move the y axis to the left side of image when at bottom of page.
  return x - image.width();
}</pre>

This looks a little ugly because there&#8217;s some maths to get the position of the bottom corner of the image. But it&#8217;s essentially just filling in the values for m and b and solving for x.

Here&#8217;s some math figuring out the Right to Left version. And trust me there were pages and pages more of this crap, this is the cleaned up version.<figure id="attachment_101" style="width: 474px" class="wp-caption aligncenter">

<a href="/images/2017/03/maths.jpg" rel="attachment wp-att-101"><img class="wp-image-101 size-large" src="/images/2017/03/maths-1024x768.jpg" alt="That pencil is so naturally placed" width="474" height="356" srcset="/images/2017/03/maths-300x225.jpg 300w, /images/2017/03/maths-768x576.jpg 768w, /images/2017/03/maths-1024x768.jpg 1024w" sizes="(max-width: 474px) 100vw, 474px" /></a><figcaption class="wp-caption-text">That pencil is so naturally placed</figcaption></figure>

Here&#8217;s the full code. I&#8217;m not sure how much use it is since there are specific problems I&#8217;ve solved here.

<pre>var images = $('.scroll-mover', context);
var lastScrollY = window.scrollY;
var ticking = false;
var timer = [];
var timeCheck = 0;
var startScrollY = 0;

var windowWidth = $(window).width();
// Call update on load so images translate to pre-scrolled position.
update(0);

function update(timestamp) {
  var windowHeight = $(window).height();

  if (windowWidth &lt;= 768) {
    images.each(function (i, el) {

      var image = $(this);
      var imageOffset = image.offset().top;

      var direction = image[0].dataset.scrollDirection;
      var translateDistance;
      if (direction === 'ltr') {
        translateDistance = translateLTR(lastScrollY, imageOffset, windowHeight, image, windowWidth);
      }
      else {
        translateDistance = -(translateRTL(lastScrollY, imageOffset, windowHeight, image, windowWidth));
      }

      move(image, translateDistance, direction);
      var wobbleClass = getWobbleClass(lastScrollY, startScrollY);

      // Check if we're still scrolling in the next frame (16.7ms).
      // Don't add class on page load.
      if (timestamp - timeCheck &lt; 17 && timestamp !== 0) {
        // Clear the last so we don't add classes.
        clearTimeout(timer[i]);
        // If the timer isn't cleared next frame, add the class.
        timer[i] = setTimeout(function () {
          image.addClass(wobbleClass);
        }, 17);
      }
      else {
        startScrollY = lastScrollY;
        image.removeClass('wobble-left').removeClass('wobble-right');
      }

    });
  }
  timeCheck = timestamp;
  ticking = false;
}

function onScroll() {
  windowWidth = $(window).width();
  lastScrollY = window.scrollY;
  requestTick();
}

function requestTick() {
  if (!ticking) {
    requestAnimationFrame(update);
    ticking = true;
  }
}

function translateLTR(windowY, imageOffset, windowHeight, image, windowWidth) {
  // y is scroll distance or vertical displacement.
  var y = windowHeight - (imageOffset - windowY);
  // m is the slope of the line along which the image moves.
  var m = (windowWidth + image.width()) / (windowHeight + image.height());
  var x = y * m;
  // Move the y axis to the left side of image when at bottom of page.
  return x - image.width();
}

function translateRTL(windowY, imageOffset, windowHeight, image, windowWidth) {
  // y is scroll distance top edge is on screen or vertical displacement into screen.
  var y = windowHeight - (imageOffset - windowY);
  // m is the slope of the line along which the image moves.
  var m = (windowWidth + image.width()) / (windowHeight + image.height());
  var x = y * m;
  // Move the y axis to right side of window.
  return x - windowWidth;
}

function move(image, translateDistance) {
  image.css('transform', 'translate(' + translateDistance + 'px)');
}

function getWobbleClass(lastScrollY, startScrollY) {
  // We set up this value ahead of the timer so that there's no delay.
  if (startScrollY &gt; lastScrollY) {
    return 'wobble-left';
  }
  else {
    return 'wobble-right';
  }
}

window.addEventListener('scroll', onScroll, false);</pre>
