---
layout: post
title: Bash Scripting
categories: [Write-Ups, Try Hack Me]
tags: [TryHackMe, Bash, Scripting, Shell ]
featured-image:  bash_image/icon.jpeg
featured-image-alt: Bash
---

It's a write-up about the room : [Try Hack Me - Room : Bash Scripting](https://tryhackme.com/room/bashscripting)

  - # Task 1 : Introduction

  ### What is bash?

  Bash is a scripting language that runs within the terminal on most Linux distros, as well as MacOS. Shell scripts are a sequence of bash commands within a file, combined together to achieve more complex tasks than simple one-liner and are especially useful when it comes to automating sysadmin tasks such as backups.

  In this room we will be working with 
  - Bash syntax
  - Variables
  - Using parameters
  - Arays
  - Conditionals

  - # Task 2 : Our first bash script

  A bash script always starts with the following line of code at the top of the script. This is so our shell (whatever type of it) knows that it needs to run our file using bash in the terminal.

  ```bash
  #!/bin/bash
  ```
  Lets get into some basic examples.

  ```bash
  #!/bin/bash
  echo "Hello World"
  ```

  This will return the string “Hello World”. 
  The command “echo” is used to output text to the screen, the same way as “print” in python. we can also perform normal Linux commands inside our bash script and it will be executed if formatted right. For example we can run the command “ls” inside our bash script and we will see the output when we run the file. So lets make it do this!

  ```bash
  #!/bin/bash

  echo "Hello World"

  whoami

  id
  ```
  Now to run our bash script we must first give it executable permissions, And then we run it using `./`

  ```bash
  chmod +x yourfile.sh
  ```

  ### Answer the questions below

  - What piece of code can we insert at the start of a line to comment out our code?

  Solution: `#`

  - What will the following script output to the screen, echo “BishBashBosh”

  Solution: `BishBashBosh`


  - # Task 3 : Variables

  In bash variables are quite simple and we create them like so:

  ```bash
  name = "Jammy"
  ```
  Where we give the value of `Jammy` and assign it to the variable `name`.

  Please note that for variables to work you cannot leave a space between the variable name, the ”=” and the value. They cannot have spaces in.
  
  So how would we now use our variable? Well its also very simple. We have to add a `$` onto front of our variable name in order to use it.

  ```bash
  name="Jammy"

  echo $name

  ```

  If we test this out in our own terminal we get something like this:

  `$ name="Jammy"`

  `$ echo $name`

  `Jammy`

