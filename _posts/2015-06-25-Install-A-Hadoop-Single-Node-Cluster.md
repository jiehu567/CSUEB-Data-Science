---
layout: page
mathjax: true
title: Installing A Hadoop Single Node Cluster
---
<br>
*In this article we look at installing a simple single node cluster of Hadoop. The install is meant to work on Ubuntu 14.04, but any debian based distro should work the same. Most of the command line output has been left out, important output will be shown.*
<br>

<a name = "top"></a>
### Table of Contents

- [1. Prerequesites](#prereqs)
  - [1.1 Install a JVM](#JVM)
  - [1.2 Create a Hadoop Group and User](#createUser)
  - [1.3 Install Open SSH](#openSSH)
- [2 Set Up SSH](#ssh)
- [3 Install Hadoop](#hadoop)
  - [3.1 Download Hadoop](#getHadoop)
  - [3.2 Configuring Hadoop](#configHadoop)
  - [3.3 Finalizing the Installation](#finalizing)

<a name = "prereqs"></a>
### 1. Prerequesites
[Top](#top)

<a name = "JVM"></a>
#### 1.1 Install Java 

We first install a Java Virtual Machine. I highly recommend to use the JVM by oracle. We can install it with the following:

``` bash
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java7-installer
```
We can confirm the installation was succesfull with

``` bash
$ java -version
java version "1.7.0_80"
Java(TM) SE Runtime Environment (build 1.7.0_80-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.80-b11, mixed mode)
```
<a name = "createUser"></a>
#### 1.2 Create the hduser

We now create a dedicated hadoop group and user which we will call `hadoop` and `hduser` respectively.

``` bash
$ sudo addgroup hadoop
$ # now to add the user to the group do:
$ sudo adduser --ingroup hadoop hduser 
# enter a password for hduser
``` 

<a name = "openSSH"></a>
#### 1.3 Install Open SSH

``` bash
$ sudo apt-get install openssh-server
```

Once complete we check the following give us the correct output

``` bash
$ which ssh
/usr/bin/ssh
$ which sshd
/usr/sbin/sshd
```

<a name ="ssh"></a>
### 2 Set Up SSH
[Top](#top)

First we want to add `hduser` to the sudo list such that we have administrator access.

``` bash
$ sudo adduser hduser sudo 
```

Let us now switch to the `hduser`

``` bash
$ su hduser
# enter password
$ ssh-keygen -t rsa -P ""
```

with the above we are creating certificates so that Hadoop can access its nodes using ssh. When asked for a file leave it blank by pressing enter. We also want to add this created key to our list of authorized keys, such when hadoop needs to connect we are not prompted for passwords, to do so we do the following:

``` bash
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys 
```

<a name = "hadoop"></a>
### 3 Install Hadoop
[Top](#top)

<a name = "getHadoop"></a>
#### 3.1 Downloading Hadoop

Let us retrieve the latest stable version of Hadoop, as of the creation of this guide the latest is 2.6.0, make sure you are logged in as hduser for the rest of the guide:

``` bash
$ wget http://mirrors.gigenet.com/apache/hadoop/common/hadoop-2.6.0/hadoop-2.6.0.tar.gz
$ tar xvzf hadoop-2.6.0.tar.gz
```

Once extracted  it will be in the home directory of hduser, we need to move it to the appropriate location, we do the following:

``` bash
$ sudo mv hadoop-2.6.0 /usr/local/hadoop
# enter hduser password
```

Lastly for good measure lets make sure hduser is the owner of the hadoop directory

``` bash
$ cd .. # move up one directory
$ pwd 
/usr/local # this should be the output (current working directory)
$ sudo chown -R hduser:hadoop /usr/local/hadoop
```

<a name = "configHadoop"></a>
#### 3.2 Configuring Hadoop

**Bashrc File**

First we want to edit the .bashrc file for hduser we can open it  with the command:

`$ nano ~/.bashrc`.

We need to add the following to the end of the file.

``` bash
$ nano ~/.bashrc
#HADOOP VARIABLES START
export JAVA_HOME=/usr/lib/jvm/java-7-oracle
export HADOOP_INSTALL=/usr/local/hadoop
export PATH=$PATH:$HADOOP_INSTALL/bin
export PATH=$PATH:$HADOOP_INSTALL/sbin
export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_HOME=$HADOOP_INSTALL
export HADOOP_HDFS_HOME=$HADOOP_INSTALL
export YARN_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_INSTALL/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib"
#HADOOP VARIABLES END
```

we exectute the basrch file with the following `source ~/.bashrc`.

**Hadoop-env.sh** 

Next we want to configure the hadoop enviroment script. Its location is /usr/local/hadoop/etc/hadoop/hadoop-env.sh. We need to add the following:

```  bash
# first we open the file with 
$ nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh
# and change the following variable.
export JAVA_HOME=/usr/lib/jvm/java-7-oracle
```

**core-site.xml**

Next we need to configure some files that hadoop needs at start up. First we make some required directories.

``` bash
$ sudo mkdir -p /app/hadoop/tmp
$ sudo chown hduser:hadoop /app/hadoop/tmp
```

Now we add the following to the file:/usr/local/hadoop/etc/hadoop/core-site.xml, in between the configuration `<configuration></configuration>`, tags.

``` bash
# to open the file we do 
$ nano /usr/local/hadoop/etc/hadoop/core-site.xml
# and we add the following in between the configuration tags.

<property>
<name>fs.default.name</name>
<value>hdfs://localhost:9000</value>
</property>
```

**mapred-site.xml**

Next we will edit the mapred file: /usr/local/hadoop/etc/hadoop/mapred-site.xml, first we copy a template with the following:

```bash
$ cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template /usr/local/hadoop/etc/hadoop/mapred-site.xml
```

Then we open the file and add the following between the configuration tags

``` bash
$ nano /usr/local/hadoop/etc/hadoop/mapred-site.xml # to open the file 
# and we add the following:

<property>
      <name>mapreduce.framework.name</name>
      <value>yarn</value>
</property>
```

**hdfs-site.xml**

Now we want to edit: /usr/local/hadoop/etc/hadoop/hdfs-site.xml, first we create some required directories and then configure the file itself, we do:

``` bash
# we create the directories:

$ sudo mkdir -p /usr/local/hadoop_store/hdfs/namenode
$ sudo mkdir -p /usr/local/hadoop_store/hdfs/datanode
$ sudo chown -R hduser:hadoop /usr/local/hadoop_store

# we open the file 
$ nano /usr/local/hadoop/etc/hadoop/hdfs-site.xml

# And we add the following between the tags <configuration></configuration>:

<property>
<name>dfs.replication</name>
<value>1</value>
<description>Default block replication.
The actual number of replications can be specified when the file is created.
The default is used if replication is not specified in create time.
</description>
</property>
<property>
<name>dfs.namenode.name.dir</name>
<value>file:/usr/local/hadoop_store/hdfs/namenode</value>
</property>
<property>
<name>dfs.datanode.data.dir</name>
<value>file:/usr/local/hadoop_store/hdfs/datanode</value>
</property>
```

**yarn-site.xml**

Lastly we also want to edit the `yarn-site.xml` file in the same directory as the ones above and add the following in between the configuration tags

```
<property>
      <name>yarn.nodemanager.aux-services</name>
      <value>mapreduce_shuffle</value>
</property>
<property>
      <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
      <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
```

<a name = "finalizing"></a>
#### 3.3 Finalizing the Installation 

We finish up the installation with a few steps. First we want to format the new Hadoop File System so we can begin to use it. We do:

``` bash
$ hadoop namenode -format
```
Next we start up hadoop with the following command (you may have to run this within the sbin directory in the hadoop installation directory)

``` bash
$ start-dfs.sh
$ start-yarn.sh
# we can check that hadoop is working with the follwing:
$ jps
# we should get an output that ressembles
9026 NodeManager
7348 NameNode
9766 Jps
8887 ResourceManager
7507 DataNode
```

[Top](#top)
