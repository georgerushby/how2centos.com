---
id: 621
title: Installing Jasper reporting for MySQL on CentOS 5.4
date: 2009-11-06T14:00:55+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=621
permalink: /installing-jasper-reporting-for-mysql-on-centos-5-4/
dsq_thread_id:
  - "43708111"
categories:
  - CentOS 5.4 Tutorials
tags:
  - bitnami
  - centos 5.4
  - jasper
  - mysql
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/11/bitnami_stack-150x150.jpg" alt="bitnami_stack" title="bitnami_stack" width="42" height="40" class="alignleft size-thumbnail wp-image-645" />](http://www.how2centos.com/wp-content/uploads/2009/11/bitnami_stack.jpg)[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="centos" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif)**What is BitNami?**

BitNami Native Installers automate the setup of a BitNami Stack on Windows, Linux or Mac OS X. Each installer includes all of the software necessary to run out of the box (the Stack). The process is simple; just download, click next-next-next and you are done! BitNami Stacks are completely self contained and will not interfere with other software on your system.

<http://bitnami.org/>

**What is Jasper for MySQL?**

JasperServer Professional is a high-performance standalone and embeddable report server that provides non-technical business users with:

* Drag and drop ad hoc report building  
* Drag and drop dashboarding with live-refresh, and mash-ups with external content  
* A rich business metadata layer for easy ad hoc query  
* Integrated and in-memory data analysis  
* Built-in charting and integration with third-party visualization tools  
* Self-service parameterized web reporting  
* Report scheduling, distribution and historical versioning  
* A secure report and metadata repository  
* Access to any data source including relational, XML, Hibernate, EJB, POJO, and custom  
* Row and column level data security

<http://www.jaspersoft.com/jasperserver>

**Preliminary Note:**

I am using a CentOS 5.4 x86_64 base installation in this tutorial

* server1.example.co.za (IP 10.0.0.100): _CentOS 5.4 x86_64 Base installation_

**Download and Install BitNami JasperSever Stack**

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://bitnami.org/files/stacks/jasperserver/bitnami-jasperserver-3.5.0-0-linux-installer.bin
# chmod 755 ./bitnami-jasperserver-3.5.0-0-linux-installer.bin
# ./bitnami-jasperserver-3.5.0-0-linux-installer.bin
</pre>

<pre class="brush: text">----------------------------------------------------------------------------
Welcome to the BitNami JasperServer Stack Setup Wizard.

----------------------------------------------------------------------------
Installation folder

Please, choose a folder to install BitNami JasperServer Stack

Select a folder [/opt/jasperserver-3.5.0-0]:

----------------------------------------------------------------------------
MySQL Information

Please enter your MySQL database information:

MySQL Server port [3306]:

----------------------------------------------------------------------------
MySQL Credentials

Please enter your database root user password

MySQL Server root password :
Re-enter password :
----------------------------------------------------------------------------
Setup is now ready to begin installing BitNami JasperServer Stack on your computer.

Do you want to continue? [Y/n]:
</pre>

In this installation I went ahead and chose all the defaults

**Note:** if you get this error:

_-bash: ./bitnami-jasperserver-3.5.0-0-linux-installer.bin: /lib/ld-linux.so.2: bad ELF interpreter: No such file or directory_ 

make sure the i386 version of ibstdc++ is installed:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># rpm -qa | grep ibstdc++
# yum install libstdc++
</pre>

**Starting and Stopping the JasperServer**

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ./catalina.sh start
# ./catalina.sh stop
</pre>

Once you have started the JasperServer point your browser to the following URL to access the JasperServer GUI

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">http://server1.example.co.za:8080/jasperserver/login.html
</pre>

[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/11/jasperserver.jpg" alt="jasperserver" title="jasperserver" width="496" height="328" class="aligncenter size-full wp-image-649" srcset="https://www.how2centos.com/wp-content/uploads/2009/11/jasperserver.jpg 496w, https://www.how2centos.com/wp-content/uploads/2009/11/jasperserver-300x198.jpg 300w" sizes="(max-width: 496px) 100vw, 496px" />](http://www.how2centos.com/wp-content/uploads/2009/11/jasperserver.jpg)  
Now your MySQL reporting can begin.