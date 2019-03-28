---
title: Finding the best job
date: 2019-03-22T20:34:07+00:00
author: Clayton Salem
layout: post
permalink: /best-job-ever/
---
I like my job, but it's not the *best* job. And since I deserver the *best* job it's time I found it, lucky for me [Outside Magazine](https://www.outsideonline.com/2257551/100-best-places-work-2017) already did the work and found the best 100 places to work!

But Clayton, you say, that's a two year old post, and how come you trust some magazine to decide what is best for you? Well, I don't, and it is, but I'm taking the day off and web scrapping relaxes me.

### 💯 is still a lot
They've narrowed it down to 100 best places to work, but that's still a lot to look at and read. Plus, they've sorted it from best to 100th best, but their criteria is opaque, and probably doesn't align with mine. However, they have lightly structured the page content, so maybe I can pull out relavent information with a little javascript DOM scraping, yay!

The first step is to look at the mark-up and determine the rhyme and reason here. Open the Inspector!

Like any generated site there's a lot of markup and `div`ities, but it's pretty clear the relevant parts are structured as follows:

```
    <div class="article__body article__body--linear post-entry spacing ">
        <!-- some more divs...-->
        <h2>1. Forum Phi Architecture</h2>
        <p></p>
        <figure class="rel has-blur has-fade-in post-block ratio-horizontal post-block--inline post-block--medium post-block--with-caption fade-in-fast">
        <!-- and a whole bunch of stuff I dont want... -->
        </figure>
        <p></p>
        <p>
            <strong>Location:</strong> Aspen, Colorado
            <br>
            <strong>What they do:</strong> <a href="https://forumphi.com/" target="_blank">Architecture, interior design, and land planning services</a>
            <br>
            <strong>Number of employees:</strong> 21
            <br>
            <strong>Average salary*:</strong> $65,819
            <br>
            <strong>Vacation time:</strong> Unlimited vacation days after one year.
            <br>
            <strong>Perks:</strong> Forum Phridays team-building events: Take a Friday off to hit the slopes, go biking, or do a hut-to-hut trip. Also: Employees who recommend someone for recruitment get a $500 bonus after that new hire’s 90-day review.
            <br>
            <strong>What they say:</strong> “We have a Forum Phitness Club where staff get together and workout during lunch. It can take many forms—hiking, biking, climbing, gym, skiing. Our facilities are all around us in Aspen.”
        </p>
        <!-- and it continues in a similar pattern -->
    </div>
```
This is pretty typical structure down the page. Once I ignore the `figure`s, empty and extraneous `p` elements I have an `h2` followed by a `p` that contains the data I want to get at. The data within that `p` isn't structured, but it fortunately follow a pretty clear pattern of `strong` tag, followed by a text node, then separated by `br` tags.

The data I'm interested in is: Name, Location, link to the company's website, number of employees, average salary. Perks, vacation time, other text is mostly fluff, or it's things that I can get a better sense of from the company's own website. 

Now that I can see the content in a mock-structured manner, what do I actually want to extract/sort on? And maybe larger, do i want to actually do something transient like sort by salary in the console, or actually save/store the data somewhere. Let's start with the goal that I'll get an object with the company title as key, and the four pertinent data points as attributes.

#### Step 1: gather the title and paragraph

```
document.querySelectorAll('.article__body h2')
```

This results in `NodeList(105)`, crap. The nice thing here is I *know* the total, and it's a nice round `100`, which means there are 5 sneaky `h2` in my list, and no classes to sort them out on.

```
Array.from(document.querySelectorAll('.article__body h2')).map(h2 => h2.innerText)
```

Great, now I can see there are a bunch of "How I work" blocks on the page. These are `h2` elements followed by `h3`s. However, the titles I do want have something in common, numbers start each title "79. Taptica!"

```
Array.from(document.querySelectorAll('.article__body h2')).map(h2 => h2.innerText).filter(h2Text => parseInt(h2Text.substring(0,1)))
```
`100`, Boom! Good thing `parseInt` returns a `null` which resolves to false in the filter.

Of course now we need to go back up to the `NodeList` so we can grab the sibling `p` tags. We could use the text strings to re-search the DOM, but I think a little re-arranging will get us what we need.

```
Array.from(document.querySelectorAll('.article__body h2')).filter(h2 => parseInt(h2.innerText.substring(0,1)))
```
Some of you might've gotten there first, but I'm a procedural kinda guy...

```
Array.from(document.querySelectorAll('.article__body h2')).filter(h2 => parseInt(h2.innerText.substring(0,1))).map(h2 => { return {name: h2.innerText, dataParagraph: h2.nextSibling}})
```
This is a little leap ahead, but I'm learning here. We now have something that looks like it's taking shape, an object with a company name, and a paragraph that contains my data. However, I know there are a bunch of empty paragraphs and figures following some titles. Basically, we want to do a `nextSibling` on each subsequent element until we get a paragraph with content. Do you think we should start making functions? [very gravelly voice] Where we're going, we don't **need** functions.

```
Array.from(document.querySelectorAll('.article__body h2')).filter(h2 => parseInt(h2.innerText.substring(0,1))).map(h2 => { return {name: h2.innerText, dataParagraph: h2.nextSibling.innerText ? h2.nextSibling : h2.nextSibling.nextSibling.nextSibling.nextSibling}})
```
Okay, I was a little cocky with this one. Without a function to handle some recursive logic, I've had to cheat this one in. The page structure is pretty consistent, so doing the quadruple `nextSibling` seems to work. Otherwise I'd have to a sort of maxed-out ternary `h2.nextSibling.innerText ? h2.nextSibling : h2.nextSibling.nextSibling.innerText ? h2.nextSibling.nextSibling.innerText : h2.nextSibling.nextSibling.nextSibling.innerText ? etc`. And without a recursion I basically do the same thing I'm doing above (namely, counting the elements until an paragraph with text), just in a harder to read way. If it turns out I'm wrong about the page structure I might have to come back to this.

Next I need to parse the paragraph into structured content. I know I bragged about doing it without functions, and I regret that already, I'm sorry, I take it back. Especially since I need to apply the structuring in two places, it really should be a function.
```
const paraParse = (paragraph) => {
    return {
        salaryInt: parseInt(Array.from(paragraph.querySelectorAll('strong')).filter(strong => strong.innerText.includes('salary'))[0].nextSibling.wholeText.replace(/,|\$/g, ''))
    }
}

```
Okay, did I say something about learning? I got to the point where I had the text from the `nextSibling` containing the salary, but string methods weren't working on it. When I did a `typeof`, it told me I was dealing with an object... huh? Like how String is an object in python?

After a few minutes I realized I had a `textNode` from the `nextSibling` attribute. I didn't find much documentation on `textNode`, but a quick stackoverflow lead me to the wholeText attribute.

Let's continue, we're getting close:

```
const paraParse = (paragraph) => {
    return {
        salaryInt: parseInt(Array.from(paragraph.querySelectorAll('strong')).filter(strong => strong.innerText.includes('salary'))[0].nextSibling.wholeText.replace(/,|\$/g, '')),
        location: Array.from(paragraph.querySelectorAll('strong')).filter(strong => strong.innerText.includes('Location'))[0].nextSibling.wholeText,
        numEmployees: Array.from(paragraph.querySelectorAll('strong')).filter(strong => strong.innerText.includes('employees'))[0].nextSibling.wholeText,
        link: Array.from(paragraph.querySelectorAll('strong')).filter(strong => strong.innerText.includes('What they'))[0].nextSibling.href
    }
}
```
Dang, that might work. To test this function I used the element inspector's "Copy JS Path" method. This allows me to assign any element on the page to a variable, then run that variable through my function. Now I apply it to my selector thing.

```
Array.from(document.querySelectorAll('.article__body h2')).filter(h2 => parseInt(h2.innerText.substring(0,1))).map(h2 => { return {name: h2.innerText, dataParagraph: h2.nextSibling.innerText ? paraParse(h2.nextSibling) : paraParse(h2.nextSibling.nextSibling.nextSibling.nextSibling)}})
```

Oh dangity dang. `Uncaught TypeError: Cannot read property 'nextSibling' of undefined`. And since it's all part of the same command and I'm using `nextSibling` everywhere it's hard to debug. I'm guessing it's in the attribute assignments.

```
const paraParse3 = (paragraph) => {
    return {
        salaryInt: parseInt(Array.from(paragraph.querySelectorAll('strong')).filter(strong => strong.innerText.includes('salary'))[0] ? strong.innerText.includes('salary')[0].nextSibling.wholeText.replace(/,|\$/g, ''): ''),
        location: Array.from(paragraph.querySelectorAll('strong')).filter(strong => strong.innerText.includes('Location'))[0].nextSibling ? strong.innerText.includes('Location')[0].nextSibling.wholeText : '',
        numEmployees: Array.from(paragraph.querySelectorAll('strong')).filter(strong => strong.innerText.includes('employees'))[0].nextSibling ? strong.innerText.includes('employees')[0].nextSibling.wholeText : '',
        link: Array.from(paragraph.querySelectorAll('strong')).filter(strong => strong.innerText.includes('What they'))[0].nextSibling ? strong.innerText.includes('What they')[0].nextSibling.href : ''
    }
}
```
Egads!

```
Array.from(document.querySelectorAll('.article__body h2')).filter(h2 => parseInt(h2.innerText.substring(0,1))).map(h2 => { return {name: h2.innerText, dataParagraph: h2.nextSibling.innerText ? paraParse3(h2.nextSibling) : paraParse3(h2.nextSibling.nextSibling.nextSibling.nextSibling)}})
```
`Uncaught ReferenceError: strong is not defined` Do it all in the console, he said, it'll be fun, he said, one simple function definition, he said. Damn you past self!

Unfortunately, my meter is running out, and so is my patience. I've successfully avoided doing things I *should* be doing for a couple hours, and I've learned a few things. The main thing, is maybe of course, I need to do the scraping in a way that I can actually debug the issues that appear. I can wing it up to a point, but after a certain amount of complexity it gets very hard to move forward. 

If I come back to this on another rainy day I'll update this post.