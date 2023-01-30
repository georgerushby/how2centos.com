---
id: 125
title: How to Set up Network Bonding on CentOS 5.x Tutorial
date: 2009-03-24T20:49:57+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=125
permalink: /how-to-set-up-network-bonding-with-centos-5x/
dsq_thread_id:
  - "44475311"
sidebar_layout:
  - ""
the_sidebar:
  - ""
lower_sidebar:
  - ""
content_sidebar:
  - ""
colorscheme:
  - ""
full_width_widget:
  - ""
hide_bottom_sidebars:
  - ""
featureboxes:
  - ""
carousel_items:
  - ""
carousel_mode:
  - ""
carousel_ngen_gallery:
  - ""
featuretitle:
  - ""
featuretext:
  - ""
featuremedia:
  - ""
categories:
  - CentOS 5.2 Tutorials
  - CentOS 5.3 Tutorials
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
tags:
  - centos 5.3
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="centos" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif)Ever wonder what to do with those extra NIC&#8217;s in your CentOS server? Why not bond them to add that extra throughput or load balancing and fault tolerance.

<!--more-->

**What is Bonding and it&#8217;s Modes?**

Bonding, the process in which multiple NIC&#8217;s are combined to function as one interface

Modes of bonding:

**mode=0 (Balance-rr)** &#8211; This mode provides load balancing and fault tolerance.  
**mode=1 (active-backup)** &#8211; This mode provides fault tolerance.  
**mode=2 (balance-xor)** &#8211; This mode provides load balancing and fault tolerance.  
**mode=3 (broadcast)** &#8211; This mode provides fault tolerance.  
**mode=4 (802.3ad)** &#8211; This mode provides load balancing and fault tolerance.  
**mode=5 (balance-tlb)** &#8211; Prerequisite: Ethtool support in the base drivers for retrieving the speed of each slave.  
**mode=6 (Balance-alb)** &#8211; Prerequisite: Ethtool support in the base drivers for retrieving the speed of each slave.

Read more&#8230;. 

**Note:** Always append extra configuration in case of a rollback.

**Preliminary Note:**

We will be using the [CentOS 5.2 YUM server](http://www.how2centos.com/creating-a-local-yum-repository-on-centos-5x/) in this tutorial:

* server1.example.co.za (IP 10.0.0.100): YUM Repo server Bond0

We need to increase the throughput of our local YUM repo so we&#8217;re bonding four of it&#8217;s six NIC&#8217;s

**Configuring the bond**

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /etc/sysconfig/network-scripts/
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi ifcfg-bond0
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >DEVICE=bond0
BOOTPROTO=none
ONBOOT=yes
#Fake MAC address
HWADDR=00:00:01:00:01:00
NETWORK=10.0.0.0
NETMASK=255.255.0.0
IPADDR=10.0.0.100
USERCTL=no
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi ifcfg-eth2
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >DEVICE=eth2
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=no
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi ifcfg-eth3
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >DEVICE=eth3
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=no
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi ifcfg-eth4
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >DEVICE=eth4
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=no
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi ifcfg-eth5
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >DEVICE=eth5
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=no
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/modprobe.conf
</pre>

We&#8217;ll be using mode=6 (Balance-alb)

Append the following onto the end out your modprobe config file

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >alias bond0 bonding
options bond0 mode=6 miimon=100
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># servive network restart
</pre>