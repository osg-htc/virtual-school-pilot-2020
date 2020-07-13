---
status: in progress
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Software Exercise 3.1: Use Singularity from OSG Connect
============================================================

Background
----------

Containers are another way to manage software installations. We don't have the time to go fully into the details of building and using containers, but can use pre-existing containers to run jobs. 

One caveat for using containers: not all systems will support them. HTCondor has built-in features for using Docker and many Open Science Grid resources have Singularity installed, but they are not always available everywhere. 

Setup
-----

Make sure you are logged into `login05.osgconnect.net` (the OSG Connect submit server for this workshop).  For this exercise we will be using Singularity containers that are hosted by OSG Connect, in a very similar way to the software modules. 

To get an idea on what container images are available on the OSG, take a look at the directory path `/cvmfs/singularity.opensciencegrid.org/opensciencegrid`.  

Job Submission
--------------

For this job, we will use the OSG Connect Ubuntu "Xenial" image. Copy a submit file you used for a previous exercise on OSG Connect and add the following lines: 

	:::file
	requirements = HAS_SINGULARITY == true
	+SingularityImage = "/cvmfs/singularity.opensciencegrid.org/opensciencegrid/osgvo-ubuntu-xenial:latest"

If you had other requirements in the submit file, remove them. These options will do two things: 

* require that your job runs on servers that have Singularity installed and can access the OSG Connect repository of Singularity containers
* tells the job which Singularity container to use

To test and see if our job is really running in Ubuntu, use this script for the job's executable: 

	:::bash
	#!/bin/bash
	
	hostname
	lsb_release -a

Submit the job and look at the output file. 

