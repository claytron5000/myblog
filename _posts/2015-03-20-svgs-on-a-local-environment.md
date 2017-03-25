---
id: 67
title: 'SVG&#8217;s on a local environment'
date: 2015-03-20T12:48:57+00:00
author: Clayton Salem
layout: post
guid: http://claytonsalem.com/claytonsalem/?p=67
permalink: /svgs-on-a-local-environment/
categories:
  - Technology
---
This afternoon I was replacing a .png image with an .svg. Working in my local development environment, I started with a simple \`<img src=&#8221;myicon.svg&#8221;>\`. It looked like a broken link. Next I went to [CSS-Tricks](https://css-tricks.com/using-svg/) and used the first fall-back technique he lists:

\`<svg width=&#8221;96&#8243; height=&#8221;96&#8243;> <image xlink:href=&#8221;svg.svg&#8221; src=&#8221;svg.png&#8221; width=&#8221;96&#8243; height=&#8221;96&#8243; /> </svg>\`

This also resulted in a broken link.

I banged my head against the wall for at least an hour, checking the path, swapping .svg for .png. Everything I could think of. Then I looked at the actual svg file.

\`<?xml version=&#8221;1.0&#8243; encoding=&#8221;utf-8&#8243;?>

<!&#8211; Generator: Adobe Illustrator 17.1.0, SVG Export Plug-In . SVG Version: 6.00 Build 0) &#8211;>

<!DOCTYPE svg PUBLIC &#8220;-//W3C//DTD SVG 1.1//EN&#8221; &#8220;http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd&#8221;>
  
<svg version=&#8221;1.1&#8243; id=&#8221;Layer_1&#8243; xmlns=&#8221;http://www.w3.org/2000/svg&#8221; xmlns:xlink=&#8221;http://www.w3.org/1999/xlink&#8221; x=&#8221;0px&#8221; y=&#8221;0px&#8221;
  
viewBox=&#8221;0 0 200 200&#8243; enable-background=&#8221;new 0 0 200 200&#8243; xml:space=&#8221;preserve&#8221;>
  
<path fill=&#8221;#FFFFFF&#8221; d=&#8221;M0,0v200h200V0H0z M161.757,70.931c0.062,0.911,0.096,2.276,0.096,4.096c0,8.453-1.234,16.888-3.705,25.307 &#8230;\`

A few links and that xmlns:xlink reference and I started wondering, &#8220;is it my dev environment?&#8221; Sure enough, once I pushed the code to a server the image showed up.

Edit: After talking with a co-worker I realized I was wrong. No, there&#8217;s no magic linking .svg&#8217;s the web over DNS or something. It was a permission&#8217;s issue. When I pushed the file through Git the permissions were set to 755, or whatever it needed to be. But on my local machine, for whatever reason the reading permissions were locked down for apache. Light dawns over Marble Head.