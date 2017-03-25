---
id: 77
title: Preventing and allowing linebreaks
date: 2016-09-10T13:14:28+00:00
author: Clayton Salem
layout: post
guid: http://claytonsalem.com/claytonsalem/?p=77
permalink: /preventing-and-allowing-linebreaks/
categories:
  - Uncategorized
---
This is a trick I figured out when a client wanted to have chevron icon to the right of a block title. However, they didn&#8217;t want the chevron to break to it&#8217;s own line, unless the title itself broke to the new line. Basically, the chevron need to act like it was part of the last word, and maintain space between itself and that word. Also, the chervon moves to the right on hover.

<pre>Here's my solution:
&lt;h4 class="title-wrap"&gt;
&lt;span class="title"&gt;This title can be any length&lt;/span&gt;&nbsp;&lt;i class="icon-chevron-right"&gt;&lt;/i&gt;
&lt;h4&gt;
 
.title-wrap {
 white-space: nowrap;
 .title {
   display: inline;
   white-space: normal;
 }
 i {
   display: inline;
 }
}
</pre>

The parent element, the .title-wrap in this case, is assigned nowrap, which then in combination with the nbsp; prevents the icon from breaking onto it&#8217;s own line. Then, by resetting the white-space on the span we let the .title itself wrap to multiple lines. So the text of the title can be any length, and will wrap when it needs to, but the icon will &#8220;stick&#8221; to the end of the line and never break on it&#8217;s own. If the title is a single word extending the length of the block it will continue past the boundaries of the block including the icon.

You&#8217;ll notice that the span and i markup is inline. This is important because certain newlines, those invisible \r\n or \n or \r will actually break the nowrap attribute. In other words, the following will break the &#8220;stickiness&#8221; of the i icon:

<pre>&lt;span class="title"&gt;This title can be any length&lt;/span&gt;
&nbsp;
&lt;i class="icon-chevron-right"&gt;&lt;/i&gt;
</pre>
