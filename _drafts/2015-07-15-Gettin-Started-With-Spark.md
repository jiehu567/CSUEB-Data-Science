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
In this article we will be using the Prebuilt version of Spark for Hadoop 2.6 or later. If you do not have the hadoop 2.6 HDFS already on the machine you would like to install spark on, you can do so by following this article: [Installing A Hadoop Single Node Cluster](http://ergz.github.io/CSUEB-Data-Science/2015/06/25/Install-A-Hadoop-Single-Node-Cluster.html).

We get a prebuilt version of Spark 1.4.1 [here](http://d3kbcqa49mib13.cloudfront.net/spark-1.4.1.tgz), we can also download and extract via the command line with the following:

```bash
# to download directly
wget http://d3kbcqa49mib13.cloudfront.net/spark-1.4.1.tgz
# to extract the compressed file
tar xvzf spark-1.4.1.tgz
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



