---
layout: page
title: What is Mapreduce?
---

<a name = "top"></a>
## Table of Contents

- [1. Introduction](#intro)
- [2. A Simple Example](#ex1)
  - [2.1 The Mapper](#mapper)
  - [2.2 The Reducer](#reducer)
- [3. Writting A Mapreduce Job](#mapred)
  - [3.1 The Hello World Program](#hello)
  - [3.2 The Word Count Mapper](#hellomap)
  - [3.3 The Word Count Reducer](#helloreducer)

<a name = "intro"></a>
##1. Introduction 

In order to understand what is going on when we use Hadoop and more importantly when we write and code up jobs to implement into Hadoop
it is important to grasp the concept of Mapreduce. Mapreduce is a programming paradigm that lies at the heart of Hadoop, it might be a
little complicated at first but  we will introduce the concept with a simple example to show its underlying concept and elaborate on top of that.

<a name = "ex1"></a>
##2. A Simple Example

Let us now present a trivial example that will demonstrate the underlying procedure of the Mapreduce paradigm. Consider a dataset where the top seven scores
on exams in core classes are recorded along with the student who got the score (one dataset file per core class), also suppose there are five core classes.
So in total we have five files to look at, one given file may look like this:


Sam, 89<br />
Anne, 92<br />
Sam, 91<br />
James, 96<br />
Rachel, 97<br />
Emanuel, 95<br />
Rachel, 100

*note that the same student may appear more than once in a file if they have two test scores in the top 7*

In Hadoop terminology the dataset is set up as key-value pair, `<name,score>`, this is the way the Mapreduce paradigm will always read in data. 
In reality our dataset will not be this clean and certainly not this small and compact, in real life applications we would hope to see millions
of data points, the important thing to note here is that the Mapreduce paradigm is scalable thus the key concepts covered here remain true no matter the
dimensionality of the dataset.

Let suppose our goal for this job is to find the highest score per student of the students represented in our files.

<a name = "mapper"></a>
### 2.1 The Mapper

The Mapper part of Mapreduce will work on one file and one file only. This allows us to use the Mapper in parallel to all the files at once. In our case suppose
we have one mapper per file, so we have five mapper jobs running in parallel. What the mapper will do is calculate the highest score of each student, and return this
`<key,value>` pair. Looking at the file above the mapper that acts on this file would return:

(Anne, 92), (Emanuel, 95), (James, 96), (Rachel, 100), (Sam, 91)

this same procedure is carried out on the other four files, and would return something similar. It is important to note that part of the Mapper job, involves sorting the `key`
part of the set of pairs, this is shown in the above output. So in total once the Mapper jobs are complete we will have 5 sets of `<key,value>` pairs depciting the highest score
for each student in each of the 5 files.

<a name = "reducer"></a>
### 2.2 The Reducer

The Reducer will take as input whatever the Mapper has calculated. In our case the input is the set of student-score pairs. What the reducer will do is exactly
what its name would imply namely it takes this set of pairs and *reduces* it to a single list of pairs. So a possible output of the reducer, and Mapreduce as a whole,  could be,

(Anne, 96), (Emanuel, 95), (Enrique, 99), (James, 96), (Rachel, 100), (Sam, 92)

*note that Enrique was not in the displayed file, thus he must have appeared somewhere else in the corpus*

<a name = "mapred"></a>
## 3. Writting a Mapreduce Job

The task in this section is to use R for implementing a Mapreduce job in Hadoop. Although Hadoop and all of its libraries are written in Java, one
is able to **_stream_** jobs into the Hadoop framework with an API that was built into the libraries of Hadoop. We will show how to do this at the end
of this section. 

<a name = "hello"></a>
### 3.1 The Hello World Program

Learning a programming language always begins with writting a program that will print "Hello World!" to the screen, its a simple way to introduce
users to a new enviroment. In the same fashion we will implement the Hello World program equivalent in Hadoop, a Word Count program. The task is to
take some text or set of texts and apply the Mapreduce paradigm to get a word count.

<a name = "hellomap"></a>
### 3.2 The Word Count Mapper

Let us develop the mapper for the word count. First things first we need to able to read in input such as a txt file. In R there is a built in
function called `file` that reads input from standard in. We will use this read in the source for which the word count will be implemented.

We use the `file` function as follows:

```R
textFile <- file("stdin", open = "r")
```

the keyword `stdin` means we can specify what file to work with at the command line and other OS independent methods.

Next we implement the rest of the mapper through a `while` loop that will read in input as long as there is a next line in the file.

```R
while (length(line <- readLines(textFile, n = 1, warn = FALSE)) > 0) {
	# first we trim white space at the beginning and end
	line <- gsub("(^ +)|( +$)", "", line)
	# now we split each word in the line
    words <- unlist(strsplit(line, "[[:space:]]+"))

	# we output the the results with the cat command
    for (w in words)
        cat(w, "\t1\n", sep="")
}

```

As a whole the mapper will look like this, also want to save this file as `WC_mapper.R`

```R
#################################################
# Mapper Function for Word Count
# in Hadoop
################################################

textFile <- file("stdin", open = "r")

while (length(line <- readLines(textFile, n = 1, warn = FALSE)) > 0) {
	line <- gsub("(^ +)|( +$)", "", line)
    words <- unlist(strsplit(line, "[[:space:]]+"))

    for (w in words)
        cat(w, "\t1\n", sep="")
}
 
 
close(textFile)
```

now since we are working with std i/o means that we don't need Hadoop to test our code let us do the following
in the command line:

```bash
# note the whitespace at beginning and end
# to emphasize the trimming that takes place in the while loop
$ echo " these are some words " | Rscript WC_mapper.R

#Output
these	1
are	    1
some	1
words	1
```

<a name = "helloreducer"></a>
### 3.3 The Word Count Reducer

Now we create another R script for implementing the reducing portion of Mapreduce

What we want to do first is create a function that will split the line we are on and assign the word to a `word` variable
and the value to a `count` varaible

```R
splitLine <- function(line) {
    val <- unlist(strsplit(line, "\t"))
    list(word = val[1], count = as.integer(val[2]))
}
```

Next we want to create an `enviroment` in R so keep track of our counts. 
