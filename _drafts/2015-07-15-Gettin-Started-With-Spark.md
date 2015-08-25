---
layout: page
title: Getting Started With Spark

---

<center>![spark logo](/CSUEB-Data-Science/assets/sparkLogo.png)</center>

*In this article we take a look at a few first steps in wotking with Spark. We will introduce what spark is, how its
different from Hadoop. We then show an installtion of Spark, and finish off with a few examples that will lead into
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
  - [4.3 Simple Linear Regression](#linearRegression)
        - [Setting Up Data](#dataSetup)
    	- [Labeled Point](#labeledPoint)

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


<a name = "linearRegression"></a>
### 4.3 Simple Linear Regression for Machine Learning

<a name = "dataSetup"></a>
#### Setting up the Data

We will use the data produced in the previous section, and carry on with the same variable names.

First let us split the data matrix, to a training and test set. Note that this split must done on the original `dataM` matrix.

```python
dataTrain = dataM[0:899,:]
dataTest = dataM[900:1000,:]
```

and now to parallelize the matrices

```python
dataTrnP = sc.parallelize(dataTrain)
dataTstP = sc.parallelize(dataTest)
```

The goal is to train the Linear Regression Model using the training set. In this examples we will be using an Ordinary Least Squres (OLS) approach to train
the parameters in the algorithm.

<a name = "labeledPoint"></a>
#### Labeled Points

In order to do linear regression in Spark we introduce the `LabeledPoint` data type. A `LabeledPoint` data type is a vector that has with it a *label* or *response* associated
with it. So we need to convert the rows of the training set, `dataTrnP` to `LabeledPoint`s. We can do this in two ways, we show both, it will also allows us to see the best way to pass function to Spark.

**The Function Approach**

```python
from pyspark.mllib.regression import LabeledPoint

def parsePoint(line):
    values = [x for x in line]
	return LabeledPoint(values[0], values[1:])
	
# we then call on this function
labeledData = dataTrnP.map(parsePoint)
```

**The Lambda Approach**

If you are not familiar with **lambda** expression in python, they are simply "anonymous" functions or "throw away" functions.

```python
from pyspark.mllib.regression import LabeledPoint

labeledData = dataTrnP.map(lambda p: LabeledPoint(p[0], p[1:]))
```

both will yield the same result, but it is preffered by Spark, to if possible express functions with a lambda expression. 


#### Regression

We are now able to train our model with the training data set.

```python
from pyspark.mllib.regression import LinearRegressionModel


lrm = LinearRegressionWithSGD.train(labeledData)
```

