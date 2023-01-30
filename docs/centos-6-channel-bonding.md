---
id: 2229
title: CentOS 6 Channel Bonding
date: 2011-10-28T11:10:29+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2229
permalink: /centos-6-channel-bonding/
dsq_thread_id:
  - "455201603"
slider_image:
  - ""
categories:
  - CentOS 6.0 Tutorials
---
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="centos" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />CentOS 6 Channel Bonding allows administrators to bind multiple network interfaces together into a single channel using the bonding kernel module and a special network interface called a channel bonding interface. Channel bonding enables two or more network interfaces to act as one, simultaneously increasing the bandwidth and providing redundancy.

### CentOS 6 Channel Bonding

Channel bonding (also known as &#8220;Ethernet bonding&#8221;) is a computer networking arrangement in which two or more network interfaces on a host computer are combined for redundancy or increased throughput.

**mode=0 (Balance-rr)** &#8211; This mode provides load balancing and fault tolerance.  
**mode=1 (active-backup)** &#8211; This mode provides fault tolerance.  
**mode=2 (balance-xor)** &#8211; This mode provides load balancing and fault tolerance.  
**mode=3 (broadcast)** &#8211; This mode provides fault tolerance.  
**mode=4 (802.3ad)** &#8211; This mode provides load balancing and fault tolerance.  
**mode=5 (balance-tlb)** &#8211; Prerequisite: Ethtool support in the base drivers for retrieving the speed of each slave.  
**mode=6 (balance-alb)** &#8211; Prerequisite: Ethtool support in the base drivers for retrieving the speed of each slave.  
<!--more-->

  
**Note:** Always append extra configuration in case of a rollback.

### Configuring Channel Bonding

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /etc/sysconfig/network-scripts/
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi ifcfg-bond0
</pre>

We&#8217;ll be using mode=6 (balance-alb)

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >DEVICE=bond0
USERCTL=no
BOOTPROTO=none
ONBOOT=yes
IPADDR=10.0.0.10
NETMASK=255.255.0.0
NETWORK=10.0.0.0
BONDING_OPTS="miimon=100 mode=balance-alb"
TYPE=Unknown
IPV6INIT=no
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi ifcfg-eth0
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >DEVICE=eth0
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
USERCTL=no
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi ifcfg-eth1
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >DEVICE=eth1
BOOTPROTO=none
ONBOOT=yes
MASTER=bond0
SLAVE=yes
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

Due to the fact that /etc/modprobe.conf has been deprecated in CentOS 6, the process of bonding network interfaces has changed a bit.

Now instead of defining your bond in your /etc/modprobe.conf, you define it in /etc/modprobe.d/bonding.conf

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/modprobe.d/bonding.conf
</pre>

Append the following onto the end out your modprobe config file

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >alias bond0 bonding
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service network restart
</pre>