---
id: 2442
title: Install Percona XtraDB Cluster on CentOS 6.2
date: 2012-03-23T21:04:44+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2442
permalink: /install-percona-xtradb-cluster-on-centos-6-2/
slider_image:
  - ""
dsq_thread_id:
  - "622089842"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.6 Tutorials
  - CentOS 6.0 Tutorials
  - CentOS 6.2 Tutorials
---
<a href="http://www.percona.com/software/percona-xtradb-cluster/" target="_blank">Percona XtraDB Cluster</a> is a new high availability and high scalability solution for MySQL users. XtraDB Cluster integrates <a href="http://www.percona.com/software/percona-xtradb-cluster/" target="_blank">Percona </a>Server with the Galera library of high availability solutions in a single product package. Percona XtraDB is an enhanced version of the InnoDB storage engine for MySQL and MariaDB. It has much faster performance than InnoDB and better scalability on modern hardware.  
<!--more-->

  
This tutorial is how to setup a 3-node XtraDB cluster, assuming you are running CentOS Linux 6.2 64-bit.

### Install Percona’s CentOS 6 repositories:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># rpm -Uhv http://repo.percona.com/testing/centos/6/os/noarch/percona-testing-0.0-1.noarch.rpm
# rpm -Uhv http://www.percona.com/downloads/percona-release/percona-release-0.0-1.x86_64.rpm
</pre>

### Install Percona XtraDB Cluster packages:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install Percona-XtraDB-Cluster-server Percona-XtraDB-Cluster-client
</pre>

XtraDB Cluster requires a couple of TCP ports to operate. Easiest way:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service iptables stop
</pre>

If you want to open only specific ports, you need to open 3306, 4444, 4567, 4568 ports. 

For example for 4567 port (substitute 10.0.0.1 by your IP):

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># iptables -A INPUT -i eth0 -p tcp -m tcp --source 10.0.0.1/24 --dport 4567 -j ACCEPT
</pre>

Now repeat on all XtraDB cluster nodes

### Create /etc/my.cnf files

On the first XtraDB Cluster node (assume IP 10.0.0.10):

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >[mysqld]
datadir=/mnt/data
user=mysql

binlog_format=ROW

wsrep_provider=/usr/lib64/libgalera_smm.so

wsrep_cluster_address=gcomm://

wsrep_slave_threads=2
wsrep_cluster_name=trimethylxanthine
wsrep_sst_method=rsync
wsrep_node_name=node1

innodb_locks_unsafe_for_binlog=1
innodb_autoinc_lock_mode=2
</pre>

On the second XtraDB Cluster node:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >[mysqld]
datadir=/mnt/data
user=mysql

binlog_format=ROW

wsrep_provider=/usr/lib64/libgalera_smm.so

wsrep_cluster_address=gcomm://10.0.0.10

wsrep_slave_threads=2
wsrep_cluster_name=trimethylxanthine
wsrep_sst_method=rsync
wsrep_node_name=node2

innodb_locks_unsafe_for_binlog=1
innodb_autoinc_lock_mode=2
</pre>

On the third XtraDB Cluster node:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >[mysqld]
datadir=/mnt/data
user=mysql

binlog_format=ROW

wsrep_provider=/usr/lib64/libgalera_smm.so

wsrep_cluster_address=gcomm://10.0.0.10

wsrep_slave_threads=2
wsrep_cluster_name=trimethylxanthine
wsrep_sst_method=rsync
wsrep_node_name=node3

innodb_locks_unsafe_for_binlog=1
innodb_autoinc_lock_mode=2
</pre>

Start mysqld on the first node:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service mysql start
</pre>

When all nodes are in SYNCED stage your cluster is ready!

Connect to database on any node and create database:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mysql
> CREATE DATABASE how2centos_test;
</pre>

The new database will be propagated to all nodes.