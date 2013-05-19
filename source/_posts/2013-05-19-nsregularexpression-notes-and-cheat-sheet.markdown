---
layout: post
title: "NSRegularExpression Notes and Cheat Sheet"
date: 2013-05-19 21:30
comments: true
categories: [DEV, iOS]
---

I'm so shamed that little knowledge about RegularExpression was armed. Then I found a [blog](http://www.raywenderlich.com/30288/nsregularexpression-tutorial-and-cheat-sheet) post by [Ray Wenderlich](http://www.raywenderlich.com/), an independent iOS developer whoes articals did have helped me a lot.

A regular expression (commonly known as a “regex”) is a string or a sequence of characters that specifies a pattern. Think of it as a search string — but with super powers!

A plain old search in a text editor or word processor will allow you to find simple matches. A regular expression can also perform these simple searches, but it takes things a step further and lets you search for patterns , such as two digits followed by letter, or three letters followed by a hyphen.

This pattern matching allows you to do useful things like validate fields (phone numbers, email addresses), check user input, perform advanced text manipulation and much much more.

In addition, I’ll give you a handy NSRegularExpression Cheat Sheet PDF that you can print out and use as reference as you’re developing!

Without further ado, it’s time to start crunching some regular expressions.

### The Basics Introduction

_Examples_

Let’s start with a few brief examples to show you what regular expressions look like.

Here’s an example of a regular expression that matches the phrase “NSRegularExpression”:

`NSRegularExpression`

That’s about as simple as regular expressions get. You can use some APIs that are available in iOS to search a string of text for any part that matches this regular expression – and once you find a match, you can find where it is, or replace the text, etc.

Here’s a slightly more complicated example – this one matches the phrase “NSRegularExpression” or “NSRegularExpressions”:

`NSRegularExpression(s)?`

This is an example of using some special characters that are available in regular expressions. The parenthesis create a group, and the question mark says “match the previous element (the group in this case) 0 or 1 times”.

Now let’s go for a really complex example. This one matches any HTML or XML tag:

`<([a-z][a-z0-9]*)\b[^>]*>(.*?)`

Wow, looks complicated, eh? :) Don’t worry, you’ll be learning about all the special characters in this regular expression in the rest of this tutorial, and by the time you’re done you should be able to understand how this works! :)

_Testing Regular Expressions_

In this tutorial, you’ll be creating a lot of regular expressions. If you want to try them out visually as you’re working with them, check out [regexpal](http://regexpal.com/) , a web-based regular expression parser. Enter a regular expression in the top field, enter some text in the bottom field, and the matches in the searched text will automatically highlight.

Load up regexpal and try out the above example expressions one at a time. Here’s some good sample text to use:

`NSRegularExpression tutorial or NSRegularExpressions tutorial. And here's an html tag.`

Pretty handy, eh? It’s great to see regular expressions in action, so you can test out your own regular expressions as you’re working with them.

### Overall Concepts

Before you go any further, it’s important to understand a few core concepts about regular expressions.

_Literal characters_ are the simplest kind of regular expression. They’re similar to a “find” operation in a word processor or text editor. For example, the single-character regular expression `t` will find all occurrences of the letter “t”, and the regular expression `hello` will find all appearances of “hello”. Pretty straightforward!

Just like a programming language, there are some “reserved” characters in regular expression syntax, as follows:

`[`
`( and )`
`\`
`*`
`+`
`?`
`{ and }`
`^`
`$`
`.`
`|(pipe)`
`/`

These characters are used for advanced pattern matching. If you want to search for one of these characters, you need to escape it with a backslash. For example, to search for all periods in a block of text, the pattern is not `.` but rather `\.` .

As an extra complication, since regular expressions are strings themselves, the backslash character needs to be escaped when working with `NSString` and `NSRegularExpression` . That means the standard regular expression `\.` will be written as `\\.` in your code.

To clarify the above concept in point form:

* The literal `@"\\."` defines a string that looks like this: \.
* The regular expression \. will then match a single period character

_Capturing parentheses_ are used to group part of a pattern. For example, `3 (pm|am)` would match the text "3 pm" as well as the text "3 am". The pipe character here ( `|`) acts like an OR operator. You can include as many pipe characters in your regular expression as you would like. As an example, `(Tom|Dick|Harry)` is a valid pattern.

Grouping with parentheses comes in handy when you need to optionally match a certain text string. Say you are looking for "November" in some text, but the user may or may not have abbreviated the month as "Nov". You can define the pattern as `Nov(ember)?` where the question mark after the capturing parentheses means that whatever is inside the parentheses is optional.

These parentheses are termed "capturing" because they capture the matched content and allow you reference it in other places in your regular expression.

As an example, assume you have the string "Say hi to Harry". If you created a search-and-replace regular expression to replace any occurences of `(Tom|Dick|Harry)` with `that guy $1` , the result would be "Say hi to that guy Harry". The `$1` allows you to reference the first captured group of the preceding rule.

_Character classes_ represent a set of possible single-character matches. Character classes appear between square brackets ( `[` and `]` ).

As an example, the regular expression `t[aeiou]` will match "ta", "te", "ti", "to", or "tu". You can have as many character possibilities inside the square brackets as you like, but remember that any single character in the set will match. `[aeiou]` looks like five characters, but it actually means "a" or "e" or "i" or "o" or "u".

You can also define a range in a character class if the characters appear consecutively. For example, to search for a number between 100 to 109, the pattern would be `10[0-9]` . This returns the same results as 10[0123456789] , but using ranges makes your regular expressions much cleaner and easier to understand.

But character classes aren't limited to numbers — you can do the same thing with characters. For instance, `[a-f]` will match "a", "b", "c", "d", "e", or "f".

Character classes usually contain the characters you want to match, but what if you want to explicitly not match a character? You can also define negated character classes, which use the `^` character. For example, the pattern `t[^o]` will match any combination of "t" and one other character except for the single instance of "to".

### NSRegularExpressions Cheat Sheet

Regular expressions are a great example of a simple syntax that can end up with some very complicated arrangements! Even the best regular expression wranglers keep a cheat sheet handy for those odd corner cases.

There is [NSRegularExpression Cheat Sheet](http://cdn5.raywenderlich.com/downloads/RW-NSRegularExpression-Cheatsheet.pdf) PDF to help!

In addition, here's an abbreviated form of the cheat sheet below with some additional explanations to get started:

* `.` matches any character. `p.p` matches pop, pup, pmp, p@p, and so on.
* `\w` matches any "word-like" character which includes the set of numbers, letters, and underscore, but does not match punctuation or other symbols. `hello\w` will match "hello_9" and "helloo" but not "hello!"
* `\d` matches a numeric digit, which in most cases means `[0-9]` . `\d\d?:\d\d` will match strings in time format, such as "9:30" and "12:45".
* `\b` matches word boundary characters such as spaces and punctuation. `to\b` will match the "to" in "to the moon" and "to!", but it will not match "tomorrow". `\b` is handy for "whole word" type matching.
* `\s` matches whitespace characters such as spaces, tabs, and newlines. `hello\s` will match "hello " in "Well, hello there!".
* `^` matches at the beginning of a line. Note that this particular `^` is different from `^` inside of the square brackets! For example, ^Hello will match against the string "Hello there", but not "He said Hello".
* `$` matches at the end of a line. For example, the `end$` will match against "It was the end" but not "the end was near".
* `*` matches the previous element 0 or more times. `12*3` will match 13, 123, 1223, 122223, and 1222222223.
* `+` matches the previous element 1 or more times. 12+3 will match 123, 1223, 122223, 1222222223, but not 13.
* Curly braces `{}` contain the minimum and maximum number of matches. For example, `10{1,2}1` will match both "101" and "1001" but not "10001" as the minimum number of matches is `1` and the maximum number of matches is `2` . `He[Ll]{2,}o` will match "HeLLo" and "HellLLLllo" and any such silly variation of "hello" with lots of L's, since the minimum number of matches is 2 but the maximum number of matches is not set — and therefore unlimited!

That's _may_ enough to get started!




