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
