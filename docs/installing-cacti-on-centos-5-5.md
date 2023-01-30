---
id: 1340
title: Installing Cacti on CentOS 5.5
date: 2010-11-19T23:59:46+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1340
permalink: /installing-cacti-on-centos-5-5/
dsq_thread_id:
  - "177618807"
wpcr_enable:
  - "1"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.5 Tutorials
tags:
  - CentOS 5.5
  - Cherokee
  - mysql
  - php
---
In this tutorial we will be installing Cacti on CentOS 5.5 using the <a href="http://www.how2centos.com/installing-lcmp-stack-on-centos-5-5/" target="blank_">LCMP stack</a> (Linux, Cherokee, MySQL and PHP).

What is Cacti? Cacti is a complete network, server and application graphing solution harnessing the power of RRDtool OpenSource industry standard, high performance data logging and graphing.

So before we start just some general house keeping. The base CentOS 5.5 server hostname and IP address that we&#8217;ll be using in this tutorial:

* centos01.how2centos.com (IP 10.0.0.3)

The Cacti server will eventually be available on http://cacti.how2centos.com

The assumption, for this Cacti and CentOS 5.5 tutorial, is that you are running as root and have a medium understanding of the software required or you&#8217;re Awesome.  
<!--more-->

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install yum-priorities 
</pre>

### Install the RPMForge i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.5.2-2.el5.rf.i386.rpm</pre>

### Install the EPEL i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm</pre>

### Install the IUS i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/i386/ius-release-1.0-10.ius.el5.noarch.rpm</pre>

### Install Cherokee web server

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install cherokee rrdtool
</pre>

### Install PHP 5.3

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php53u-pear php53u php53u-cli php53u-common php53u-devel php53u-gd php53u-mbstring php53u-mcrypt php53u-mysql php53u-pdo php53u-soap php53u-xml php53u-xmlrpc php53u-bcmath php53u-pecl-apc php53u-pecl-memcache php53u-snmp 
</pre>

### Install MySQL and MySQL Server

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install mysql mysql-server
</pre>

### Install SNMP

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install net-snmp net-snmp-utils
</pre>

### Install Cacti

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install cacti
</pre>

Configure snmpd, move snmpd.conf and create a new one. The &#8216;snmpuser&#8217; is what you&#8217;ll use later in the Cacti interface.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mv /etc/snmp/snmpd.conf /etc/snmp/snmpd.conf.old
# echo &#034;rocommunity		snmpuser&#034; &gt; /etc/snmp/snmpd.conf
</pre>

Let make sure that everything is added to runlevels 2, 3, 4 and start them up.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chkconfig mysqld on
# chkconfig snmpd on
# chkconfig cherokee on
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service mysqld start
# service snmpd start
# service cherokee start
</pre>

Create &#8216;cacti&#8217; MySQL database and grant privileges to &#8216;cactiuser&#8217; with password &#8216;cactipassword&#8217;

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 14323
Server version: 5.0.77 Source distribution

Type 'help;' or '\h' for help. Type '\c' to clear the buffer.

mysql&gt;create database cacti;
mysql&gt;GRANT ALL ON cacti.* TO cactiuser@localhost IDENTIFIED BY &#039;cactipassword&#039;;
mysql&gt;quit
</pre>

Import the Cacti database schema 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mysql -ucactiuser -pcactipassword cacti &lt; /var/www/cacti/cacti.sql
</pre>

Configure Cacti with the details above.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /var/www/cacti/include/config.php 
</pre>

<pre lang="php" line="1">/* make sure these values refect your actual database/host/user/password */
$database_type = "mysql";
$database_default = "cacti";
$database_hostname = "localhost";
$database_username = "cactiuser";
$database_password = "cactipassword";
$database_port = "3306";
</pre>

Once all that has been done time to get PHP 5.3 working with Cherokee and then adding the Cacti virtual server.

Firstly lets get PHP 5.3 working with Cherokee

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cherokee-admin -b
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"></pre>

Finally add the Cacti virtual server and browser to the URL and follow the onscreen instuctions.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"></pre>