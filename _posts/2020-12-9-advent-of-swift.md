## Swift advent and regex

id: 38
title: Advent of Swift
date: 2020-12-09-00:00:00
author: Clayton Salem
layout: post

---

Like so many of my potential readers I’m participating in this year’s [advent of code](https://adventofcode.com/). This presents me with an opportunity to learn Swift, Apple’s app-oriented language and successor to Objective-C. There are always challenges learning a new language, and I expected those, what surprised me was how far Swift is from my daily drivers: JavaScript and PHP, and my go to “toy” language, Python. (Toy, as in, I almost never finish a Python project, not that the language is a toy).

The core of Swift isn’t all that unfamiliar. It falls not too far from the “C family” tree. However, it’s statically typed, and highly abstract, both of which are surprising trip-wires for someone who spends all his time in dynamic languages.

Static typing carries its own set of challenges, but ones that I was more or less prepared for. It’s the abstract nature of Swift and its Core library, Foundation, that are the big surprise. The [Swift Foundation](https://swift.org/standard-library/#standard-library-preview-package) lists Foundation under SDK, though in the [repository](https://github.com/apple/swift/tree/main/stdlib/public/Darwin/Foundation) it is found in the `sdlib` directory. However it’s classified, Foundation is where you find both String (Swift’s string primitive) and NSString (Objective-C’s string primitive).

Here’s the kicker: Swift’s String object has a very limited set of methods on it. Check out the documentation here: https://developer.apple.com/documentation/swift/string. The methods are at the bottom of the page. For a company that prides itself on design, I’m flabbergasted at the poor design of the documents. It’s really hard to get sense of the String object from the docs. Unlike, say MDN, or php.net where the string methods all live on a sidebar, and can be quickly read. Apple’s documentation hides all the methods at the bottom of the page, and never actually lists them all together. (This is a strong case for a mirror site that lays out the methods in a more user-friendly way.)

Finding the methods was step one, but then I realized, it's missing all the things!

While there’s approximately 40 `init` methods, there’s no generic `find`, or `indexOf` methods. There’s no `slice` or `splice` or `explode`. Regex isn’t even mentioned on the page. Compare that to the available php string methods: https://www.php.net/manual/en/function.addcslashes.php (notice the very handy sidebar of methods… I’m going to keep banging that drum, sorry). Or checkout the methods listed for JavaScript on MDN: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String. So useful, so practical.

This is where Swift’s highly abstract nature hit hard. There are few practical tools to allow you to interact with a very standard data type, String. I’m projecting here, but what I understand from this is not that the creators of Swift don’t want you interacting with Strings, but that they want you to define your own methods for interacting with Strings. This took me about a week to understand.

If you’re familiar with the advent of code, you’ll know there’s a lot of string parsing. The tricky parts are in the algorithm to solve the problems (I assume, I’m stuck around day 5.) But the tricky part for this new Swift coder is just the basics.

So this is part of learning Swift: writing your own extensions for doing the things that need to be done.

I'm not getting into Protocols and extensions here, only to briefly say, you write `extension`s into order to fill in implementation of `protocol`s. I.e. it's a place to write some practical code.

Following is my cobbled together regex on strings. I’ve done some extensive stack-overflow copy-and pasting in order to Frankenstein the following which adds a few methods: `index`, `endIndex`, `indices`(unused in this code), `ranges`, and finally `regex` to our `StringProtocol`. This allows me to do the following: `let match = "abcdefg".regex(of: "dg")`. I have _not_ tested it extensively, and I have not doubt there are a number of edge or not-so-edge cases where it will fail, but it's a start!

```swift
import Foundation

extension StringProtocol {
    func index<S: StringProtocol>(of string: S, options: String.CompareOptions = []) -> Index?
    {
        range(of: string, options: options)?.lowerBound
    }
    func endIndex<S: StringProtocol>(of string: S, options: String.CompareOptions = []) -> Index?
    {
        range(of: string, options: options)?.upperBound
    }
    func indices<S: StringProtocol>(of string: S, options: String.CompareOptions = []) -> [Index]
    {
        ranges(of: string, options: options).map(\.lowerBound)
    }
    func ranges<S: StringProtocol>(of string: S, options: String.CompareOptions = []) -> [Range<Index>]
    {
        var result: [Range<Index>] = []
        var startIndex = self.startIndex
        while startIndex < endIndex,
            let range = self[startIndex...]
                .range(of: string, options: options) {
                result.append(range)
                startIndex = range.lowerBound < range.upperBound ? range.upperBound :
                    index(range.lowerBound, offsetBy: 1, limitedBy: endIndex) ?? endIndex
        }
        return result
    }
    func regex(of query:String) -> Bool   
    {       
        let escapedQuery = NSRegularExpression.escapedPattern(for: query)    
        let ranges = self.ranges(of: escapedQuery, options: .regularExpression)       
        return ranges.count > 0
    }
}

```
