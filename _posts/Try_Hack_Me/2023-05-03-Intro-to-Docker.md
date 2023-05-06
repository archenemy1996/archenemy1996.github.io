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

<table class="table table-bordered"><tbody><tr><td><b>Docker Image</b></td><td><b>Tag</b></td><td><b>Command Example</b></td><td><b>Explanation</b></td></tr><tr><td>ubuntu</td><td>latest</td><td><p>docker pull ubuntu</p><p><b>- IS THE SAME AS -</b></p><p>docker pull ubuntu:latest</p></td><td><p>This command will pull the latest version of the "ubuntu" image. If no tag is specified, Docker will assume you want the "latest" version if no tag is specified.</p><p>It is worth remembering that you do not always want the "latest". This image is quite literally the "latest" in the sense it will have the most recent changes. This could either fix or break your container.</p></td></tr><tr><td>ubuntu</td><td>22.04</td><td>docker pull ubuntu:22.04</td><td>This command will pull version "22.04 (Jammy)" of the "ubuntu" image.<br></td></tr><tr><td>ubuntu</td><td>20.04</td><td>docker pull ubuntu:20.04</td><td>This command will pull version "20.04 (Focal)" of the "ubuntu" image.</td></tr><tr><td>ubuntu</td><td>18.04</td><td>docker pull ubuntu:18.04</td><td><p>This command will pull version "18.04 (Bionic)" of the "ubuntu" image.</p></td></tr></tbody></table>

When specifying a tag, you must include a colon `:` between the image `name` and `tag`, for example, `ubuntu:22.04 (image:tag)`. Don’t forget about tags `-` we will return to these in a future task!

### Docker Image x/y/z
The docker image command, with the appropriate option, allows us to manage the images on our local system. To list the available options, we can simply do docker image to see what we can do. I’ve done this for you in the terminal below:

```bash 
cmnatic@thm:~$ docker image

Usage:  docker image COMMAND

Manage images

Commands:
  build       Build an image from a Dockerfile
  history     Show the history of an image
  import      Import the contents from a tarball to create a filesystem image
  inspect     Display detailed information on one or more images
  load        Load an image from a tar archive or STDIN
  ls          List images
  prune       Remove unused images
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rm          Remove one or more images
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

Run 'docker image COMMAND --help' for more information on a command.
```
In this room, we are only going to cover the following options for docker images:

 - pull (we have done this above!)
 - ls (list images)
 - rm (remove an image)
 - build (we will come onto this in the “Building our First Container” task)

### Docker Image ls

This command allows us to list all images stored on the local system. We can use this command to verify if an image has been downloaded correctly and to view a little bit more information about it (such as the tag, when the image was created and the size of the image).

```bash
cmnatic@thm:~$ docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       22.04     2dc39ba059dc   10 days ago   77.8MB
nginx        latest    2b7d6430f78d   2 weeks ago   142MB
cmnatic@thm:~$
```
### Docker Image rm

If we want to remove an image from the system, we can use `docker image rm` along with the `name (or Image ID)`. In the following example, I will remove the `"ubuntu"` image with the tag "22.04". My command will be `docker image rm ubuntu:22.04`

It is important to remember to include the `tag` with the image name.

```bash
cmnatic@thm:~$ docker image rm ubuntu:22.04
Untagged: ubuntu:22.04
Untagged: ubuntu@sha256:20fa2d7bb4de7723f542be5923b06c4d704370f0390e4ae9e1c833c8785644c1
Deleted: sha256:2dc39ba059dcd42ade30aae30147b5692777ba9ff0779a62ad93a74de02e3e1f
Deleted: sha256:7f5cbd8cc787c8d628630756bcc7240e6c96b876c2882e6fc980a8b60cdfa274
cmnatic@thm:~$
```
If we were to run a `docker image ls`, we would see that the image is no longer listed:

```bash
cmnatic@thm:~$ docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    2b7d6430f78d   2 weeks ago   142MB
cmnatic@thm:~$
```

## Questions and Answers:

__Q__ If we wanted to `pull` a docker image, what would our command look like?
 - __A__ `docker pull`

__Q__ If we wanted to list all images on a device running Docker, what would our command look like?
 - __A__ `docker image ls`

__Q__ Let's say we wanted to pull the image "tryhackme" (no quotations); what would our command look like?
 - __A__ `docker pull tryhackme`

__Q__ Let's say we wanted to pull the image "tryhackme" with the tag "1337" (no quotations). What would our command look like?
 - __A__ `docker pull tryhackme:1337`


# Task 3 : Running Your First Container

The Docker run command creates running containers from images. This is where commands from the Dockerfile (as well as our own input at runtime) are run. Because of this, it must be some of the first syntaxes we learn.

The command works in the following way:`docker run [OPTIONS] IMAGE_NAME [COMMAND] [ARGUMENTS...]`  the options enclosed in brackets are not required for a container to run.

Docker containers can be run with various options - depending on how we will use the container. This task will explain some of the most common options that you may want to use.

First, Simply Running a Container

Let's recall the syntax required to run a Docker container: `docker run [OPTIONS] IMAGE_NAME [COMMAND] [ARGUMENTS...]` . In this example, I am going to configure the container to run:

 - An image named `"helloworld"`

 - `"Interactively"` by providing the `-it` switch in the [OPTIONS] command. This will allow us to interact with the container directly.

 - I am going to spawn a shell within the container by providing `/bin/bash` as the `[COMMAND]` part. This argument is where we will place what commands you want to run within the container (such as a file, application or shell!)
 
 - So, to achieve the above, my command will look like the following: `docker run -it helloworld /bin/bash`

 ```bash
 cmnatic@thm-intro-to-docker:~$ docker run -it helloworld /bin/bash
 root@30eff5ed7492:/#
 ```
We can verify that we have successfully launched a shell because our prompt will change to another user account and hostname. The hostname of a container is the container ID (which can be found by using docker ps). For example, in the terminal above, our username and hostname are root@30eff5ed7492

Running Containers...Continued

As previously mentioned, Docker containers can be run with various options. The purpose of the container and the instructions set in a Dockerfile (we'll come onto this in a later task) determines what options we need to run the container with. To start, I've put some of the most common options you may need to run your Docker container into the table below.


<table class="table table-bordered"><tbody><tr><td><b>[OPTION]</b></td><td><b>Explanation</b></td><td><b>Relevant Dockerfile Instruction</b></td><td><b>Example</b></td></tr><tr><td>-d</td><td>This argument tells the container to start in "detached" mode. This means that the container will run in the background.</td><td>N/A</td><td><p><code>docker run -d helloworld</code></p></td></tr><tr><td>-it</td><td>This argument has two parts. The "i" means run interactively, and "t" tells Docker to run a shell within the container. We would use this option if we wish to interact with the container directly once it runs.</td><td>N/A</td><td><p><code>docker run -it helloworld</code></p></td></tr><tr><td>-v</td><td>This argument is short for "Volume" and tells Docker to mount a directory or file from the host operating system to a location within the container. The location these files get stored is defined in the Dockerfile</td><td>VOLUME</td><td><p><code>docker run -v /host/os/directory:/container/directory helloworld</code></p></td></tr><tr><td>-p</td><td>This argument tells Docker to bind a port on the host operating system to a port that is being exposed in the container. You would use this instruction if you are running an application or service (such as a web server) in the container and wish to access the application/service by navigating to the IP address.</td><td>EXPOSE</td><td><p><code>docker run -p 80:80 webserver</code></p></td></tr><tr><td>--rm</td><td>This argument tells Docker to remove the container once the container finishes running whatever it has been instructed to do.</td><td>N/A</td><td><p><code>docker run --rm helloworld</code></p></td></tr><tr><td>--name</td><td>This argument lets us give a friendly, memorable name to the container. When a container is run without this option, the name is two random words. We can use this open to name a container after the application the container is running.</td><td><p>N/A</p></td><td><p><code>docker run --name helloworld</code> </p></td></tr></tbody></table>


These are just some arguments we can provide when running a container. Again, most arguments we need to run will be determined by how the container is built. However, arguments such as --rm and --name will instruct Docker on how to run the container. Other arguments include (but are not limited to!):

 - Telling Docker what network adapter the container should use.
 - What capabilities the container should have access to. 
 - Storing a value into an environment variable.

### Listing Running Containers

To list running containers, we can use the `docker ps` command. This command will list containers that are currently running - like so:

```bash
cmnatic@thm:~/intro-to-docker$ docker ps
CONTAINER ID   IMAGE                           COMMAND        CREATED        STATUS      PORTS     NAMES                                                                                      
                             
a913a8f6e30f   cmnatic/helloworld:latest   "sleep"   1 months ago   Up 3 days   0.0.0.0:8000->8000/tcp   helloworld
cmnatic@thm:~/intro-to-docker$
```

his command will also show information about the container, including:

 - The container's ID
 - What command is the container running
 - When was the container created
 - How long has the container been running
 - What ports are mapped
 - The name of the container

Tip: To list all containers (even stopped), you can use `docker ps -a`:

```bash
cmnatic@thm:~/intro-to-docker$ docker ps -a
CONTAINER ID   IMAGE                             COMMAND                  CREATED             STATUS     PORTS    NAMES                                                                                  
00ba1eed0826   gobuster:cmnatic                  "./gobuster dir -url…"   an hour ago   Exited an hour ago practical_khayyam
```

## Questions and Answers:

__Q__ What would our command look like if we wanted to run a container interactively?
Note: Assume we are not specifying any image here.
 - __A__ `docker run -it`


__Q__ What would our command look like if we wanted to run a container in "detached" mode?
Note: Assume we are not specifying any image here.
 - __A__ `docker run -d`

__Q__ Let's say we want to run a container that will run and bind a webserver on port 80. What would our command look like?
Note: Assume we are not specifying any image here.
- __A__ `docker run -p 80:80`

__Q__ How would we list all running containers?
 - __A__ `docker ps`

__Q__ Now, how would we list all containers (including stopped)?
 - __A__ `docker ps -a`


# Task 4 : Intro to Dockerfiles

Dockerfiles play an essential role in Docker. Dockerfiles is a formatted text file which essentially serves as an instruction manual for what containers should do and ultimately assembles a Docker image.

You use Dockerfiles to contain the commands the container should execute when it is built. To get started with Dockerfiles, we need to know some basic syntax and instructions. Dockerfiles are formatted in the following way:

`INSTRUCTION argument`

First, let’s cover some essential instructions:

<table class="table table-bordered"><tbody><tr><td><b>Instruction</b></td><td><b>Description</b></td><td><b>Example</b></td></tr><tr><td>FROM</td><td>This instruction sets a build stage for the container as well as setting the base image (operating system). All Dockerfiles must start with this.</td><td>FROM ubuntu</td></tr><tr><td>RUN</td><td>This instruction will execute commands in the container within a new layer.</td><td>RUN whoami</td></tr><tr><td>COPY</td><td><p>This instruction copies files from the local system to the working directory in the container (the syntax is similar to the <code>cp</code> command).</p></td><td>COPY /home/cmnatic/myfolder/app/</td></tr><tr><td>WORKDIR</td><td><p>This instruction sets the working directory of the container. (similar to using <code>cd</code><span> on <a class="qFJxbyqI glossary-term" onclick="initPopOver('Linux', 'qFJxbyqI')">Linux</a>).</span></p></td><td>WORKDIR /<br>(sets to the root of the filesystem in the container)</td></tr><tr><td>CMD</td><td>This instruction determines what command is run when the container starts (you would use this to start a service or application).</td><td>CMD /bin/sh -c script.sh</td></tr><tr><td>EXPOSE</td><td>This instruction is used to tell the person who runs the container what port they should publish when running the container.</td><td><p>EXPOSE 80</p><p>(tells the person running the container to publish to port 80 i.e. <code>docker run -p 80:80</code>)</p></td></tr></tbody></table>

Now that we understand the core instructions that make up a Dockerfile, let’s see a working example of a Dockerfile. But first, I’ll explain what I want the container to do:

 1. Use the “Ubuntu” (version 22.04) operating system as the base.
 2. Set the working directory to be the root of the container.
 3. Create the text file “helloworld.txt”.

```bash
# THIS IS A COMMENT
# Use Ubuntu 22.04 as the base operating system of the container
FROM ubuntu:22.04

# Set the working directory to the root of the container
WORKDIR / 

# Create helloworld.txt
RUN touch helloworld.txt
```
Remember, the commands that you can run via the `RUN` instruction will depend on the operating system you use in the `FROM` instruction. (In this example, I have chosen Ubuntu. It’s important to remember that the operating systems used in containers are usually very minimal. I.e., don’t expect a command to be there from the start (even commands like *curl, ping, etc.*, may need to be installed.)

### Building Your First Container

Once we have a Dockerfile, we can create an image using the `docker build` command. This command requires a few pieces of information:

 - Whether or not you want to name the image yourself (we will use the `-t` (tag) argument).
 - The name that you are going to give the image.
 - The location of the Dockerfile you wish to build with.

I’ll provide the scenario and then explain the relevant command. Let’s say we want to build an image - let’s fill in the two required pieces of information listed above:
 - We are going to name it ourselves, so we are going to use the `-t` argument.
 - We want to name the image.
 - The Dockerfile is located in our current working directory (`.`).

The Dockerfile we are going to build is the following:

```bash
# Use Ubuntu 22.04 as the base operating system of the container
FROM ubuntu:22.04

# Set the working directory to the root of the container
WORKDIR / 

# Create helloworld.txt
RUN touch helloworld.txt
```

The command would look like so: `docker build -t helloworld .` (__we are using the dot to tell Docker to look in our working directory__). If we have filled out the command right, we will see Docker starting to build the image:

_A terminal showing the building process of the "helloworld" image_

```bash
cmnatic@thm:~$ docker build -t helloworld .
Sending build context to Docker daemon  4.778MB
Step 1/3 : FROM ubuntu:22.04
22.04: Pulling from library/ubuntu
2b55860d4c66: Pull complete
Digest: sha256:20fa2d7bb4de7723f542be5923b06c4d704370f0390e4ae9e1c833c8785644c1
Status: Downloaded newer image for ubuntu:22.04
 ---> 2dc39ba059dc
Step 2/3 : WORKDIR /
 ---> Running in 64d497097f8a
Removing intermediate container 64d497097f8a
 ---> d6bd1253fd4e
Step 3/3 : RUN touch helloworld.txt
 ---> Running in 54e94c9774be
Removing intermediate container 54e94c9774be
 ---> 4b11fc80fdd5
Successfully built 4b11fc80fdd5
Successfully tagged helloworld:latest
cmnatic@thm:~$
```
Great! That looks like a success. Let’s use `docker image ls` to now see if this image has been built:

```bash
Using the "docker image ls" command to confirm whether or not our image has successfully built
cmnatic@thm:~$ docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
helloworld   latest    4b11fc80fdd5   2 minutes ago   77.8MB
ubuntu       22.04     2dc39ba059dc   10 days ago     77.8MB
cmnatic@thm:~$
```

Note: Whatever base operating system you list in the FROM instruction in the Dockerfile will also be downloaded. This is why we can see two images:

 1. helloworld (our image).
 2. ubuntu (the base operating system used in our image).

You will now be able to use this image in a container. Refer to the “Running Your First Container” task to remind you how to start a container.

### Levelling up Our Dockerfile

Let’s level up our Dockerfile. So far, our container will only create a file - that’s not very useful! In the following Dockerfile, I am going to:

 1. Use Ubuntu 22.04 as the base operating system for the container.
 2. Install the “apache2” web server.
 3. Add some networking. As this is a web server, we will need to be able to connect to the container over the network somehow. I will achieve this by using the EXPOSE instruction and telling the container to expose port 80.
 4. Tell the container to start the “apache2” service at startup. Containers do not have service managers like systemd (this is by design - it is bad practice to run multiple applications in the same container. For example, this container is for the apache2 web server - and the apache2 web server only).

 ```bash
 # THIS IS A COMMENT
FROM ubuntu:22.04

# Update the APT repository to ensure we get the latest version of apache2
RUN apt-get update -y 

# Install apache2
RUN apt-get install apache2 -y

# Tell the container to expose port 80 to allow us to connect to the web server
EXPOSE 80 

# Tell the container to run the apache2 service
CMD ["apache2ctl", "-D","FOREGROUND"]
```

For reference, the command to build this would be docker `build -t webserver` . (assuming the Dockerfile is in the same directory as where you run the command from). Once starting the container with the appropriate options (`docker run -d --name webserver -p 80:80  webserver`), we can navigate to the IP address of our local machine in our browser!

__The web server works! Currently, Apache2 is serving the default files because we have not added our own to the container.__

### Optimising Our Dockerfile

There’s certainly an art to Docker - and it doesn’t stop with Dockerfiles! Firstly, we need to ask ourselves why is it essential to optimise our Dockerfile? Bloated Dockerfiles are hard to read and maintain and often use a lot of unnecessary storage! For example, you can reduce the size of a docker image (and reduce build time!) using a few ways:

 - Only installing the essential packages. What’s nice about containers is that they’re practically empty from the get-go - we have complete freedom to decide what we want.
 - Removing cached files (such as APT cache or documentation installed with tools). The code within a container will only be executed once (on build!), so we don’t need to store anything for later use.
 - Using minimal base operating systems in our `FROM` instruction. Even though operating systems for containers such as Ubuntu are already pretty slim, consider using an even more stripped-down version (i.e. `ubuntu:22.04-minimal`). Or, for example, using Alpine (which can be as small as 5.59MB!).
 - Minimising the number of layers - I’ll explain this further below.

Each instruction (I.E. `FROM`, `RUN`, etc.) is run in its own layer. Layers increase build time! The objective is to have as few layers as possible. For example, try chaining commands from RUN together like so:

__Before:__
```bash
FROM ubuntu:latest
RUN apt-get update -y
RUN apt-get upgrade -y
RUN apt-get install apache2 -y
RUN apt-get install net-tools -y
```

A terminal showing five layers of a Dockerfile being built
cmnatic@thm:~$ docker build -t before .

```bash
Step 2/5 : RUN apt-get update -y
 ---> Using cache
 ---> 446962612d20
Step 3/5 : RUN apt-get upgrade -y
 ---> Running in 8bed81c695f4
--omitted for brevity--
cmnatic@thm:~$
```

__After:__
```bash
FROM ubuntu:latest
RUN apt-get update -y && apt-get upgrade -y && apt-get install apache2 -y && apt-get install net-tools
```

```bash
cmnatic@thm:~$ docker build -t after .
Sending build context to Docker daemon   4.78MB
Step 1/2 : FROM ubuntu
 ---> 2dc39ba059dc
Step 2/2 : RUN apt-get update -y && apt-get upgrade -y && apt-get install apache2 -y && apt-get install net-tools
 ---> Running in a4d4943bcf04
--omitted for brevity--
cmnatic@thm:~$
```

Note here how there are now only two build steps (this will be two layers, making the build much quicker). This is just a tiny example of a Dockerfile, so the build time will not be so drastic, but in much larger Dockerfiles - reducing the number of layers will have a fantastic performance increase during the build.


## Questions and Answers:

__Q__ What instruction would we use to specify what base image the container should be using?
 - __A__ `FROM`

__Q__ What instruction would we use to tell the container to run a command?
 - __A__ `RUN`

__Q__ What docker command would we use to build an image using a Dockerfile?
 - __A__ `build`

__Q__ Let's say we want to name this image; what argument would we use?
 - __A__ `-t`
 

# Task 5 : Intro to Docker Compose

Let’s first understand what Docker Compose is and why it’s worth understanding. So far, we’ve only interacted with containers individually. Docker Compose, in summary, allows multiple containers (or applications) to interact with each other when needed while running in isolation from one another.

You may have noticed a problem with Docker so far. More often than not, applications require additional services to run, which we cannot do in a single container. For example, modern - dynamic - websites use services such as databases and a web server. For the sake of this task, we will consider each application as a “microservice”.

While we can spin up multiple containers or “microservices” individually and connect them, doing so one by one is cumbersome and inefficient. Docker Compose allows us to create these “microservices” as one singular “service”. 

This illustration shows how containers are deployed together using Docker Compose Vs. Docker:

![illustration docker and docker compose](/assets/img/002-Intro-to-Containerisation.png "illustration docker and docker compose")

Before we demonstrate Docker Compose, let’s cover the fundamentals of using Docker Compose.
 - We need Docker Compose installed (it does not come with Docker by default). Installing it is out of scope for this room, as it changes depending on your operating system and other factors. You can check out the installation documentation here.
 - We need a valid docker-compose.yml file - we will come onto this shortly.
 - A fundamental understanding of using Docker Compose to build and manage containers.

I have put some of the essential Docker Compose commands into the table below:

<table class="table table-bordered"><tbody><tr><td><b>Command</b></td><td><b>Explanation</b></td><td><b>Example</b></td></tr><tr><td>up</td><td>This command will (re)create/build and start the containers specified in the compose file.</td><td><p><code>docker-compose up</code></p></td></tr><tr><td>start</td><td>This command will start (but requires the containers already being built) the containers specified in the compose file.<br></td><td><p><code>docker-compose start</code><br></p></td></tr><tr><td>down</td><td>This command will stop and <b>delete</b>&nbsp;the containers specified in the compose file.<br></td><td><p><code>docker-compose down</code><br></p></td></tr><tr><td>stop</td><td>This command will stop (<b>not</b> delete) the containers specified in the compose file.<br></td><td><p><code>docker-compose stop</code><br></p></td></tr><tr><td>build</td><td>This command will build (but will not start) the containers specified in the compose file.<br></td><td><p><code>docker-compose build</code><br></p></td></tr></tbody></table>


### A Showcase of Docker Compose

With that said, let’s look into how we can use Docker Compose ourselves. In this scenario, I am going to assume the following requirements:

An E-commerce website running on Apache
This E-commerce website stores customer information in a MySQL database
Now, we could manually run the two containers via the following:

Creating the network between the two containers: `docker network create ecommerce`
Running the Apache2 webserver container: `docker run -p 80:80 --name webserver --net ecommerce webserver`
Running the MySQL Database server: `docker run --name database --net ecommerce webserver`

