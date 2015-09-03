---
layout: page
title: Getting Started With Spark

---

<center>![spark logo](/CSUEB-Data-Science/assets/sparkLogo.png)</center>

*In this article we take a look at a few first steps in working with Spark. We will introduce what spark is, how its
different from Hadoop. We then show an installation of Spark, and finish off with a few examples that will lead into
future articles*

<a name = "top"></a>
## Table of Contents

- [1. What is Spark?](#whatIsSpark")
- [2. Install JVM and Scala](#JVMandScala)
- [3. Installing Spark](#installSpark)
  - [3.1 Getting Spark](#gettingSpark)
  - [3.2 Configuring Spark ](#configureSpark)
- [4. Working with Spark](#workingWithSpark)
  - [4.1 Terminology and Basic Concepts](#terminology)
  - [4.2 Basic Statistics](#basicStats)
  - [4.3 Writing a Script Job](#script)


<a name = "whatIsSpark"></a>
##1. What is Spark?

Spark like Hadoop is a framework for working with Big Data, but unlike Hadoop, that operates on top of the Mapreduce paradigm, Spark works with in-memory primitives. This in turn yields performance that is much faster than that of Hadoop. This boost in performance is very important for programs and jobs that require the user to access data repeatedly, and thus well suited for machine learning tasks.

<a name = "JVMandScala"></a>
##2. Installing Scala and the JVM

The next thing we need to do is install Scala and JVM. To install JVM follow this little snippet from another guide (only do section 1.1 ): [Install JVM](http://ergz.github.io/CSUEB-Data-Science/2015/06/25/Install-A-Hadoop-Single-Node-Cluster.html#JVM)

Once that is done we install Scala.

First download Scala with the following command:

```bash
wget http://downloads.typesafe.com/scala/2.10.5/scala-2.10.5.tgz
```

we extract the this compressed file and move it to a proper directory with:

```bash
tar xvzf scala-2.10.5.tgz
# to move scala
sudo mv scala-2.10.5 /usr/local/share/scala
```

to finish the installtion we add a few lines to our bashrc file:

```
export SCALA_HOME=/usr/local/share/scala
export PATH=$PATH:$SCALA_HOME/bin
```

we update the modified bashrc file with `source ~/.bashrc`.

<a name = "installSpark"></a>
## 3. Installing Spark 

In this section we will go through a stand alone installation as well as one in which spark uses the HDFS we previously installed for its distributed file system.

<a name = "gettingSpark"></a>
### 3.1 Getting Spark and Setting Up 
In this article we will be using the Prebuilt version of Spark for Hadoop 2.6 or later. If you want spark to sit ontop of HDFS you will need to have hadoop 2.6 HDFS already on the machine you would like to install spark on. If you already don't have it you can do so by following this article: [Installing A Hadoop Single Node Cluster](http://ergz.github.io/CSUEB-Data-Science/2015/06/25/Install-A-Hadoop-Single-Node-Cluster.html).

We get a prebuilt version of Spark 1.4.1 [here](http://d3kbcqa49mib13.cloudfront.net/spark-1.4.1-bin-hadoop2.6.tgz), we can also download and extract via the command line with the following:

```bash
# to download directly
wget http://d3kbcqa49mib13.cloudfront.net/spark-1.4.1-bin-hadoop2.6.tgz
# to extract the compressed file
tar xvzf spark-1.4.1-bin-hadoop2.6.tgz
```

now we add the following to the bashrc file

```
export SPARK_HOME=/home/YourUserName/spark-1.4.1 # note here I am assuming spark was downloaded
# into your home folder, if this is not the case or you moved it, modify this accordingly

export PATH=$PATH:$SPARK_HOME/bin # to add spark's bin folder to env path
```

update the bashrc file with `source ~/.bashrc`. You should now be able to start and interactive spark interpreter with `spark-shell`.

<a name = "configureSpark"></a>
### 3.2 Configuring Spark 

When you first run the spark-shell you will most likely be prompted with a bunch of Warning and Info logs, we will change it such that we only see the ERRORS, which is what is important to us.

First we will change our working directory into the conf folder of spark, so `cd spark-1.4.1/conf`.

Now we will make a copy of the log4j.properties.template that we can edit,

```bash
cp log4j.properties.template  log4j.properties
```

now with your favorite text editor we will edit this newly created file and change the entry `log4j.rootCategory=INFO, console` to `log4j.rootCategory=ERROR, console`. Now restarting spark and spark-shell we should no longer see all log outputs we saw the first time around.

Next we want to set up Spark with the HDFS. If you want to use Spark standalone, then the work is done and you can skip this section. The configuration is quite straightforward.

First we want to make a copy of `spark-env.sh.template` that we can edit

```bash 
cp spark-env.sh.template  spark-env.sh
```

we now edit this file with our favorite text editor. Look for entry that says `# - HADOOP_CONF_DIR, to point Spark towards Hadoop configuration files.`
Right below this we want to add this variable, so we add:

```bash
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
```

once again this is assuming you followed the guide for hadoop, change accoridingly otherwise. Once that is set up we are done. Simply start HDFS with start-dfs.sh before accesing it through spark.

The installtion process is now complete, we now explore Spark with some simple examples.

<a name = "workingWithSpark"></a>
## 4. Working with Spark 

We jump into spark with the Machine Learning Library provided by Spark. 

<a name = "terminology"></a>
### 4.1 Terminology and Basic Concepts

The most central part of Spark are **RDD's (Resilient Distributive Datasets)**. RDD's allow us to work with large amounts of data and data sets in parallel. 

Spark ships with three different interactive shells, these include:

* pyspark: an interactive python shell 

* spark-shell: an interactive scala shell (Spark is written in Scala)

* sparkR: an interative R shell 

In this guide we will introduce the basic examples through the `pyspark` shell.   

<a name = "basicStats"></a>
### 4.2 Basic Statistics 

Lets explore the Statistics Library in the Machine Learning Library. First we start the `pyspark` shell with

```bash
$ pyspark
```

and import several modules

```python
import numpy as np
from pyspark.mllib.stat import Statistics
```

Lets now create some data to explore,


```python
# x1 a vector of values from nornal distribution with mu = 2.1 and sigma = .43, total 1000 values
x1 = np.random.normal(2.1, .43, 1000)
# error terms added for some noise
errorTerms = np.random.normal(0,.21,1000)
x2 = 3 * x1 + errorTerms
y = 3.2 * x1 + 1.1 * x2 + errorTerms

# aggregating to a single matrix
dataM = np.array([y, x1, x2])
# note the dimensions are not correct
dataM.shape # (3, 1000)
# we correct this with
dataM = np.transpose(dataM)
dataM.shape # (1000, 3)
```

at the moment this `dataM` matrix is a dense numpy matrix and thus cannot take any of the advantages that Spark provides. We convert it to a parallelized object with the following command.

```python
dataMPar = sc.parallelize(dataM)
```

We can now use functionality provided by Spark, namely we can obtain some descriptive statistics for each column of our data matrix.

```python
dataSumm = Statistics.colStats(dataMPar)

print dataSumm.mean() # [ 13.70850977   2.10785612   6.32711359]

print dataSumm.variance() # [ 8.45450811  0.19589769  1.80348414]
```

the functions available to dataSumm, `colStats` objects, are: `'call', 'count', 'max', 'mean', 'min', 'normL1', 'normL2', 'numNonzeros', 'variance'`, each of which yields a numpy array object.

<a name = "script"></a>
### 4.3 Writing A Script Job

Being able to use the shell is great for testing out code and exploring data on the fly, but we need a better way to write algorithms, and jobs
for spark. Here we introduce how to write a python script to work with spark, and submitting this to Spark for execution. 

To keep things simple we will simply implement the code we have written above into a python script. 

There are only a few changes that need to be made for this script file to work, namely we need to import the Spark Context, and Spark Configuration.
In the shell this is done automatically, so we did not have to worry about it. So we create a new file call it, `columnStats.py`, and add this to the top:

```python 
# to import the modules
from pyspark import SparkContext, SparkConf
```

next we need to add a name for the application and create the `sc` so it can be used as above.

```python 
conf = SparkConf().setAppName("columnStats")
sc = SparkContext(conf = conf)
```

once this is dont the we can implement code very much the same way we did in the shell. Lets create a program that created random data, parallelizes it and lastly outputs some descriptive statistics on it. As a whole the script will look like this:

```python 

from pyspark import SparkContext, SparkConf
from pyspark.mllib.stat import Statistics
import numpy as np

conf = SparkConf().setAppName("columnStats")
sc = SparkContext(conf = conf)

# create some data 
x1 = np.random.normal(.11, .03, 1000)
x2 = 1.45*x1 + np.random.normal(0, .12, 1000)
y = 1.1*x1 + 6.7*x2 + np.random.normal(0, .02, 1000)

# create the array 
dataMatrix = np.transpose(np.array([y, x1, x2]))

# parallelize the data set 
dataMatrixP = sc.parallelize(dataMatrix)

# create the data Summary
dataSummary = Statistics.colStats(dataMatrixP)

# print out the values 
print dataSummary.mean()
print dataSummary.variance()
```

now we simply submit the script to spark. 

To do this we go onto the Terminal, `cd` into the directory containing the script and do the following:

```bash 
$ spark-submit --master local[*] columnStats.py
```

Here we are calling onto the program `spark-submit` (*note that this is assuming the bin directory of spark was added to .bashrc, otherwise a path to the spark-submit file is needed*) and giving several commands.

First `--master` indicates where the master node of cluster is, since we are running a local cluster we pass the `local[*]` parameter.

The `[*]` option in `local` indicates how many virtual cores to use. `*` indicates to use all possible cores, other option could have been 
`2`, `4` and so on depending on the machine.

Lastly we indicate what script we want to run, in our case `columnStats.py`, and thats it, we should see the output of the mean and variance of 
each of the columns.





















































