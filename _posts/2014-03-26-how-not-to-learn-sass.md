---
id: 35
title: how not to learn SASS
date: 2014-03-26T20:34:07+00:00
author: Clayton Salem
layout: post
guid: http://claytonsalem.com/claytonsalem/?p=35
permalink: /how-not-to-learn-sass/
categories:
  - Uncategorized
---
<p style="text-align: left">
  I was looking for a bare-bones wordpress theme on which to build a custom site. The &#8220;Bones&#8221; theme from http://themble.com/bones/ looked great(probably because of the skelton pirate) and it was built using the SASS(syntactically awesome stylesheet) preprocessor. I&#8217;ve been meaning to sit down and learn SASS for a while, so I said to myself, &#8220;this is a good opportunity to get an introduction.&#8221;
</p>

<p style="text-align: left">
  Wrong.
</p>

<p style="text-align: left">
  Most evangelists of the preprocessor say that it&#8217;s easy to start using, that you can start writing without replacing your entire stylesheet. Here&#8217;s what the guys at themble say &#8220;Sass comes baked into Bones. You&#8217;ll be off and running in no time. Never worked with Sass? Don&#8217;t stress, it&#8217;s not as hard as you think.&#8221;
</p>

<p style="text-align: left">
  The first thing I wanted to do was place a background color in the header. Pretty simple, so I thought. Instead, I opened the scss folder and found ~25 style sheets and something called the config.rb. My text editor, Sublime-Text doesn&#8217;t have SASS context coloration like it does for other programming languages. So every page was just solid white on black.After digging through I found where the header was called and where the colors were defined. I tried to change the color values, no effect, I tried to change the color variables, still no effect. <a href="/images/2014/03/Screen-Shot-2014-03-26-at-4.28.37-PM.png"><img class="alignnone size-medium wp-image-36" alt="howza?" src="/images/2014/03/Screen-Shot-2014-03-26-at-4.28.37-PM-300x187.png" width="300" height="187" srcset="/images/2014/03/Screen-Shot-2014-03-26-at-4.28.37-PM-300x187.png 300w, /images/2014/03/Screen-Shot-2014-03-26-at-4.28.37-PM-1024x640.png 1024w" sizes="(max-width: 300px) 100vw, 300px" /></a>
</p>

<p style="text-align: left">
  The caveat is that I&#8217;m used to charging in &#8220;commando style&#8221; and making changes on the fly. Obviously (only after messing with this for an hour), SASS requires a preprocessing engine to actually make the files into a stylesheet, something that&#8217;s not included on my server.
</p>

<p style="text-align: left">
  Lesson learned. When I actually start using SASS I will post thoughts about it.
</p>