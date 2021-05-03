---
id: 1811
title: Upgrading CentOS 5.5 to CentOS 5.6
date: 2011-04-11T10:55:34+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1811
permalink: /upgrading-centos-5-5-to-centos-5-6/
dsq_thread_id:
  - "276554209"
wpcr_enable:
  - "1"
categories:
  - CentOS 5.6 Tutorials
---
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="" width="42" height="40" class="alignleft size-full wp-image-225" /> Before upgrading CentOS 5.5 to 5.6 it cannot be stressed enough how important it is to make a backup of your system before you do this. Actions listed in this post are written with the assumption that they will be executed by the root user running the bash or any other modern shell. 

Clean All Packages

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum clean all</pre>

The following command will get a list of packages that are going to be updated.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum list updates</pre>

### Upgrade CentOS 5.5 to 5.6



<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum update</pre>

Finally reboot the server for Kernel changes to take effect

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># reboot</pre>

Verify that CentOS 5.6 is working:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cat /etc/redhat-release
CentOS release 5.6 (Final)
</pre>

Or:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># lsb_release -a
LSB Version:    :core-4.0-amd64:core-4.0-noarch:graphics-4.0-amd64:graphics-4.0-noarch:printing-4.0-amd64:printing-4.0-noarch
Distributor ID: CentOS
Description:    CentOS release 5.6 (Final)
Release:        5.6
Codename:       Final
</pre>

Or:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># rpm -q centos-release
centos-release-5-6.el5.centos.1
</pre>