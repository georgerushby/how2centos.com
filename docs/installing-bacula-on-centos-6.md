---
id: 2035
title: Installing Bacula on CentOS 6
date: 2011-07-25T16:16:35+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2035
permalink: /installing-bacula-on-centos-6/
dsq_thread_id:
  - "368060136"
categories:
  - CentOS 6.0 Tutorials
---
Installing Bacula on CentOS 6 with Webmin interface quick start guide. The assumption for this CentOS 6 tutorial is that you are running as root and have a basic understanding of the software required but if you follow this tutorial you should be able to complete the task successfully.

### What is Bacula

<a href="http://www.bacula.org/en/" title="Bacula Home Page" target="_blank">Bacula</a> is a set of Open Source, enterprise ready, computer programs that permit you (or the system administrator) to manage backup, recovery, and verification of computer data across a network of computers of different kinds. <a href="http://www.bacula.org/en/" title="Bacula Home Page" target="_blank">Bacula</a> is relatively easy to use and efficient, while offering many advanced storage management features that make it easy to find and recover lost or damaged files. In technical terms, it is an Open Source, enterprise ready, network based backup program.

Bacula Website: <a href="http://www.bacula.org/en/" title="Bacula Home Page" target="_blank">http://www.bacula.org/en/</a>  
<!--more-->

### Install the EPEL x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm</pre>

### Disable SELinux

This is disabled at your own risk! We dont recommend it but it makes the installtion easier.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># vi /etc/sysconfig/selinux
</pre>

Change the following: SELINUX=enforcing to SELINUX=disabled

### Install the Webmin YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># cat > /etc/yum.repos.d/webmin.repo &lt;&lt; EOF<br />[Webmin]<br />name=Webmin Distribution Neutral<br />#baseurl=http://download.webmin.com/download/yum<br />mirrorlist=http://download.webmin.com/download/yum/mirrorlist<br />enabled=1<br />EOF</pre>



### Add the GPG Key and Install Webmin



<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm --import http://www.webmin.com/jcameron-key.asc</pre>

\# yum install webmin

### Install MySQL and Bacula

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># yum install mysql-devel mysql-server
# yum install bacula-storage-mysql bacula-docs
# yum install bacula-director-mysql bacula-console
# yum install bacula-client bacula-traymonitor
</pre>

### Start and Configure MySQL for Bacula

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># service mysqld start
# chkconfig mysqld on
</pre>

Change the MySQL root password if you have a fresh install of MySQL

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># mysqladmin -u root password 'new-password'
</pre>

### Now run the scripts

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># /usr/libexec/bacula/grant_mysql_privileges -u root -p
# /usr/libexec/bacula/create_mysql_database -u root -p
# /usr/libexec/bacula/make_mysql_tables -u root -p
# /usr/libexec/bacula/grant_bacula_privileges -u root -p
</pre>

### Edit the default config files

Change Director password, change address and password on Client, change Storage Address and Password, change Console password:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># vi /etc/bacula/bacula-dir.conf</pre>

Change bacula-fd password, change bacula-mon password:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># vi /etc/bacula/bacula-fd.conf</pre>

Change bacula-dir password, change bacula-mon password, change Device {Archive Device to /backup

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># vi /etc/bacula/bacula-sd.conf</pre>

Change Address and Password:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># vi /etc/bacula/bconsole.conf</pre>

Change Address Password and additionally change Director name to localhost

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># vi /etc/bacula/tray-monitor.conf</pre>

Create the backup folder 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># mkdir /backup
# chown bacula /backup
</pre>

### Set the MySQL password for user bacula

<pre class="lang:mysql toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># mysql -u root -p
Enter Password:
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " >mysql> UPDATE mysql.user SET password=PASSWORD ('somepassword')
mysql> WHERE user='bacula';
mysql> UPDATE mysql.user SET password=PASSWORD ('somepassword') WHERE user='bacula';
mysql> FLUSH PRIVILEGES;
mysql> quit
</pre>

### Start the Bacula Services

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># chkconfig bacula-dir on
# chkconfig bacula-fd on
# chkconfig bacula-sd on
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># service bacula-dir start
# service bacula-fd start
# service bacula-sd start
</pre>

Now use your web browser and go to the host IP address where you installed Bacula  
http://10.0.0.20:10000  
This will take you to the Webmin. Login using root and your password. You will find the Bacula module on the left under System