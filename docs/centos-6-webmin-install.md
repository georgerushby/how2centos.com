---
id: 2030
title: Webmin CentOS 6 Install
date: 2011-07-23T21:49:30+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2030
permalink: /centos-6-webmin-install/
dsq_thread_id:
  - "366488514"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 6.0 Tutorials
---
This brief yet effective tutorial will instruct you on how to install <a href="http://www.webmin.com/" title="Webmin" target="_blank">Webmin</a>, a web-based interface for system administration for Linux, on CentOS 6 

The assumption for this Webmin, CentOS 6 tutorial is that you are running as root and have a basic understanding of the software required but if you follow this tutorial you should be able to complete the task successfully.

Learn more about Webmin: <a href="http://www.webmin.com/" title="Webmin Home Page" target="_blank">http://www.webmin.com/</a>  
<!--more-->

### Install the Webmin YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># cat > /etc/yum.repos.d/webmin.repo &lt;&lt; EOF<br />[Webmin]<br />name=Webmin Distribution Neutral<br />#baseurl=http://download.webmin.com/download/yum<br />mirrorlist=http://download.webmin.com/download/yum/mirrorlist<br />enabled=1<br />EOF</pre>



### Add the GPG Key and Install Webmin



<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm --import http://www.webmin.com/jcameron-key.asc</pre>

\# yum install webmin

or if you prefer using an editor

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/yum.repos.d/webmin.repo
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >[Webmin]
name=Webmin Distribution Neutral
#baseurl=http://download.webmin.com/download/yum
mirrorlist=http://download.webmin.com/download/yum/mirrorlist
enabled=1
</pre>

### Installing Webmin, CentOS 6

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install webmin
</pre>

Finally browse to your machine on port 10000

http://centos01.how2centos.com:10000