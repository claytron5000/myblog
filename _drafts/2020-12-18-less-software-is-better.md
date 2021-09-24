---
id: 39
title: Less Software is Better Software
date: 2020-09-18-00:00:00
author: Clayton Salem
layout: post
---

There's a tendency in software development to correct problems by writing software. It's in the name after all, and it's human nature to take action - to _do_, instead of _not do_. The nature of software is such that adding another line of code, or even thousand lines of code is a natural part of a developer's day.

The best programmers will take into consideration a lot of things before or during the discovery of a solution to the problem. Some of the micro-decisions a developer with make in the course of writing software:

- how will other developers interact with this code in the future?
- will this scale if the problem area grows in different dimensions?
- what are the performance effects of one solution over the other?
- will this be supported in the long term?
- what are consequences for the user?

There are many other questions a developer will answer along the way. It's part of the joy and challenge of writing software. However, there's one question I want every developer to ask before starting on a problem:

- do we need this?

Because the most brillant, thoughtful, knowledgable developer will never be able to forsee all the consequences of their choices, therefore there will always be decisions that have adverse effects. If we can short-circuit all that decision making and code and solve the problem without software, or convince the stake-holders that what they've presented is no really a problem worth solving, we can have better more robust, more performant software.

My case study is a tiny edge of the gitlab software that has gotten under my skin and I can't shake.

On the merge request page, under Overview, the first box below the description shows which branch is being merged into which other branch.

Below that there's a pipeline box that also shows the branch name.

As you can see, in both places the mergin branch in truncated. When reviewing code, as the Merge Request page is want to do, a reviewer often will need the branch name. In fact, I'd say that's the primary piece of information a reviewer needs to complete a code review (your system may differ). And the designers/developers at gitlab have gone ahead and hidden a key piece of data from the user.

Obviously, someone at gitlab saw the issue, and they took time to come up with a solution to the problem: they added more software. In the first box there's a little icon of a clipboard and box. When you hover over it it informs you you can copy the branch name. This is good functionality, and it can be seen as an agumentation, after all, it allows the user to one-click copy the branch. But I suspect it's partly a result of the truncated branch name. However, it's only in the first box, not the second. It's also not eminently obvious to a user what that is.

Here what that box looks like with a very long branch name, and two lines of CSS removed.
