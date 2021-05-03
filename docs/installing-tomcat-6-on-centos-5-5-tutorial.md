---
id: 1740
title: Installing Tomcat 6 on CentOS 5.5 Tutorial
date: 2011-01-11T14:19:28+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1740
permalink: /installing-tomcat-6-on-centos-5-5-tutorial/
dsq_thread_id:
  - "207071146"
categories:
  - CentOS 5.5 Tutorials
tags:
  - CentOS 5.5
  - tomcat
---
[Apache Tomcat](http://tomcat.apache.org/) is an open source software implementation of the Java Servlet and JavaServer Pages technologies. The Java Servlet and JavaServer Pages specifications are developed under the [Java Community Process](http://jcp.org/en/introduction/overview). 

So before we start just some general house keeping. The base CentOS 5.5 server hostname and IP address that we’ll be using in this tutorial:

* www.how2centos.com (IP 10.0.0.3)

The Tomcat 6 Server will eventually be available on http://www.how2centos.com:8080

The assumption, for this Tomcat 6 and CentOS 5.5 tutorial, is that you are running as root and have a medium understanding of the software required but if you’re Awesome that&#8217;s good enough.  
<!--more-->

### Install Yum Priorities

For a brief overview on and how to configure Yum Priorities you can follow the instructions outlined in our <a href="http://www.how2centos.com/install-yum-priorities-centos/" target="_blank">Install YUM Priorities on CentOS</a> tutorial.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># yum install yum-priorities</pre>

### Install the EPEL i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm</pre>

### Install the RPMForge i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.5.2-2.el5.rf.i386.rpm</pre>

### Install the JPackage Project repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /etc/yum.repos.d/
# wget http://jpackage.org/jpackage50.repo
</pre>

### Install Java

We just used the openjdk that available via the repositories but if you prefer Sun JDK the download it and install it.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">yum install java
</pre>

### Install Tomcat6

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install tomcat6 tomcat6-webapps tomcat6-admin-webapps
</pre>

Add in JAVA\_HOME under the CATALINA\_TMPDIR reference

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /usr/share/tomcat6/conf/tomcat6.conf
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >JAVA_HOME="/usr/lib/jvm/java-1.6.0-openjdk-1.6.0.0/"
</pre>

### Start the Tomcat 6 service

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service tomcat6 start
</pre>

Browser to your newly installed Tomcat6 Server (remember port 8080)

http://www.how2centos.com:8080