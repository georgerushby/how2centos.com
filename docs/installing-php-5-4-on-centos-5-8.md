---
id: 2794
title: Installing PHP 5.4 on CentOS 5.8
date: 2012-07-28T11:10:36+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2794
permalink: /installing-php-5-4-on-centos-5-8/
dsq_thread_id:
  - "821476579"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.8 Tutorials
---
This tutorial is intended for system administrators wanting to install PHP 5.4 on CentOS 5.8

The reader should know how to configure a web server or application server and have basic knowledge of the HTTP protocol. Once finished the reader should have PHP 5.4 running with the default configuration.

### Install Yum Priorities

For a brief overview on and how to configure Yum Priorities you can follow the instructions outlined in our <a href="http://www.how2centos.com/install-yum-priorities-centos/" target="_blank">Install YUM Priorities on CentOS</a> tutorial.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># yum install yum-priorities</pre>

### Installing PHP 5.4 on CentOS 5.8 x86_64

### Install the EPEL x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm</pre>

### Install the IUS x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/x86_64/ius-release-1.0-10.ius.el5.noarch.rpm</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php54 php54-common php54-devel
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># php -v
PHP 5.4.5 (cli) (built: Jul 23 2012 10:10:54)
Copyright (c) 1997-2012 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2012 Zend Technologies
</pre>

### Installing PHP 5.4 on CentOS 5.8 i386

### Install the EPEL i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm</pre>

### Install the IUS i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/i386/ius-release-1.0-10.ius.el5.noarch.rpm</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php54 php54-common php54-devel
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># php -v
PHP 5.4.5 (cli) (built: Jul 23 2012 10:10:54)
Copyright (c) 1997-2012 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2012 Zend Technologies
</pre>