---
layout: post
title: Task-14 [Day 9] Pivoting Dock the halls
categories: [Write-Ups, Try Hack Me]
tags: [TryHackMe, Network, Wireshark]
featured-image:  _-_/icon.jpeg
featured-image-alt: Pivoting
---

It's a write-up about the room : [Try Hack Me - Room : Advent of Cyber 2022](https://tryhackme.com/room/adventofcyber4)

# Learning Objectives

 - Using Metasploit modules and Meterpreter to compromise systems
 - Network Pivoting
 - Post exploitation

# Concepts

### What is Docker?

Docker is a way to package applications, and the associated dependencies into a single unit called an image. This image can then be shared and run as a container, either locally as a developer or remotely on a production server. Santa’s web application and database are running in Docker containers, but only the web application is directly available via an exposed port. A common way to tell if a compromised application is running in a Docker container is to verify the existence of  a **/.dockerenv** file at the root directory of the filesystem.

### What is Metasploit? 
Metasploit is a powerful penetration testing tool for gaining initial access to systems, performing post-exploitation, and pivoting to other applications and systems. Metasploit is free, open-source software owned by the US-based cybersecurity firm Rapid7.

#### What is a Metasploit session?
After successfully exploiting a remote target with a Metasploit module, a session is often opened by default. These sessions are often Command Shells or Meterpreter sessions, which allow for executing commands against the target. It’s also possible to open up other session types in Metasploit, such as SSH or WinRM - which do not require payloads.

The common Metasploit console commands for viewing and manipulating sessions in Metasploit are:

![image](https://github.com/archenemy1996/archenemy1996.github.io/blob/main/_posts/Try_Hack_Me/Advent%20of%20Cyber%202022/carbon_png/Metasploit%20Console%20Commands%20-%201.pngIsolated.png")

