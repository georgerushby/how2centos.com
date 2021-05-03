---
id: 1946
title: 'CentOS: Configure network'
date: 2011-07-14T19:12:51+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1946
permalink: /centos-configure-network/
dsq_thread_id:
  - "358486510"
wpcr_enable:
  - "1"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.3 Tutorials
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
  - CentOS 5.8 Tutorials
---
This tutorial is intended for system administrators wanting to either change the IP address or add additional LAN cards (NIC) on their CentOS 5 system. There are a couple of ways to configure the network card using the command line but only some commands will take immediate effect on kernel. If you are doing this remotely remember that you will lose connectivity or if your configuration on your network is incorrect be unable to connect.

### Configure network with immediate effect

Using a single command line to configure the network

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ifconfig eth0 192.168.0.10 netmask 255.255.255.0
</pre>

[or]

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ip addr add 192.168.0.10 dev eth0
</pre>

<!--more-->

### Configure network with setup or netconfig

If you are using netconfig (or) setup utility it will only overwrite the /etc/sysconfig/network-scripts/ifcfg-eth0 file. Then , after that u have to restart the network service like follow,

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># setup
</pre>

Network configuration -> Edit Devices -> eth0 (eth0) &#8211; Intel Corporation 82540EM Gigabit Ethernet Controller

Save your settings and quit

[or]

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># system-config-network
</pre>

Edit Devices -> eth0 (eth0) &#8211; Intel Corporation 82540EM Gigabit Ethernet Controller

Save your settings and quit

Restart your network in order for your configuration to take effect.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># /etc/init.d/network restart
</pre>

### Configure network by editing configuration

Lastly you can configure the network by editing the configuration files stored in the /etc/sysconfig/network-scripts/ directory.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /etc/sysconfig/network-scripts/
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi ifcfg-eth0
</pre>

Append/modify as follows:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># Intel Corporation 82540EM Gigabit Ethernet Controller
DEVICE=eth0
BOOTPROTO=none
ONBOOT=yes
HWADDR=06:01:78:a7:00:33
NETMASK=255.255.255.0
IPADDR=192.168.0.10
TYPE=Ethernet
</pre>

Save and close the file. Define default gateway and hostname in /etc/sysconfig/network configuration file

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/sysconfig/network
</pre>

Append/modify configuration as follows:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >NETWORKING=yes
NETWORKING_IPV6=no
HOSTNAME=centos.how2centos.com
GATEWAY=192.168.0.1
</pre>

Save and close the file. Restart networking:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># /etc/init.d/network restart
</pre>