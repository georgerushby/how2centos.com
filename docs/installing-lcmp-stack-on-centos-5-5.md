---
id: 1278
title: Installing LCMP stack on CentOS 5.5
date: 2010-10-26T20:06:06+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1278
permalink: /installing-lcmp-stack-on-centos-5-5/
dsq_thread_id:
  - "162341644"
wpcr_enable:
  - "1"
categories:
  - CentOS 5.5 Tutorials
tags:
  - CentOS 5.5
  - Cherokee
  - mysql
  - php
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/10/LCMP-e1288103969866.png" alt="LCMP Stack" width="200" height="161" class="alignleft size-full wp-image-1288" />](http://www.how2centos.com/wp-content/uploads/2010/10/LCMP-e1288103969866.png) **_LCMP_** is an acronym for a stack of free, open source software from the first letters of Linux (operating system), Cherokee HTTP Server, MySQL and Perl/PHP/Python. These are the principal components to build a viable general purpose web server.

**_LCMP &#8211; Linux.Cherokee.MySQL.PHP/Perl/Python_**

In this tutorial we will be installing the following open source software components to build the **_LCMP_** stack. CentOS 5.5 (operating system), Cherokee 1.0.6 (web server), MySQL 5.0.77 (database server), PHP 5.3.3, Perl 5.8.8, Python 2.4.3 

Before we start just some general house keeping. The base CentOS 5.5 server hostname and IP address in this tutorial:

* centos01.how2centos.com (IP 10.0.0.3)

The assumption, for this CentOS 5.5 **_LCMP_** tutorial, is that you are running as root and have a medium understanding of the software required.  
<!--more-->

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install yum-priorities
</pre>

Download and install the relevant YUM install repositories

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># rpm -Uhv http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.3.6-1.el5.rf.i386.rpm
# rpm -Uvh http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-4.noarch.rpm
# rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/i386/ius-release-1.0-8.ius.el5.noarch.rpm
</pre>

Install Cherokee web server and RRDtool (allows for graphs on the vhosts)

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install cherokee
# yum install rrdtool
</pre>

Install MySQL server

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install mysql mysql-server
</pre>

Finally install PHP, Perl and Python. 

Note: If you upgrading make sure you remove your previous version of PHP

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php53u-pear php53u php53u-cli php53u-common php53u-devel php53u-gd php53u-mbstring php53u-mcrypt php53u-mysql php53u-pdo php53u-soap php53u-xml php53u-xmlrpc php53u-bcmath php53u-pecl-apc php53u-pecl-memcache php53u-snmp
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum update perl python
</pre>

Make sure everything that needs to be started at boot is on

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chkconfig cherokee on
# chkconfig mysqld on
</pre>

Create a PHP info page to view your newly install LCMP Stack

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /var/www/cherokee/phpinfo.php 
</pre>

<pre lang="php" line="1"></pre>

Copy the apc.php file (if you&#8217;re using APC) to the same directory.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cp /usr/share/doc/php53-pecl-apc-3.1.4/apc.php /var/www/cherokee/
</pre>

Start the required services

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service mysqld start
# service cherokee start
</pre>

Lastly we&#8217;ll configure Cherokee&#8217;s default vhost to use PHP (video below)

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cherokee-admin -b
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"></pre>

Once that’s done, visit http://your.server.address/apc.php and http://your.server.address/phpinfo.php