---
id: 1159
title: Installing PHP 5.3.3 on CentOS 5.5 Tutorial
date: 2010-09-14T16:00:06+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1159
permalink: /installing-php-5-3-3-on-centos-5-5-tutorial/
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
  - "141660301"
slider_image:
  - ""
categories:
  - CentOS 5.5 Tutorials
tags:
  - CentOS 5.5
  - php
---
**Update:** This tutorial will install PHP 5.3.6

In celebration of CentOS now leading the Linux distribution statistics on web servers, with almost 30% of all Linux servers, we thought it fitting that in this tutorial we will show you how to install PHP 5.3.3 (Supports the Kohana Framework) with APC and Memcached on CentOS 5.5.

The assumption for this CentOS 5.5 tutorial is that you are running as root and have a basic understanding of the software required but if you follow this tutorial you should be able to complete the task successfully.  
<!--more-->

**Preliminary Note:**

I am using a CentOS 5.5 32bit server in this tutorial:

* centos01.how2centos.com (IP 10.0.0.3)

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install yum-priorities
</pre>

### Installing PHP 5.3 on CentOS 5.5 i386

### Install the EPEL i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm</pre>

### Install the IUS i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/i386/ius-release-1.0-10.ius.el5.noarch.rpm</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install httpd 
</pre>

**NOTE:** If you have a previously installed version of PHP run this command.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum remove php php-*
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php53u-pear php53u php53u-cli php53u-common php53u-devel php53u-gd php53u-mbstring php53u-mcrypt php53u-mysql php53u-pdo php53u-soap php53u-xml php53u-xmlrpc php53u-bcmath php53u-pecl-apc php53u-pecl-memcache php53u-snmp
</pre>

**UPDATE:** The above will install APC version 3.1.9 which does not have the bug mentioned below.

**NOTE:** Version 3.1.4 of APC has a bug that will give you this error in you error.log file _PHP Warning: require(): Unable to allocate memory for pool._

To resolve this do the following 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum remove php53u-pecl-apc
# yum update php53u-pecl-apc &#45;&#45;enablerepo=ius-testing
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /var/www/html/phpinfo.php
</pre>

<pre lang="php" line="1"><?php

// Show all information, defaults to INFO_ALL
phpinfo();

// Show just the module information.
// phpinfo(8) yields identical results.
phpinfo(INFO_MODULES);

?>
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chkconfig httpd on
# service httpd start
</pre>

Finally browse to your phpinfo.php http://10.0.0.3/phpinfo.php and view your newly configured PHP 5.3.3 Apache server.