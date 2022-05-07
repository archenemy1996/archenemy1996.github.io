---
layout: post
title: Write-Up [THM] Metasploit
categories: [Write-Ups, TryHackMe]
tags: [TryHackMe, Networking, Red Primer Series, Metasploit]
featured-image:  thm/metasploit/theme.png
featured-image-alt: Metasploit
---

It's a write-up about the room : [Try Hack Me - Room : Metasploit](https://tryhackme.com/room/rpmetasploit)

# [Task 1] - Intro

> Metasploit, an open-source pentesting framework, is a powerful tool utilized by security engineers around the world. Maintained by Rapid 7, Metasploit is a collection of not only thoroughly tested exploits but also auxiliary and post-exploitation tools. Throughout this room, we will explore the basics of using this massive framework and a few of the modules it includes. 

# [Task 2] - Initializing 

We initialize  the database with `msfdb init`.

> Before starting Metasploit, we can view some of the advanced options we can trigger for starting the console. Check these out now by using the command: `msfconsole -h`

* We can start the Metasploit console on the command line without showing the banner or any startup information as well. What switch do we add to msfconsole to start it without showing this information? `-q`

We start `Metsploit` via the command `msfconsole`.

* Cool! We've connected to the database, which type of database does Metasploit 5 use?  
We used the command : `db_status`and found the type of database used by Metasploit 5 **postgresql**.

# [Task 3] - Rock'em to the Core

> Using the help menu, let's now learn the base commands and the module categories in Metasploit. Nearly all of the answers to the following questions can be found in the Metasploit help menu.

* The help menu has a very short one-character alias, what is it? `?`

* Finding various modules we have at our disposal within Metasploit is one of the most common commands we will leverage in the framework. What is the base command we use for searching? `search`

* Once we've found the module we want to leverage, what command we use to select it as the active module? `use`

* How about if we want to view information about either a specific module or just the active one we have selected? `info`

* Metasploit has a built-in netcat-like function where we can make a quick connection with a host simply to verify that we can 'talk' to it. What command is this? `connect`

* Entirely one of the commands purely utilized for fun, what command displays the motd/ascii art we see when we start msfconsole (without -q flag)? `banner`

* We'll revisit these next two commands shortly, however, they're two of the most used commands within Metasploit. First, what command do we use to change the value of a variable? `set`

* Metasploit supports the use of global variables, something which is incredibly useful when you're specifically focusing on a single box. What command changes the value of a variable globally? `setg`

* Now that we've learned about to change the value of variables, how do we view them? There are technically several answers to this question, however, I'm looking for a specific three-letter command which is used to view the value of single variables. `get`

* How about changing the value of a variable to null/no value? `unset`

* When performing a penetration test it's quite common to record your screen either for further review or for providing evidence of any actions taken. This is often coupled with the collection of console output to a file as it can be incredibly useful to grep for different pieces of information output to the screen. What command can we use to set our console output to save to a file? `spool`

* Leaving a Metasploit console running isn't always convenient and it can be helpful to have all of our previously set values load when starting up Metasploit. What command can we use to store the settings/active datastores from Metasploit to a settings file? This will save within your msf4 (or msf5) directory and can be undone easily by simply removing the created settings file. `save`

# [Task 4] - Module for every Occasion!

![Task 4](/assets/img/thm/metasploit/task-4.png)

* Easily the most common module utilized, which module holds all of the exploit code we will use? `exploit`

* Used hand in hand with exploits, which module contains the various bits of shellcode we send to have executed following exploitation? `payload`

* Which module is most commonly used in scanning and verification machines are exploitable? This is not the same as the actual exploitation of course. `auxiliary`

* One of the most common activities after exploitation is looting and pivoting. Which module provides these capabilities? `post`

* Commonly utilized in payload obfuscation, which module allows us to modify the 'appearance' of our exploit such that we may avoid signature detection? `encoder`

* Last but not least, which module is used with buffer overflow and ROP attacks? `NOP`

* Not every module is loaded in by default, what command can we use to load different modules? `load`

# [Task 5] - Move that shell!

![Task 5](/assets/img/thm/metasploit/task-5.png)

We use the command `db_nmap -sV BOX-IP`.

* What service does nmap identify running on port 135? `msrpc`

* Now that we've scanned our victim system, let's try connecting to it with a Metasploit payload. First, we'll have to search for the target payload. In Metasploit 5 (the most recent version at the time of writing) you can simply type 'use' followed by a unique string found within only the target exploit. For example, try this out now with the following command 'use icecast'. What is the full path for our exploit that now appears on the msfconsole prompt? `exploit/windows/http/icecast_header`

* While that use command with the unique string can be incredibly useful that's not quite the exploit we want here. Let's now run the command 'search multi/handler'. What is the name of the column on the far left side of the console that shows up next to 'Name'? Go ahead and run the command `use NUMBER_NEXT_TO exploit/multi/handler` wherein the number will be what appears in that far left column (typically this will be 4 or 5). In this way, we can use our search results without typing out the full name/path of the module we want to use. `#`

# [Task 6] - We're in, now what?

> Now that we've got a shell into our victim machine, let's take a look at several post-exploitation modules actions we can leverage! Most of the questions in the following section can be answered by using the Meterpreter help menu which can be accessed through the 'help' command. This menu dynamically expands as we load more modules.

* First things first, our initial shell/process typically isn't very stable. Let's go ahead and attempt to move to a different process. First, let's list the processes using the command 'ps'. What's the name of the spool service? `spoolsv.exe`

* Let's go ahead and move into the spool process or at least attempt to! What command do we use to transfer ourselves into the process? This won't work at the current time as we don't have sufficient privileges but we can still try! `migrate`

* Well that migration didn't work, let's find out some more information about the system so we can try to elevate. What command can we run to find out more information regarding the current user running the process we are in? `getuid`

* How about finding more information out about the system itself? `sysinfo`

* This might take a little bit of googling, what do we run to load mimikatz (more specifically the new version of mimikatz) so we can use it? `load kiwi`

* Let's go ahead and figure out the privileges of our current user, what command do we run? `getprivs`

* What command do we run to transfer files to our victim computer? `upload`

* How about if we want to run a Metasploit module? `run`

* A simple question but still quite necessary, what command do we run to figure out the networking information and interfaces on our victim? `ipconfig`

* One quick extra question, what command can we run in our meterpreter session to spawn a normal system shell? `shell`

# [Task 7] - Makin' Cisco Proud

> Last but certainly not least, let's take a look at the autorouting options available to us in Metasploit. While our victim machine may not have multiple network interfaces (NICs), we'll walk through the motions of pivoting through our victim as if it did have access to extra networks.

* Let's go ahead and run the command `run autoroute -h`, this will pull up the help menu for autoroute. What command do we run to add a route to the following subnet: 172.18.1.0/24? Use the -n flag in your answer. `run autoroute -s 172.18.1.0 -n 255.255.255.0`

* Additionally, we can start a socks4a proxy server out of this session. Background our current meterpreter session and run the command `search server/socks4a`. What is the full path to the socks4a auxiliary module? `auxiliary/server/soks4a`

* Once we've started a socks server we can modify our /etc/proxychains.conf file to include our new server. What command do we prefix our commands (outside of Metasploit) to run them through our socks4a server with proxychains? `proxychains`
