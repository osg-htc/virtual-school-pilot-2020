---
status: in progress
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Software Exercise 1 (Required): Using a Pre-compiled Binary
===================================================

In this exercise, we will run a job using a downloaded, pre-compiled binary. This exercise should take 10-15 minutes.

Background
----------

This is the simplest scenario for using a particular software program on the Open Science Grid - downloading a pre-compiled binary and using it to run jobs. 

Our Software Example
--------------------

The software we will be using for this example is a common tool for aligning genome and protein sequences against a
reference database, the BLAST program.

1.  Search the internet for the BLAST software.  Searches might include "blast executable or "download blast software".  Hopefully these searches will lead you to a BLAST website page that looks like this:

    ![BLAST landing page][blast-page]

1.  Click on the title that says "Download BLAST" and then look for the link that has the latest installation and source code.  You should end up on a page with a list of each version of BLAST that is available for different operating systems.

1.  We could download the source and compile it ourselves, but instead, we're going to use one of the pre-built binaries.  Before proceeding, look at the list of downloads and try to determine which one you want. 

1.  Based on our operating system, we want to use the Linux binary, which is labelled with the `x64-linux` suffix. 

	![BLAST downloads][blast-downloads]

	All the other links are either for source code or other operating systems. 
	
1. While logged into `login05.osgconnect.net`, create a directory for this exercise. Then download the appropriate `tar.gz` file and un-tar it. You can download the file directly from the BLAST website using `wget` or download our local copy with the command below: 

        :::console
        user@login $ wget http://proxy.chtc.wisc.edu/SQUID/osgschool19/ncbi-blast-2.10.1+-x64-linux.tar.gz
        user@login $ tar -xzf ncbi-blast-2.10.1+-x64-linux.tar.gz

1.  We're going to be using the `blastx` binary in our job. Where is it in the directory you just downloaded?

Copy the Input Files
--------------------

To run BLAST, we need an input file and reference database. For this example, we'll use the "pdbaa" database, which contains sequences for the protein structure from the Protein Data Bank. For our input file, we'll use an abbreviated fasta file with mouse genome information.

1.  Download these files to your current directory: 

        :::console
        username@login $ wget http://proxy.chtc.wisc.edu/SQUID/osgschool20/pdbaa.tar.gz
        username@login $ wget http://proxy.chtc.wisc.edu/SQUID/osgschool20/mouse.fa

1.  Untar the `pdbaa` database: 

        :::console
        username@login $ tar -xzf pdbaa.tar.gz


Submitting the Job
------------------

We now have our program (the pre-compiled `blastx` binary) and our input files, so all that remains is to create the submit file. The form of a typical `blastx` command looks something like this:

```file
blastx -db <database_dir/prefix> -query <input_file> -out <output_file>
```

1.   Copy the submit file from **TBD**

1. Think about which lines you will need to change or add to your submit file in order to submit the job successfully. In particular:    
	-   What is the executable?
	-   How can you indicate the entire command line sequence above?
	-   Which files need to be transferred in addition to the executable?
	-   Does this job require a certain type of operating system?
	-   Do you have any idea how much memory or disk to request?

1. Try to answer these questions and modify your submit file appropriately.

1. Once you have done all you can, check your submit file against the lines below, which contain the exact components to run this particular job.

    * The executable is `blastx`, which is located in the `bin` directory of our downloaded BLAST directory. We need to use the `arguments` line in the submit file to express the rest of the command. 
    
            :::file
            executable = ncbi-blast-2.10.1+/bin/blastx
            arguments = -db pdbaa/pdbaa -query mouse.fa -out results.txt

    * The BLAST program requires our input file and database, so they must be transferred with `transfer_input_files`. 
    
            :::file
            transfer_input_files = pdbaa, mouse.fa
	    
    * Let's assume that we've run this program before, and we know that 1GB of disk and 1GB of memory will be MORE than enough (the 'log' file will tell us how accurate we are, after the job runs): 
    
            :::file
            request_memory = 1GB
	    	request_disk = 1GB

    * Because we downloaded a Linux-specific binary, we need to request machines that are running Linux. 
    
            :::file
            requirements = (OSGVO_OS_STRING == "RHEL 7")

1. Submit the blast job using `condor_submit`. Once the job starts, it should run in just a few minutes and produce a file called `results.txt`.

[blast-page]: /materials/day2/files/osgus19-day2-part3-ex2-blast-src-page.png
[blast-downloads]: /materials/day2/files/osgus19-day2-part3-ex2-blast-src-page.png