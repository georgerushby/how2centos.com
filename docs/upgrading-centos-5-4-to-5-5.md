---
id: 846
title: Upgrading CentOS 5.4 to 5.5
date: 2010-05-15T09:19:19+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=846
permalink: /upgrading-centos-5-4-to-5-5/
dsq_thread_id:
  - "94857811"
categories:
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
tags:
  - CentOS 5.5
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif)Before upgrading CentOS 5.4 to 5.5 it cannot be stressed enough how important it is to make a backup of your system before you do this. Actions listed in this post are written with the assumption that they will be executed by the root user running the bash or any other modern shell. 

Clean All Packages

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum clean all</pre>

The following command will get a list of packages that are going to be updated.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum list updates</pre>

Lets begin upgrading CentOS 5.4 to 5.5

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum update</pre>

Finally reboot the server for Kernel changes to take effect

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># reboot</pre>

Verify that CentOS 5.5 is working:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cat /etc/redhat-release
CentOS release 5.5 (Final)
</pre>

Or you can use the lsb_release command:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># lsb_release -a
LSB Version:    :core-3.1-ia32:core-3.1-noarch:graphics-3.1-ia32:graphics-3.1-noarch
Distributor ID: CentOS
Description:    CentOS release 5.5 (Final)
Release:        5.5
Codename:       Final
</pre>