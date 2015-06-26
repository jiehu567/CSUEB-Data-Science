---
layout: page
title: Install A Hadoop Multinode Cluster
mathjax: true
---

<br>
*This is a follow up to the single node cluster set up in a previous article. A lot of implementation in the this guide assumes you have a single node cluster already set on each of the computers that will be a part of the Hadoop cluster.*
<br>

<a name = "top"></a>
### Table of Contents

- [1. Setting Up A Local Network](#networkSetup)
- [2. Configure SSH](#ssh)
- [3. Hadoop Cluster Configurations](#hadoopConfig)
  - [3.1 Master Node Configurations](#master)
  - [3.2 All Nodes Configurations](#allNodes)
- [4. Starting The Cluster](#start)
  - [4.1 Formating HDFS](#format)
  - [4.2 Starting HDFS Daemons](#hdfs)
  - [4.3 Starting YARN Daemons](#yarn)


<a name = "networkSetup"></a>
### 1. Setting Up A Local Network 
[Top](#top)

The first thing we want to do is make sure the computers can talk to each other. Assuming they are
connected to each other via a switch, we shall manually set up an IP address for each one. First we want to
find out the computer's mac address, we do so like this:

```bash
$ ifconfig
# we are looking for an out that resembles:
eth0      Link encap:Ethernet  HWaddr 00:30:1b:b9:53:94

```

Here we have that `HWaddr 00:30:1b:b9:53:94` is the mac address we are looking for.

Although we could do this all through terminal we will use the **Network Manager Applet** in Ubuntu.
After opening the Network Manger, we select **Add** and enter the MAC address noted above.

Next we select IP adress for the computer, for now we pick `10.0.0.1` with netmask `255.255.255.0`, the next computer
would then have IP adress `10.0.0.2` and so on. All will have same netmask.

A ping from `10.0.0.1` to `10.0.0.2` should resemble something similar to this:

```bash
$ ping 10.0.0.2  
PING 10.0.0.2 (10.0.0.2) 56(84) bytes of data.
64 bytes from 10.0.0.2: icmp_seq=1 ttl=128 time=0.457 ms
```

Assuming that `10.0.0.1` will be the master we finish the networking process by editing the **/etc/hosts** file on each of the computers.

```bash
# we open the hosts file with 
$ sudo nano /etc/hosts
```

We add the following lines to all the computers:

```
10.0.0.1 master
10.0.0.2 slave1
10.0.0.3 slave3
# add more accordingly 
```

<a name = "ssh"></a>
### 2. Configure SSH 
[Top](#top)

What we want to do now is make it such that the master node log in securely to each of the slave nodes without having to enter a password. In order to do this we simply need to copy the masters public ssh key to the `authorized_keys` file in the slave's file (all the slaves). To do this we do the following:

```bash 
$ ssh-copy-id -i $HOME/.ssh/id_rsa.pub hduser@slave
# this will prompt for password of hduser on slave computer 
```

We confirm that the above steps worked with the following:

``` bash
$ ssh master
# out should resemble
The authenticity of host 'master...
```

and we do the same for all the slaves, this is required.

``` bash 
$ ssh slave1
# output should resemble
The authenticity of host 'slave1...
```

<a name = "hadoopConfig"></a>
### 3. Hadoop Cluster Configurations
[Top](#top)

<a name = "master"></a>
#### 3.1 Master Node Configurations

The first file we want to configure is the `masters` file which is located in `hadoop/etc/hadoop/` if the file doesnt already exist we want to create it.
We are going to update the file with the line:


```
master
```

Simiarly we want to edit the `slaves` file in the same directory. We are going to populate it with the slave machines in the cluster, one hostname per line.
I have added master node since I wish to have the master be both master and slave.

```
master
slave1 
slave2 
slave3
```

add slave machines according to your set up.

<a name = "allNodes"></a>
#### 3.2 All Nodes

The next couple of files we want to edit on all machines.

*__Note that we are either changing an existing parameter or appending to a file that already has content from the single node cluster.__*

**core-site.xml**

The first file we want to edit is the `core-site.xml` located in `hadoop/etc/hadoop/`. The file will specify Namenode host and port, add the following: `hdfs://master:54310`
in between the `<value></value>` tags.
 
We change the locahost for master on all machines.

```
<property>
  <name>fs.default.name</name>
  <value>hdfs://master:9000</value>
</property>
```

**mapred-site.xml**

The next file we want to edit is in the same directory and is called `mapred-site.xml`. This file specfies the Jobtracker and port. We modify by 
adding `master` instead of `localhost`, in between the `<value> </value>` tags. 

The file should now resemble something like this:

```
<property>
	<name>mapreduce.job.tracker</name>
	<value>HadoopMaster:5431</value>
</property>
<property>
	<name>mapred.framework.name</name>
	<value>yarn</value>
</property>
```

**hdfs-ste.xml**

The next file we want to modify is called the `hdfs-site.xml` in the same directory. What this file does is it defines how many machines a single 
file should replicated to, before it becomes available. We want to set this number equal to the number of available DataNodes in the cluster. We want
to add this value  in between the tags `<value> </value>`. The file should now look something like this:

```
<property>
  <name>dfs.replication</name>
  <value>3</value>
</property>
```

**yarn-site.xml**

Lastly we edit the yarn file once again we want to change localhost to the master

```
<property>
	<name>yarn.resourcemanager.resource-tracker.address</name>
	<value>HadoopMaster:8025</value>
</property>
<property>
	<name>yarn.resourcemanager.scheduler.address</name>
	<value>HadoopMaster:8035</value>
</property>
<property>
	<name>yarn.resourcemanager.address</name>
	<value>HadoopMaster:8050</value>
</property>
```

<a name = "start"></a>
### 4. Starting the Cluster 
[Top](#top)

<a name = "format"></a>
#### 4.1 Formatting HDFS

Prior to starting the cluster we must first format HDFS, we want to do this from within the namenode. We do so the same way we set up the single node cluster:

*Doing this will erase __All__ existing data on HDFS.*

``` bash 
$ hadoop namenode -format
```

<a name = "hdfs"></a>
#### 4.2 Starting HDFS Daemons

To start the hdfs daemon, we want to run the following on the primary namenode machine in the cluster. In our case this is the master machine. We do the 
following: 

``` bash 
$ start-dfs.sh
```

We can check success or failure of the above command from any of the `slave` machines by inspecting the `hadoop-hduser-datanode-slave.log` log file.

Once this process is complete we should be able to run the following on master machine: 

``` bash 
$ jps
14799 NameNode
15314 Jps
14880 DataNode
14977 SecondaryNameNode
```

And running the same command `jps` on the `slave` machines we get 

``` bash 
$ jps
15183 DataNode
15616 Jps
```

<a name = "yarn"></a>
#### 4.3 Starting YARN Daemons

Now we will start the mapred daemons from the machine we wish to have the JobTracker to run on. In our case we want this to be on the `master` node.
To do this we want to run the command:

``` bash 
$ start-mapred.sh
```

Once we can check the log of this process from any of the slaves by the checking the `hadoop-hduser-tasktracker-slave.log` log file. 

Running `jps` on master and slave machines should now output the following respectively:

``` bash 
$ jps
16017 Jps
14799 NameNode
15686 TaskTracker
14880 DataNode
15596 JobTracker
14977 SecondaryNameNode

# and for the slave machines
$ jps
15183 DataNode
15897 TaskTracker
16284 Jps

```
