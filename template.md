# Article title

<!-- A single line about what this document is for -->
This document is to show how to setup RocksDB with the HDFS plugin enabled and how to use it. 

---

## Table of Contents

<!-- Refers to the repository wide ToC. For Document ToC github has built in features. 
In case your document needs an internal ToC you are doing something wrong, break the document down into smaller parts! -->

<!-- Example -->
+ [parent (rocksdb)](./../RocksDB/)

----

## Definitions

<!-- Some non-trivial terms or definitions that are going to be used in the document -->
For this document, the following 3 servers are used.
+	HDFS Master server: &emsp; 10.128.0.11 &emsp; hdfs-master

---

## Pre-requisites

<!-- Things that you are not going to be covering in this document but are still very much essential for the reader to know about before they get into this application -->
A core part that is not going to be covered is HDFS installation. You need to have HDFS installed in order to have RocksDB work with HDFS
+ [Installing HDFS](./HDFS/HDFS%20Full-Distributed%20Setup.md)


---

## Setup

<!-- How your particular system is to be set up. Go through the steps -->
The following steps need to be performed on all servers in the HDFS cluster.

<!-- Sub sections are highly encouraged if the installation is particularly long -->
<!-- Try to have sub sections at places that act like breakpoints. So, if there is a screw up, commands from the last breakpoint only have to be repeated -->
### Installing Libraries and downloading HDFS
1.	`sudo apt-get install -y openjdk-8-jdk openssh-server openssh-client`
2.	`wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz`
3.	`tar -xzf hadoop-3.3.1.tar.gz`

### Configuring your hosts file
The file /etc/hosts needs to be configured for machines in the cluster to be able to locate the other machines without using IP all the time. The master server needs to be able to ssh to itself and the slave servers and the slave servers need to ssh to themselves and the master server.
1.	`nano /etc/hosts`
2.	Update your file as per the requirement 
    
    ![HDFS Hosts](./media/hdfs-hosts.png)

---

## Usage

<!-- How to actually use it, or how to run some basic tests on the system once the installation is completed -->
Your HDFS setup is now complete and the above steps need not be repeated. Now you can use the master node The following set of commands are useful to start and stop the hdfs setup.
1.	To start the hdfs setup: `$(HADOOP_HOME)/sbin/start-dfs.sh` 
2.	To stop the hdfs setup: `$(HADOOP_HOME)/sbin/stop-dfs.sh`
3.	To check status of hdfs: `hdfs dfsadmin report`

    ![HDFS status report](./media/hdfs-status.png) 


---

## Further Reading

<!-- What's next, some furture stuff you wanted to read into in the given topic and couldn't -->
<!-- or just something that is an interesting read after the day of work -->

You can read into how to setup HDFS and RocksDB on different servers next!!
1. Status: To Do


---

## Contributors

<!-- Who all have contributed to writing that particular article -->
<!-- In case of multiple authors, in order of contribution. Mention at least 1 mod of getting in touch (github/mail/etc) -->
1. Viraj Thakkar ([veedata](github.com/veedata))
