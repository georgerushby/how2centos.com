---
id: 2783
title: Installing PHP 5.4 on CentOS 6.2
date: 2012-08-26T15:38:49+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2783
permalink: /installing-php-5-4-on-centos-6-2/
dsq_thread_id:
  - "819125165"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 6.2 Tutorials
---
The assumption for installing PHP 5.4 on CentOS 6.2 tutorial is that you are running as root and have a basic understanding of the software required but if you follow this tutorial you should be able to complete the task successfully.

### Install Yum Priorities

For a brief overview on and how to configure Yum Priorities you can follow the instructions outlined in our <a href="http://www.how2centos.com/install-yum-priorities-centos/" target="_blank">Install YUM Priorities on CentOS</a> tutorial.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># yum install yum-priorities</pre>

### Installing PHP 5.4 on CentOS 6.2 x86_64

### Install the EPEL x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm</pre>

### Install the IUS x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/6/x86_64/ius-release-1.0-10.ius.el6.noarch.rpm</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php54 php54-common php54-devel
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># php -v
PHP 5.4.5 (cli) (built: Jul 23 2012 10:10:54)
Copyright (c) 1997-2012 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2012 Zend Technologies
</pre>

### Installing PHP 5.4 on CentOS 6.2 i386

### Install the EPEL i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm</pre>

### Install the IUS i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/6/i386/ius-release-1.0-10.ius.el6.noarch.rpm</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php54 php54-common php54-devel
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># php -v
PHP 5.4.5 (cli) (built: Jul 23 2012 10:10:54)
Copyright (c) 1997-2012 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2012 Zend Technologies
</pre>