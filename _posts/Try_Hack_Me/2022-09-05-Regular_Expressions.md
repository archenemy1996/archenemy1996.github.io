---
layout: post
title: Regular Expressions
categories: [Write-Ups, Try Hack Me]
tags: [TryHackMe, patterns, regex]
featured-image: N/A
featured-image-alt: N/A 
---

It's a write-up about the room : [Try Hack Me - Room : Regular Expressions](https://tryhackme.com/room/catregex)

# Task 1 : Introduction

- What are regular expression?
Regular expressions (or Regex) are patterns of text that you define to search documents and match exactly what you're looking for.

- Why should we learn them?
Even if you won't need them sooner or later, it's a great tool to know how to use. It will make you more capable in CTF's, and potentially a better developer if that's a goal you have. You spend a little time learning it and save yourself lots of time in the long run by using it.

- Goals - 
In this room we will create a text file with some test paragraphs (in a Unix machine) and then use egrep <pattern> <file> to see what matches and what doesn't, or
use an online editor like `https://regexr.com/` You can add your own text in the "Text" field, and then type your expressions (patterns) in the "Expression" field.

# Task 2 : Charsets


When we are searching for a specific string in a file or block of text, we can search for it as is, with `grep 'string' <file>` . But what happens if you want to search for patterns of text? For example, you could be looking for a word that starts with a specific letter, or any words that end with numbers. That's where Regular Expressions come in.

Both of the aforementioned problems can be solved by using charsets. A charset is defined by enclosing in `[` `square brackets` `]` the character(s), or range of characters that you want to match.  Then, it finds every occurrence of the pattern you have defined in the file/text you are searching.

`[abc]` will match `a, b,` and `c` (every occurrence of each letter)

`[abc]zz` will match `azz, bzz, `and `czz`.

You can also use a `-` dash to define ranges:
`[a-c]zz` is the same as above.

And then you can combine ranges together:
`[a-cx-z]zz` will match `azz, bzz, czz, xzz, yzz,` and `zzz`.

Most notably, this can be used to match any alphabetical character:
`[a-zA-Z]` will match any single letter (lowercase or uppercase).

You can use numbers too:
`file[1-3]` will match `file1, file2,` and `file3`.

Then, there is a way to exclude characters from a charset with the `^` hat symbol, and include everything else.
`[^k]ing` will match `ring, sing, $ing,` but not `king`.

Of course, you can exclude charsets, not just single characters.
`[^a-c]at` will match `fat` and `hat`, but not `bat` or `cat`.

Note 1: Don't confuse strings with charsets. The charset `[abc]` will match the string `abc`, but also `cba` and `ca`. It doesn't match the string, but rather every <b>occurrence</b> of the specified characters in that string.

<b>Note 2</b>: When specifying charsets, you should type the letters in the same order they appear in the questions, to avoid typing something correct that is not the right answer.

<b>Note 3</b>: <b>Answering some of these questions is going to be tricky</b>. Often times there are many different patterns that match specific strings. That means (as stated in the previous note) that you may find a proper solution that isn't the right answer for this room (because there can only be one). The right answer is typically the most efficient regex for that question. Efficient in this context means 2 things:
    1. Be specific. Here's an example: you could match any character from a to c using the `[a-z]` charset. But if the question only requires you to match characters from `a` to `c`, you should use the `[a-c]` charset, not `[a-z]`.
    2. Don't be too specific. In contrast to the previous example, if a question requires you to match `a, c, f, r, s, z,` at that point, the expression that matches those specific characters would get longer and more complicated. So, it would make more sense to use `[a-z]`, because it is short and simple.

To reiterate,<b> there cannot be one single correct solution </b>. So if we've tested your solution and it works, we can take a break and come back to it later.

### Answer the questions below 

<b> Question </b> : Match all of the following characters: c, o, g  
<b> Answer </b> : `[cog]`


<b> Question </b> : Match all of the following words: cat, fat, hat  
<b> Answer </b> : `[cfh]at`

<b> Question </b> : 
Match all of the following words: Cat, cat, Hat, hat  
<b> Answer </b> : `[CcHh]at`

<b> Question </b> : 
Match all of the following filenames: File1, File2, file3, file4, file5, File7, file9  
<b> Answer </b> : [Ff]ile[1-9]

<b> Question </b> : 
Match all of the filenames of question 4, except "File7" (use the hat symbol)  
<b> Answer </b> : `[Ff]ile[^7]`

# Task 3 : Wildcards and optional characters

The wildcard that is used to match any single character (except the line break) is the `.` dot. That means that a.c will match `aac`, `abc`, `a0c`, `a!c`, and so on.

Also, you can set a character as optional in your pattern using the `?` question mark. That means that `abc?` will match `ab` and `abc`, since the `c` is optional.

*Note*: If you want to search for . `a` literal dot, you have to escape it with a `\` reverse slash. That means that `a.c` will match `a.c`, but also `abc`, `a@c`, and so on. But `a\.c` will match just `a.c`.