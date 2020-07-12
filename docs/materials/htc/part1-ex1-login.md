---
status: testing
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

HTC Exercise 1.1: Log In and Look Around
===========================================

The goal of this first exercise is simply to log in to the local submit server and look around a little bit, which will take only a few minutes. 

**If you have trouble getting SSH access to the submit server, ask the instructors right away! Gaining access is critical for all remaining exercises.**

Logging In
----------

Today, you will use a submit server named `learn.chtc.wisc.edu`, which will allow you to submit jobs to our local HTCondor pool in CHTC.

To log in, use a [Secure Shell](http://en.wikipedia.org/wiki/Secure_Shell) (SSH) client.

-   From a Mac or Linux computer, run the Terminal app and use the `ssh` command, like so:

        :::console
        username@learn $ ssh %RED%<USERNAME>%ENDCOLOR%@learn.chtc.wisc.edu

-   On Windows, we recommend a free client called [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/),
    but any SSH client should be fine.

**If you need help finding or using an SSH client, ask the instructors for help right away**!

### About Your Password

-   Your mentor should have given you your username and password.
    If this is not true or you have lost the password, reach out to any staff member for help.

-   While the `passwd` command will work (and will change your password temporarily),
    your initial password will be automatically reset for you on an hourly basis.
    So consider not changing your password and save the one provided in a secure place.

Running Commands
----------------

In the exercises, we will show commands that you are supposed to type or copy into the command line, like this:

``` console
username@learn $ hostname
learn.chtc.wisc.edu
```

!!! note
    In the first line of the example above, the `username@learn $` part is meant to show the Linux command-line prompt.
    You do not type this part! Further, your actual prompt probably is a bit different, and that is expected.
    So in the example above, the command that you type at your own prompt is just the eight characters `hostname`.
    The second line of the example, without the prompt, shows the output of the command; you do not type this part,
    either.

Here are a few other commands that you can try (the examples below do not show the output from each command):

``` console
username@learn $ whoami
username@learn $ date
username@learn $ uname -a
```

A suggestion for the day: Try typing into the command line as many of the commands as you can. Copy-and-paste is fine, of course, but **you WILL learn more if you take the time to type each command, yourself.**

Organizing Your Workspace
-------------------------

You will be doing many different exercises over the next few days, many of them on this submit server. Each exercise may use many files, once finished. To avoid confusion, it may be useful to create a separate directory for each exercise.

For instance, for the rest of this exercise, you may wish to create and use a directory named `intro-1.1-login`, or something like that.

``` console
username@learn $ mkdir intro-1.1-login
username@learn $ cd intro-1.1-login
```

Showing the Version of HTCondor
-------------------------------

HTCondor is installed on this server. But what version? You can ask HTCondor itself:

``` console
username@learn $ condor_version
$CondorVersion: 8.9.8 Jun 29 2020 BuildID: 508520 PackageID: 8.9.8-0.508520 $
$CondorPlatform: x86_64_CentOS7 $
```

As you can see from the output, we are using HTCondor 8.9.8.

### FYI: Background information about HTCondor version numbers

HTCondor always has two types of releases at one time: stable and development. HTCondor 8.6.x and 8.8.x are considered stable releases, indicated by even-numbered second digits (e.g., 6 or 8 in these cases). Within one stable series, all versions have the same features (for example 8.6.0 and 8.6.8 have the same set of features) and differ only in bug and security fixes.

HTCondor 8.9.8 is the latest _development_ release series of HTCondor. You know that these are a development release because the second digit (i.e., 9) is an odd number. CHTC is usually running the latest development series as the local CHTC Pool is somewhat of a final testing ground for new features. Other HTCondor pools and submit servers that you use outside of CHTC (including the OSG submit server you'll use later) may run different versions. In general, the user-facing HTCondor features in 8.6 forward are mostly the same, but you may see some differences in the format of output from `condor_` commands or in more advanced or non-user features.

Reference Materials
-------------------

Here are a few links to reference materials that might be interesting after the school (or perhaps during).

-   [HTCondor home page](http://research.cs.wisc.edu/htcondor/)
-   [HTCondor manuals](https://htcondor.readthedocs.io/en/latest/); it is probably best to read the manual corresponding to the version of HTCondor that you use. That link points to the latest version of the manual, but you can switch versions using the toggle in the lower left corner of that page.
-   [Center for High Throughput Computing](http://chtc.cs.wisc.edu/), our campus research computing center, and home to HTCondor and other development of distributed computing tools.
