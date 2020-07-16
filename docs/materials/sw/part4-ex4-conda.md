---
status: in progress
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: #008; } </style>

Software Exercise 4.5: Using Conda Environments
====================================

This exercise covers how to use Python environments managed by miniconda. 

Introduction
------------

Many Python users manage their Python installation and environments with either the
either the `Anaconda` or `miniconda` distributions. These distribution tools are great 
for creating portable Python installations but can run 


Sample Script
-------------------

For this example, create a script called `rand_array.py`

	:::file
	import numpy as np

	#numpy array with random values
	a = np.random.rand(4,2,3)

	print(a)

Instructions
------------------

(For a generic version of these instructions, see the [CHTC User Guide](http://chtc.cs.wisc.edu/conda-installation))

## 1. Create a Miniconda installation

On the submit server, 
download the latest Linux [miniconda installer](https://docs.conda.io/en/latest/miniconda.html) and run it. 

```
[alice@submit]$ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
[alice@submit]$ sh Miniconda3-latest-Linux-x86_64.sh
```
{: .term}

Accept the license agreement and default options. At the end, you can choose whether or 
not to "initialize Miniconda3 by running conda init?" The default is no; you would 
then run the `eval` command listed by the installer to "activate" Miniconda. If you
choose "no" you'll want to save this command so that you can reactivate the 
Miniconda installation when needed in the future.

## 2. Create a conda "environment" with your software

> (If you are using an `environment.yml` file as described 
> [later](#specifying-exact-dependency-versions), you should instead create
> the environment from your `environment.yml` file. If you don’t have an 
> `environment.yml` file to work with, follow the install instructions in this
> section. We recommend switching to the `environment.yml` method of creating 
> environments once you understand the "manual" method presented here.)

Make sure that you've activated the base Miniconda environment if you haven't 
already. Your prompt should look like this: 

```
(base)[alice@submit]$ 
```
{: .term}

To create an environment, use the `conda create` command and then activate the 
environment: 

```
(base)[alice@submit]$ conda create -n env-name
(base)[alice@submit]$ conda activate env-name
```
{: .term}


Then, run the `conda install` command to install the different packages and 
software you want to include in the installation. How this should look is often 
listed in the installation examples for software 
(e.g. [Qiime2](https://docs.qiime2.org/2020.2/install/native/#install-qiime-2-within-a-conda-environment), 
[Pytorch](https://pytorch.org/get-started/locally/)). 

```
(env-name)[alice@submit]$ conda install pkg1 pkg2
```
{: .term}

Packages may also be installed via `pip`, but you should only do this
when there is no `conda` package available.

Once everything is installed, deactivate the environment to go back to the 
Miniconda "base" environment.

```
(env-name)[alice@submit]$ conda deactivate
```
{: .term}

For example, if you wanted to create an installation with `pandas` and 
`matplotlib` and call the environment `py-data-sci`, you would use this sequence 
of commands: 

```
(base)[alice@submit]$ conda create -n py-data-sci
(base)[alice@submit]$ conda activate py-data-sci
(py-data-sci)[alice@submit]$ conda install pandas matplotlib
(py-data-sci)[alice@submit]$ conda deactivate
(base)[alice@submit]$ 
```
{: .term}

> ## More about Miniconda
> 
> See the [official conda documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) for 
> more information on creating and managing environments with `conda`. 

## 3. Create Software Package

Make sure that your job's Miniconda environment is created, but deactivated, so 
that you're in the "base" Miniconda environment: 

```
(base)[alice@submit]$ 
```
{: .term}

Then, run this command to install the `conda pack` tool: 

```
(base)[alice@submit]$ conda install -c conda-forge conda-pack
```
{: .term}

Enter `y` when it asks you to install. 

Finally, use `conda pack` to create a zipped tar.gz file of your environment
(substitute the name of your conda environment where you see `env-name`): 

```
(base)[alice@submit]$ conda pack -n env-name
```
{: .term}

When this step finishes, you should see a file in your current directory named
`env-name.tar.gz`

## 4. Create a Job Executable

The job will need to go through a few steps to use this "packed" conda environment; 
first, setting the `PATH`, then unzipping the environment, then activating it, 
and finally running whatever program you like. The script below is an example 
of what is needed (customize as indicated to match your choices above).

```
#!/bin/bash

set -e

# replace env-name on the right hand side of this line with the name of your conda environment
ENVNAME=env-name
# if you need the environment directory to be named something other than the environment name, change this line
ENVDIR=$ENVNAME

# these lines handle setting up the environment; you shouldn't have to modify them
export PATH
mkdir $ENVDIR
tar -xzf $ENVNAME.tar.gz -C $ENVDIR
. $ENVDIR/bin/activate

# modify this line to run your desired Python script and any other work you need to do
python3 hello.py
```
{: .file}

## 5. Submit Jobs

In your submit file, make sure to have the following: 

- Your executable should be the the bash script you created in [step 4](#4-create-a-job-executable).
- Remember to transfer your Python script and the environment `tar.gz` file via
 `transfer_input_files`. 
  Since the `tar.gz` file will almost certainly be larger than 100MB, 
  please [email](mailto:chtc@cs.wisc.edu) us about different tools for 
  delivering the installation to your jobs, 
  likely our [SQUID web proxy](http://chtc.cs.wisc.edu/file-avail-squid.shtml).

