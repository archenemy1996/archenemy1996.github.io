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
Regular expressions (or Regex) are patterns of text that we define to search documents and match exactly what we are looking for.

- Why should we learn them?  
We will likely need them sooner or later, it's a great tool to know how to use. It will make us more capable in CTF's, and potentially a better developer.

- Goals  
In this room we will create a text file with some test paragraphs (in a Unix machine) and then use egrep <pattern> <file> to see what matches and what doesn't, or we can use an online editor like `https://regexr.com/` we can add our own text in the "Text" field, and then type our expressions (patterns) in the "Expression" field.

# Task 2 : Charsets

When we are searching for a specific string in a file or block of text, we can search for it as is, with `grep 'string' <file>` . But what happens if we want to search for patterns of text? For example, we could be looking for a word that starts with a specific letter, or any words that end with numbers. That's where Regular Expressions come in.

Both of the aforementioned problems can be solved by using charsets. A charset is defined by enclosing in `[` `square brackets` `]` the character(s), or range of characters that we want to match.  Then, it finds every occurrence of the pattern you have defined in the file/text you are searching.

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

<b>Note 1</b>:  
Don't confuse strings with charsets. The charset `[abc]` will match the string `abc`, but also `cba` and `ca`. It doesn't match the string, but rather every <b>occurrence</b> of the specified characters in that string.

<b>Note 2</b>:  
When specifying charsets, we should type the letters in the same order they appear in the questions, to avoid typing something correct that is not the right answer.

<b>Note 3</b>:  
<b>Answering some of these questions is going to be tricky</b>. Often times there are many different patterns that match specific strings. That means (as stated in the previous note) that you may find a proper solution that isn't the right answer for this room (because there can only be one). The right answer is typically the most efficient regex for that question. Efficient in this context means 2 things:  

    1. Be specific. Here's an example: you could match any character from a to c using the `[a-z]` charset. But if the question only requires you to match characters from `a` to `c`, you should use the `[a-c]` charset, not `[a-z]`.  

    2. Don't be too specific. In contrast to the previous example, if a question requires you to match `a, c, f, r, s, z,` at that point, the expression that matches those specific characters would get longer and more complicated. So, it would make more sense to use `[a-z]`, because it is short and simple.

To reiterate,<b> there cannot be one single correct solution </b>. So if we've tested your solution and it works, we can take a break and come back to it later.

### Answer the questions below 

<b> Question </b> : 
Match all of the following characters: c, o, g  
<b> Answer </b> : __`[cog]`__

<b> Question </b> : 
Match all of the following words: cat, fat, hat  
<b> Answer </b> : __`[cfh]at`__

<b> Question </b> : 
Match all of the following words: Cat, cat, Hat, hat  
<b> Answer </b> : __`[CcHh]at`__

<b> Question </b> : 
Match all of the following filenames: File1, File2, file3, file4, file5, File7, file9  
<b> Answer </b> : __`[Ff]ile[1-9]`__

<b> Question </b> : 
Match all of the filenames of question 4, except "File7" (use the hat symbol)  
<b> Answer </b> : __`[Ff]ile[^7]`__

# Task 3 : Wildcards and optional characters

The wildcard that is used to match any single character (except the line break) is the `.` dot. That means that a.c will match `aac`, `abc`, `a0c`, `a!c`, and so on.

Also, we can set a character as optional in our pattern using the `?` question mark. That means that `abc?` will match `ab` and `abc`, since the `c` is optional.

*Note*: If we want to search for . `a` literal dot, you have to escape it with a `\` reverse slash. That means that `a.c` will match `a.c`, but also `abc`, `a@c`, and so on. But `a\.c` will match just `a.c`.

### Answer the questions below 

<b> Question </b> :
Match all of the following words: Cat, fat, hat, rat  
<b> Answer </b> : __` .at `__

<b> Question </b> :
Match all of the following words: Cat, cats  
<b> Answer </b> : __` [Cc]ats? `__

<b> Question </b> :
Match the following domain name: cat.xyz  
<b> Answer </b> : __` cat\.xyz `__

<b> Question </b> :
Match all of the following domain names: cat.xyz, cats.xyz, hats.xyz  
<b> Answer </b> : __` [ch]ats?\.xyz `__

<b> Question </b> :
Match every 4-letter string that doesn't end in any letter from n to z  
<b> Answer </b> : __` ...[^n-z] `__

<b> Question </b> :
Match bat, bats, hat, hats, but not rat or rats (use the hat symbol)  
<b> Answer </b> : __` [^r]ats? `__

# Task 4 : Metacharacters and repetitions  

There are easier ways to match bigger charsets. For example, `\d` is used to match any single digit. 

Here's a reference:  
`\d` matches a digit, like `9 ` 
`\D` matches a non-digit, like `A` or `@`  
`\w` matches an alphanumeric character, like a or `3`  
`\W` matches a non-alphanumeric character, like `!` or `#`  
`\s` matches a whitespace character `(spaces, tabs, and line breaks)`  
`\S` matches everything else `(alphanumeric characters and symbols)`

Note: Underscores `_ `are included in the `\w` metacharacter and not in `\W`. That means that `\w` will match every single character in test_file.  

Often we want a pattern that matches many characters of a single type in a row, and we can do that with repetitions. For example, `{2}` is used to match the preceding character (or `metacharacter`, or `charset`) two times in a row. That means that `z{2}` will match exactly `zz`.  

Here's a reference for each repetition along with how many times it matches the preceding pattern:  

`{12}` - exactly `12` times.  
`{1,5}` - `1` to `5` times.  
`{2,}` - `2` or more times.  
`*` - `0` or more times.  
`+` - `1` or more times.  

### Answer the questions below 

<b> Question </b> :
Match the following word: catssss  
<b> Answer </b> : __`cats{4}`__  

<b> Question </b> :
Match all of the following words (use the * sign): Cat, cats, catsss  
<b> Answer </b> : __``__  

<b> Question </b> :
Match all of the following sentences (use the + sign): regex go br, regex go brrrrrr  
<b> Answer </b> : __``__  

<b> Question </b> :
Match all of the following filenames: ab0001, bb0000, abc1000, cba0110, c0000 (don't use a metacharacter)  
<b> Answer </b> : __``__  

<b> Question </b> :
Match all of the following filenames: File01, File2, file12, File20, File99  
<b> Answer </b> : __``__  

<b> Question </b> :
Match all of the following folder names: kali tools, kali     tools  
<b> Answer </b> : __``__  

<b> Question </b> :
Match all of the following filenames: notes~, stuff@, gtfob#, lmaoo!  
<b> Answer </b> : __``__  

<b> Question </b> :
Match the string in quotes (use the * sign and the \s, \S metacharacters): "2f0h@f0j0%!     a)K!F49h!FFOK"  
<b> Answer </b> : __``__  

<b> Question </b> :
Match every 9-character string (with letters, numbers, and symbols) that doesn't end in a "!" sign  
<b> Answer </b> : __``__  

<b> Question </b> :
Match all of these filenames (use the + symbol): .bash_rc, .unnecessarily_long_filename, and note1    
<b> Answer </b> : __``__  

<b> Question </b> :
  
<b> Answer </b> : __``__  

<b> Question </b> :
  
<b> Answer </b> : __``__  