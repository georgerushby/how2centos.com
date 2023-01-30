---
id: 422
title: Installing Cherokee with PHP 5.2 and MySQL Support On CentOS 5.3
date: 2009-10-05T19:01:16+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=422
permalink: /installing-cherokee-with-php5-2-and-mysql-support-on-centos-5-3/
dsq_thread_id:
  - "43376961"
wpcr_enable:
  - "1"
categories:
  - CentOS 5.3 Tutorials
tags:
  - centos 5.3
  - Cherokee
  - mysql
  - php 5.2.10
---
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/10/cherokee.png" alt="cherokee" width="122" height="40" class="alignleft size-full wp-image-423" /><img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="centos" width="42" height="40" class="alignleft size-full wp-image-225" />

Cherokee is a very fast, flexible and easy to configure Web Server. It supports the widespread technologies nowadays: FastCGI, SCGI, PHP, CGI, TLS and SSL encrypted connections, virtual hosts, authentication, on the fly encoding, load balancing, Apache compatible log files, and much more. This tutorial shows how you can install Cherokee on a CentOS server with PHP 5.2 support (through FastCGI) and MySQL support.  
<!--more-->

  
**Preliminary Note**

In this tutorial I use a base 64 bit server install with the hostname server1.example.co.za with the IP address 10.0.0.10. These settings might differ for you, so you have to replace them where appropriate.

Adjust any package names if installing the EL4 or 32 bit packages. If any dependencies are unsatisfied, install the required packages and retry.

Let&#8217;s begin by adding some Repositories that we&#8217;ll be needing

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install yum-priorities 
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># rpm -Uhv http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.3.6-1.el5.rf.i386.rpm
# rpm -Uvh http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-4.noarch.rpm
# rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/i386/ius-release-1.0-8.ius.el5.noarch.rpm 
</pre>

### Installing MySQL 5

First we install MySQL 5 like this:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum groupinstall "MySQL Database"
</pre>

You will be asked to provide a password for the MySQL root user &#8211; this password is valid for the user root@localhost as well as root@server1.example.co.za, so we don&#8217;t have to specify a MySQL root password manually later on:

New password for the MySQL &#8220;root&#8221; user: <&#8211; yourpassword  
Repeat password for the MySQL "root" user: <&#8211; yourrpassword

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chkconfig mysqld on
</pre>

### Installing Cherokee

Then we&#8217;ll install the Cherokee binaries and add it to startup:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install cherokee
# chkconfig cherokee on
# service cherokee start
</pre>

Now direct your browser to http://10.0.0.10, and you should see the Cherokee placeholder page:

Cherokee can be configured through a web-based control panel which we can start as follows:

cherokee-admin -b

(By default cherokee-admin binds only to 127.0.0.1 (localhost), with the -b parameter you can specify the network address to listen to. If no IP is provided, it will bind to all interfaces.)

Output should be similar to this one:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cherokee-admin -b

Login:
  User:              admin
  One-time Password: (your one-time password)

Web Interface:
  URL:               http://localhost:9090/

Cherokee Web Server 0.99.17 (Jun 14 2009): Listening on port ALL:9090, TLS
disabled, IPv6 disabled, using epoll, 1024 fds system limit, max. 505
connections, caching I/O, single thread
</pre>

The admin web interface can be found on http://10.0.0.10:9090/ (make sure to enter your one-time password)

To stop cherokee-admin, type CTRL+C on the shell.

### Installing PHP 5.2 with FastCGI and MySQL Support

We can make PHP 5.2 work in Cherokee through FastCGI:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php52-pear php52 php52-cli php52-common php52-devel php52-gd php52-mbstring php52-mcrypt php52-mysql php52-pdo php52-soap php52-xml php52-xmlrpc php52-bcmath php52-pecl-apc php52-pecl-memcache
</pre>

### Configuring PHP 5.2

There is not much to learn to configure PHP with Cherokee. Cherokee-admin ships a one-click wizard that will do everything for you. It will look for the PHP interpreter, it will check whether it supports FastCGI, and then it&#8217;ll perform all the necessary operations to set it up on Cherokee.

It requires a single operation to get PHP configured on a Virtual Server: Choose the virtual server your want to configure, and click on the Behavior tab. Once in there, click on Wizards.

Now select Languages and run the PHP wizard. And, that is it. If you had php-cgi installed in your system, PHP should be configured now and listed on the Behavior tab. Make sure you mark its checkbox in the Final column

Save your changes and reboot!

### Testing PHP 5.2 and getting details about your installation

The document root of the default web site is /var/www/cherokee. We will now create a PHP info file (info.php) in that directory and call it in a browser. The file will display lots of useful details about our PHP installation, such as the installed PHP version.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /var/www/cherokee/info.php
</pre>

<pre></pre>

Save the file and call that it in a browser (e.g. http://10.0.0.10/info.php):

As you see, PHP 5.2 is working, and it&#8217;s working through FastCGI, as shown in the Server API line. If you scroll further down, you will see all modules that are already enabled in PHP 5.2 including MySQL.

**Links**

Cherokee: <http://www.cherokee-project.com/>  
PHP: <http://www.php.net/>  
MySQL: <http://www.mysql.com/>  
CentOS: <http://centos.org/>