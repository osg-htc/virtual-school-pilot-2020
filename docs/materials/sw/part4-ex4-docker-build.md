---
status: in progress
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: #008; } </style>

Software Exercise 4.4: Building Your Own Docker Container (Beta!)
====================================

This exercise will walk you through the steps to build your own Docker container 
based on Python, with the `numpy` Python library added on. 

Sample Script
-------------------

For this example, create a script called `rand_array.py` on `learn.chtc.wisc.edu`:

	:::file
	import numpy as np

	#numpy array with random values
	a = np.random.rand(4,2,3)

	print(a)

To run this script, we will need a copy of Python with the `numpy` library. 

Getting Set Up
--------------

Before building your own Docker container, you need to go through the following 
set up steps: 

1. Install Docker Dekstop on your computer. 
	* [Docker Desktop page](https://www.docker.com/products/docker-desktop)

2. You may need to create a Docker Hub user name to download Docker Desktop; if not 
created at that step, create a user name for [Docker Hub](https://hub.docker.com/) now.

3. (Optional): Once Docker is up and running on your computer, you are welcome to take 
some time to explore the basics of downloading and running a container, as shown in 
the initial sections of this Docker lesson:
	* [Introduction to Docker](https://christinalk.github.io/docker-introduction/)
	However, this isn't strictly necessary for building your own container. 

Building a Container
--------------------

In order to make our container reproducible, we will be using Docker's capability 
to build a container image from a specification file.  

1. First, create an empty build directory on **your computer**, not the CHTC or OSG submit 
servers. 

2. In the build directory, create a file called `Dockerfile` (no file extension!) with 
the following contents: 

		:::file
		# Start with this image as a "base".
		# It's as if all the commands that created that image were inserted here.
		# Always use a specific tag like "4.7.12", never "latest"!
		FROM continuumio/miniconda:4.7.12

		# Use RUN to execute commands inside the image as it is being built up.
		RUN conda install --yes numpy

		# RUN multiple commands together.
		# Try to always "clean up" after yourself to reduce the final size of your image.
		RUN apt-get update \
		 && apt-get --yes install --no-install-recommends graphviz\
		 && apt-get --yes clean \
		 && rm -rf /var/lib/apt/lists/*

	This is our specification file and provides Docker with the information it needs 
	to build our new container. There are other options besides `FROM` and `RUN`; see 
	the [Docker documentation](https://docs.docker.com/engine/reference/builder/) for more information. 

3. Note that our container is starting from an existing container 
`continuumio/miniconda:4.7.12`. This container is produced by the `continuumio` 
organization; the number `4.7.12` indicates the container version. When we create our 
new container, we will want to use a similar naming scheme of: 

		USERNMAE/CONTAINER:VERSIONTAG

	In what follows, you will want to replace `USERNAME` with your DockerHub user name. 
	The `CONTAINER` name and `VERSIONTAG` are your choice; in what follows, we will 
	use `py3-numpy` as the container name and `2020-07` as the version tag. 

4. To build and name the new container, open a command line window on your computer 
where you can run Docker commands. Use the `cd` command to change your working directory 
to the build directory with the `Dockerfile` inside. 

		:::console
		$ docker build -t USERNAME/py3-numpy:2020-07 .
		
	Note the `.` at the end of the command! This indicates that we're using the current 
	directory as our build environment, including the `Dockerfile` inside. 

Upload Container and Submit Job
-------------------------------

Right now the container image only exists on your computer. To use it in CHTC or 
elsewhere, it needs to be added to a public registry like Docker Hub. 

1. To put your container image in Docker Hub, use the `docker push` command on the 
command line:

		:::console
		$ docker push USERNAME/py3-numpy:2020-07

2. Once your container image is in DockerHub, you can use it in jobs as described 
in [Exercise 4.3][/materials/sw/part4-ex3-docker]. 

> Thanks to [Josh Karpel](https://github.com/JoshKarpel/osg-school-example-dockerfile) for 
providing the sample `Dockerfile`!