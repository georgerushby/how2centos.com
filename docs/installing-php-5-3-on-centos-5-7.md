---
id: 2208
title: Installing PHP 5.3 on CentOS 5.7
date: 2011-10-10T13:33:35+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2208
permalink: /installing-php-5-3-on-centos-5-7/
dsq_thread_id:
  - "439126439"
slider_image:
  - ""
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.7 Tutorials
---
The assumption for installing PHP 5.3 on CentOS 5.7 tutorial is that you are running as root and have a basic understanding of the software required but if you follow this tutorial you should be able to complete the task successfully.

### Install Yum Priorities

For a brief overview on and how to configure Yum Priorities you can follow the instructions outlined in our <a href="http://www.how2centos.com/install-yum-priorities-centos/" target="_blank">Install YUM Priorities on CentOS</a> tutorial.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># yum install yum-priorities</pre>

### Installing PHP 5.3 on CentOS 5.7 x86_64

### Install the EPEL x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm</pre>

### Install the IUS x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/x86_64/ius-release-1.0-10.ius.el5.noarch.rpm</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php53u php53u-common php53u-devel
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># php -v
PHP 5.3.8 (cli) (built: Aug 23 2011 06:33:32)
Copyright (c) 1997-2011 The PHP Group
Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies
</pre>

### Installing PHP 5.3 on CentOS 5.7 i386

### Install the EPEL i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm</pre>

### Install the IUS i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/i386/ius-release-1.0-10.ius.el5.noarch.rpm</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php53u php53u-common php53u-devel
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># php -v
PHP 5.3.8 (cli) (built: Aug 23 2011 06:33:32)
Copyright (c) 1997-2011 The PHP Group
Zend Engine v2.3.0, Copyright (c) 1998-2011 Zend Technologies
</pre>