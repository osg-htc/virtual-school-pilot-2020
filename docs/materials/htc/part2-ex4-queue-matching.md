---
status: testing
---

<style type="text/css"> pre em { font-style: normal; background-color: yellow; } pre strong { font-style: normal; font-weight: bold; color: \#008; } </style>

Bonus HTC Exercise 2.4: Submit With “queue matching”
=================================================

The goal of this exercise is to submit many jobs from a single submit file by using the `queue ... matching` syntax to submit jobs with variable values derived from files in the current directory which match a specified pattern.

Counting Words in Files
-----------------------

Returning to our book word-counting example, let's pretend that instead of
three books, we have an entire library. While we could list all of the text
files in a `books.txt` file and use `queue book from books.txt`, it could be a
tedious process, especially for tens of thousands of files. Luckily HTCondor
provides a mechanism for submitting jobs based on pattern-matched files.

Queue Jobs By Matching Filenames
--------------------------------

This is an example of a common scenario: We want to run one job per file, where the filenames match a certain consistent pattern. The `queue ... matching` statement is made for this scenario.

Let’s see this in action. First, here is a new version of the script (note, we removed the 'top n words' restriction):

``` python
#!/usr/bin/env python

import os
import sys
import operator

if len(sys.argv) != 2:
    print 'Usage: %s DATA' % (os.path.basename(sys.argv[0]))
    sys.exit(1)
input_filename = sys.argv[1]

words = {}

my_file = open(input_filename, 'r')
for line in my_file:
    line_words = line.split()
    for word in line_words:
        if word in words:
            words[word] += 1
        else:
            words[word] = 1
my_file.close()

sorted_words = sorted(words.items(), key=operator.itemgetter(1))
for word in sorted_words:
    print '%s %8d' % (word[0], word[1])
```

To use the script:

1.  Save it as `wordcount.py`.
1.  Verify the script by running it on one book manually.
1.  Create a new submit file to submit one job (pick a book file and model your submit file off of the one above), including memory and disk requests of 20 MB; submit it, if you like.
1.  Modify the following submit file statements to work for all books:

        :::text
        transfer_input_files = $(book) 
        arguments = $(book) 
        output = $(book).out 
        error = $(book).err 
        queue book matching *.txt

    !!!note
        As always, the order of statements in a submit file does not matter, except that the `queue` statement should be last. Also note that any submit file variable name (here, `book`, but true for `process` and all others) may be used in any mixture of upper- and lowercase letters.

1.  Submit the jobs.

HTCondor uses the `queue ... matching` statement to look for files in the submit directory that match the given pattern, then queues one job per match. For each job, the given variable (e.g., `book` here) is assigned the name of the matching file, so that it can be used in `output`, `error`, and other statements.

The result is the same as if we had written out a much longer submit file:

``` file
...

transfer_input_files = AAiW.txt
arguments = "AAiW.txt"
output = AAiW.txt.out
error = AAiW.txt.err
queue

transfer_input_files = PandP.txt
arguments = "PandP.txt"
output = PandP.txt.out
error = PandP.txt.err
queue

transfer_input_files = TAoSH.txt
arguments = "TAoSH.txt"
output = TAoSH.txt.out
error = TAoSH.txt.err
queue

...
```

How many jobs were created? Is this what you expected? If you ran this in the
same directory as Exercise 2.3, you may have noticed that a job was submitted
for the `books_n.txt` file that holds the variable values in the `queue from`
statement. Beware the dangers of matching more files than intended! One
solution would be to put all of the books into an `input` directory and `queue
matching input/*.txt`. 


Here is some example `condor_q -nobatch` output:

``` console
 ID      OWNER            SUBMITTED     RUN_TIME ST PRI SIZE CMD
  89.0   iaross          7/17 11:41   0+00:00:00 I  0    0.0 wordcount.py AAiW.txt
  89.1   iaross          7/17 11:41   0+00:00:00 I  0    0.0 wordcount.py PandP.txt
  89.2   iaross          7/17 11:41   0+00:00:00 I  0    0.0 wordcount.py TAoSH.txt
```

All three jobs were part of cluster 89. The first filename that was matched in the queue statement resulted in a process ID of 0, the second match has a process ID of 1, and the third has a process ID of 2.

When the three jobs finish, carefully look at the resulting files. Do they match your expectations? There should be a single log file, but three separate output files and three separate (and hopefully empty) error files, one for each job.

Extra Challenge 1
-----------------

In the example above, you used a single log file for all three jobs. HTCondor handles this situation with no problem; each job writes its events into the log file without getting in the way of other events and other jobs. But as you may have seen, it may be difficult for a person to understand the events for any particular job in the combined log file.

Create a new submit file that works just like the one above, except that each job writes its own log file.

Extra Challenge 2
-----------------

Between this exercise and the previous one, you have explored two of the three primary `queue` statements. How would you use the `queue in ... list` statement to accomplish the same thing(s) as one or both of the exercises?

