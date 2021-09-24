---
id: 39
title: Switching to Linux
date: 2021-09-01-00:00:00
author: Clayton Salem
layout: post
---

## A tale of failure

I've seen a number of blog posts where developers complain about the Apple ecosystem and show how they made the switch from MacOS to Linux. Usually, they have a complaint about the company, their anti-competitive behavior, or their intrusiveness, or their non-web focus, or their relentless un-openness. I usually find myself nodding along, and I'm usually inspired by their post to try out some combination of alternative services or hand-rolled versions of the key Apple applications. "I can do it!" I say to myself, "I can break the bond." I've tried moving to linux at least twice now, and each time I fail. Is it Messages app that keeps me tied to MacOS? Or Handoff? Is it keystrokes I just can't unlearn? Is it the third party apps that just don't have equals on other systems? It's all of these things, but maybe at core, it's an application philosophy, a way of looking at human-computer interaction that's just different between the two ecosystems. I'm going to explore these differences, not with some analysis of the unix application, or of open source versus closed source, but instead, screen-shot annotation.

Incidentally, I know how to break away from MacOS operating system: integrate some tool or part of another operating system into your work in a way that makes it vital. For example, if I become a `proc` file master, I would have to use Linux primarily. But this strategy of "use X instead of Y" is never going to work for me.

### Skitch

[Skitch](https://evernote.com/products/skitch) is an image annotation app from Evernote. It's essential look is a big magenta arrow drawn over a screenshot. It has a few other tools besides the arrow: text (which by default is also magenta, and is a bubbly thick font with a white outline so it stands out from any background), boxes, free draw, and little sticky-note like tabs. You can change the color you're drawing with to a series of bright 6 bright colors, black & white, and a color picker. You can also change the size of the thing you're drawing (arrow thickness, font size). There's some integration with Evernote, there's a screen-capture tool, highlighting, some other things, but the main drawing tools are the ones I use.

My workflow with the app is as follows:

1. Ctrl+Cmd+Shift+4 (MacOS screen-capture to pastable memory)
2. Move to Skitch and Cmd+v (often I have to discard the current Skitch item, I never save anything in Skitch, so this is a little work-flow interuption)
3. Make annotations
4. Ctrl+Cmd+Shift+4 again, only this time I'm just capturing the Skitch window
5. Cmd+v where ever I'm sending the annotated image

It's very effemeral, and I'm fighting the app a bit by not saving anything, but it's a workflow I'm extremely comfortable with, and one with which I can provide strong visual feed-back in seconds.

### The alternative

Annotations were not the first thing I did when I switched to Linux, but it was one of the first things I realized I was missing. There are a few solutions out there, but the one that's the closest is [Flameshot](https://flameshot.org/). I have to admit, Flameshot might be a better tool in a lot of ways. It's interactive, not just with whatever image you've pasted in, but the actual screen. And there are more built-in tools than Skitch. It's even configurable, so if those actions in the action list are not to your liking, you can remove them. (Also, it's open source and usable in Linux, Mac and Windows.)

However, the results don't look as good. The difference are tiny, but the effects are huge. In Skitch, you're confined to only 5 sizes for anything, but they're sizes that work. In Flameshot you have a slider, but it's to small so it's hard to get the right size. I was only able to get three visibly different sizes: too small, Okay, too big. In Skitch you're limited to one font, but it's a friendly font without being silly. In Flameshot, you've got every font on the system, so good luck finding one that speaks in the right voice. In Skitch you have 6 default colors, but they're all clean and bright. In Flameshot you have a color-picker, again tyranny of choice. In Skitch there's dropshadows and white outline on everything. In Flameshot it's just flat annotations.

But, I hear you shouting, you're comparing two different apps, not operating systems! You're correct, but I'm using these apps to describe a difference in philosophy between MacOS ecosystem and Linux.

Skitch is an app with a limited scope, and a lot of guardrails. Clearly the developers and designers focused on the look and feel, and the results the application could produce. They missed configurability, and most glaringly, they missed a part of the essence of this application: annotating screen shots. Instead, it's really a picture annotation app. But those design choices, the guardrails built into the app make it fast and easy to get a good-looking screenshot in very little time. This is the essential value-proposition of an app like this. I _could_ open the screenshit in PhotoShop, or GIMP, but the point is to move fast and have good results. Skitch does that.

Flameshot is an app that know exactly what it is, a screen-shot annotation app. I really appreciate that. The problem is the output is not as good, and it's not as easy to use. In writing this post I figured out a bunch of things I could do with Flameshot that months of using it I'd missed. The learning curve is steep for something that should be kind of brainless. Once it's configured just right, it might be better, but in the world of work, I need to move fast, not spend hours tinkering with settings.

I think those are pretty good metaphors for the MacOS philosophy and the Linux philosophy. Design and outcome versus functionality and configuration.

If you'll excuse me, I'm going to go make an issue request to add drop-shadows to Flameshot now...
