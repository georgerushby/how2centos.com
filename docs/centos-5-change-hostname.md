---
id: 1103
title: CentOS 5 Change Hostname
date: 2010-09-02T11:16:35+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1103
permalink: /centos-5-change-hostname/
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
dsq_thread_id:
  - "136524863"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.3 Tutorials
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
  - CentOS 5.8 Tutorials
---
In this CentOS tutorial we will be showing you how to find and change the hostname of your system. 

The assumption is that you are running as root and have a basic understanding of the software required but if you follow this tutorial you should be able to complete the task successfully.

Let us begin by finding the CentOS systems fully qualified domain name (FQDN) by seeing how it identifies itself.

### Print the network node hostname

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># uname -n
centos01.how2centos.com
</pre>

### Show the systems DNS domain name

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># dnsdomainname 
how2centos.com
</pre>

<!--more-->

  
There are a couple of ways to change the hostname. 

Edit /etc/sysconfig/network

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/sysconfig/network
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >NETWORKING=yes
NETWORKING_IPV6=no
HOSTNAME=centos01.how2centos.com
</pre>

or run System Config

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># system-config-network
</pre>

Select Edit DNS configuration 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">ââââââââ¤ Select Action âââââââ
  â
  â&nbsp;&nbsp;Edit a device params
  â&nbsp;&nbsp;Edit DNS configuration
  â
  â
  â
  â&nbsp;&nbsp;âââââââââââââ&nbsp;&nbsp;ââââââââ
  â&nbsp;&nbsp;â Save&Quit â&nbsp;&nbsp;â Quit 
  â&nbsp;&nbsp;âââââââââââââ&nbsp;&nbsp;ââââââââ
  â
  â
  âââââââââââââââââââââââââââââââ
  
  âââââââ¤ DNS configuration âââââââ
  â
  â
  â&nbsp;&nbsp;Hostname&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;centos01.how2ce
  â&nbsp;&nbsp;Primary DSN&nbsp;&nbsp;&nbsp;10.0.0.10______
  â&nbsp;&nbsp;Secondary DNS&nbsp;_______________
  â&nbsp;&nbsp;Tertiary DNS&nbsp;&nbsp;_______________
  â&nbsp;&nbsp;Search&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;how2centos.com_
  â
  â&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ââââââ&nbsp;&nbsp;ââââââââââ
  â&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;â Ok â&nbsp;&nbsp;â Cancel â
  â&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ââââââ&nbsp;&nbsp;ââââââââââ
  â
  â
  ââââââââââââââââââââââââââââââââââ
</pre>

Edit the hostname and select OK and then Save&Quit 

Dont forget to change the hostname in your hosts file

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/hosts
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># Do not remove the following line, or various programs
# that require network functionality will fail.
127.0.0.1       centos01.how2centos.com centos01 localhost.localdomain localhost
::1             localhost6.localdomain6 localhost6
</pre>

However we’re not finished. The machine wont normally see the updated hostname until about it reboots, but we can force it to update. 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># source /etc/sysconfig/network
# hostname $HOSTNAME
</pre>