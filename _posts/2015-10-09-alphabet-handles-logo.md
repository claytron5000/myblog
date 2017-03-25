---
id: 72
title: This weird trick to hide your site name
date: 2015-10-09T01:30:28+00:00
author: Clayton Salem
layout: post
guid: http://claytonsalem.com/claytonsalem/?p=72
permalink: /alphabet-handles-logo/
categories:
  - Technology
---
I took a look at Google&#8217;s parent company Alphabet&#8217;s  website today. It a clean little one-pager. The thing I noticed was their handling of the logo.

[<img class="alignnone size-full wp-image-73" src="/wp-content/uploads/2015/10/Screen-Shot-2015-10-08-at-9.20.00-PM.png" alt="Screen Shot 2015-10-08 at 9.20.00 PM" width="654" height="672" srcset="/wp-content/uploads/2015/10/Screen-Shot-2015-10-08-at-9.20.00-PM-292x300.png 292w, /wp-content/uploads/2015/10/Screen-Shot-2015-10-08-at-9.20.00-PM.png 654w" sizes="(max-width: 654px) 100vw, 654px" />](/wp-content/uploads/2015/10/Screen-Shot-2015-10-08-at-9.20.00-PM.png)

At first it looks like a simple logo font. Turns out their doing some old-school web shenanigans.

\`<span class="selector"><span class="simple-selector selector-matches">#logo</span></span> {

<ol class="style-properties monospace" tabindex="0">
  <li class="parent">
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="background-repeat:no-repeat;">background-repeat</span>: <span class="value">no-repeat</span>;
  </li>
  <li class="parent">
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="background-position:0 0;">background-position</span>: <span class="value">0 0</span>;
  </li>
  <li>
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="color:transparent;">color</span>: <span class="value">transparent</span>;
  </li>
  <li class="parent">
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="font:0 / 0 a;">font</span>: <span class="value">0 / 0 a</span>;
  </li>
  <li>
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="text-shadow:none;">text-shadow</span>: <span class="value">none</span>;
  </li>
  <li class="parent">
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="background:url(../img/logo_2x.png) 0 0 no-repeat;">background</span>: <span class="value">url(<a class="webkit-html-resource-link" title="https://abc.xyz/img/logo_2x.png" href="https://abc.xyz/img/logo_2x.png">../img/logo_2x.png</a>) 0 0 no-repeat</span>;
  </li>
  <li>
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="background-size:100px 24px;">background-size</span>: <span class="value">100px 24px</span>;
  </li>
  <li>
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="height:24px;">height</span>: <span class="value">24px</span>;
  </li>
  <li>
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="left:24px;">left</span>: <span class="value">24px</span>;
  </li>
  <li>
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="position:absolute;">position</span>: <span class="value">absolute</span>;
  </li>
  <li>
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="top:18px;">top</span>: <span class="value">18px</span>;
  </li>
  <li>
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="width:100px;">width</span>: <span class="value">100px</span>;
  </li>
  <li>
    <input class="enabled-button" type="checkbox" /> <span class="webkit-css-property" title="z-index:999">z-index</span>: <span class="value">999</span>;
  </li>
</ol>

<div class="styles-panel-hovered">
  }`
</div>

<div class="styles-panel-hovered">
  <p>
    Notice the  `color: transparent;` and `font: 0 / 0 a;`? In the past we usually saw `position: absolute; left: 9999px;` or something to through it off the page. This is basically the same thing, they&#8217;re just shrinking the font to nothing, and making it transparent. Actually, the font declaration caught my eye. Turns out it&#8217;s short-hand for `
  </p>

  <ol class="children expanded">
    <li>
      <span class="webkit-css-property" title="font-size: 0px;">font-size</span>: <span class="value">0px</span>;
    </li>
    <li>
      <span class="webkit-css-property" title="line-height: 0;">line-height</span>: <span class="value"></span>;
    </li>
    <li class="styles-panel-hovered">
      <span class="webkit-css-property" title="font-family: a;">font-family</span>: <span class="value">a</span>;`
    </li>
  </ol>

  <p>
    For the most part it&#8217;s very neat, simple and clean. Even this feels like a neater version of that old hack. However, it&#8217;s standard now to not hide text in anyway, but instead to render a picture element with a title and alt tag.
  </p>
</div>
