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


## 3. Writting a Mapreduce Job

The task in this section is to use R for implementing a Mapreduce job in Hadoop. Although Hadoop and all of its libraries are written in Java, 
