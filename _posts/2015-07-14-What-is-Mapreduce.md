---
layout: page
title: What is Mapreduce?
---

<a name = "top"></a>

- [1. Introduction](#intro)
- [2. A Simple Example](#ex1)

<a name = "intro"></a>
#1. Introduction 

In order to understand what is going on when we use Hadoop and more importantly when we write and code up jobs to implement into Hadoop
it is important to grasp the concept of Mapreduce. Mapreduce is a programming paradigm that lies at the heart of Hadoop, it might be a
little complicated at first but  we will introduce the concept with a simple example to show its underlying concept and elaborate on top of that.

<a name = "ex1"></a>
#2. A Simple Example

Let us now present a trivial example that will demonstrate the underlying procedure of the Mapreduce paradigm. Consider a class with 10 students,
each with a final exam score, the data would look something like this:

```
Sam, 89
Anne, 77
Robert, 81
James, 96
Derick, 71
Rachel, 97
Emanuel, 67
David, 88
Karen, 88
Matt, 84
```

In Hadoop terminology the dataset is set up as key-value pair, `<name,score>`.
