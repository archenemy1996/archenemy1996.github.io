---
layout: post
title: Intro to Docker
categories: [Write-Ups, Try Hack Me]
tags: [TryHackMe, Docker, Containerization]
featured-image: N/A
featured-image-alt: N/A 
---

It's a write-up about the room : [Try Hack Me - Room : Intro to Docker](https://tryhackme.com/room/introtodockerk8pdqk)

# Task 1 : Introduction

In this room, you’ll get your first hands-on experience deploying and interacting with Docker containers.

Namely, by the end of the room, we will be familiar with the following:

 - The basic syntax to get you started with Docker.
 - Running and deploying your first container.
 - Understanding how Docker containers are distributed using images.
 - Creating your own image using a Dockerfile.
 - How Dockerfiles are used to build containers, using Docker Compose to orchestrate multiple containers.
 - Applying the knowledge gained from the room into the practical element at the end.

 ### Questions 

 - Mark complete the question

# Task 2 : Basic Docker Syntax

The syntax for Docker can be categorised into four main groups:

 - Running a container
 - Managing & Inspecting containers
 - Managing Docker images
 - Docker daemon stats and information

 Managing Docker Images

`Docker Pull`

In this room, we will use the Nginx image to run a web server within a container. Before downloading the image, let’s break down the commands and syntax required to download an image. Images can be downloaded using the `docker pull` command and providing the name of the image.

For example, `docker pull nginx`. Docker must know where to get this image (such as from a repository which we’ll come onto in a later task).

Continuing with our example above, let’s download this Nginx image! using `docker pull nginx`
By running this command, we are downloading the latest version of the image titled “nginx”. Images have these labels called tags. These tags are used to refer to variations of an image. For example, an image can have the same name but different tags to indicate a different version. I’ve provided an example of how tags are used within the table below:

<table>
	<tbody>
		<tr>
			<td><b>Docker Image</b></td>
			<td><b>Tag</b></td><td>
				<b>Command Example</b></td>
				<td><b>Explanation</b></td></tr>
				<tr><td>ubuntu</td><td>latest</td>
					<td><p>docker pull ubuntu</p><p><b>- IS THE SAME AS -</b></p><p>docker pull ubuntu:latest</p></td><td><p>This command will pull the latest version of the "ubuntu" image. If no tag is specified, Docker will assume you want the "latest" version if no tag is specified.</p><p>It is worth remembering that you do not always want the "latest". This image is quite literally the "latest" in the sense it will have the most recent changes. This could either fix or break your container.</p></td></tr><tr><td>ubuntu</td><td>22.04</td><td>docker pull ubuntu:22.04</td><td>This command will pull version "22.04 (Jammy)" of the "ubuntu" image.<br></td></tr><tr><td>ubuntu</td><td>20.04</td><td>docker pull ubuntu:20.04</td><td>This command will pull version "20.04 (Focal)" of the "ubuntu" image.</td></tr><tr><td>ubuntu</td><td>18.04</td><td>docker pull ubuntu:18.04</td><td><p>This command will pull version "18.04 (Bionic)" of the "ubuntu" image.</p></td>
</tr>
</tbody>
</table>

