---
id: 1244
title: Installing Webmin on CentOS 5.5 Tutorial
date: 2010-10-22T21:23:47+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1244
permalink: /installing-webmin-on-centos-5-5-tutorial/
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
  - "160444170"
categories:
  - CentOS 5.5 Tutorials
tags:
  - CentOS 5.5
  - Webmin
---
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/10/webmin-e1287775060663.png" alt="Webmin" title="Webmin Logo" width="150" height="40" class="alignleft size-full wp-image-1254" /></a><img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="centos" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" /> This brief yet effective tutorial will instruct you on how to install CentOS 5.5 with Webmin, a web-based interface for system administration for Linux 

The assumption for this CentOS 5.5 Webmin tutorial is that you are running as root and have a basic understanding of the software required but if you follow this tutorial you should be able to complete the task successfully.

Learn more about Webmin: <http://www.webmin.com/>  
<!--more-->

  
**Preliminary Note:**

I am using a CentOS 5.5 32bit server in this tutorial:

* centos01.how2centos.com (IP 10.0.0.3)

Let&#8217;s begin by installing the GPG key with which the packages are signed

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># rpm &#45;&#45;import http://www.webmin.com/jcameron-key.asc
</pre>

Add the Webmin YUM Repository 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cat > /etc/yum.repos.d/webmin.repo &#60;&#60; EOF
[Webmin]
name=Webmin Distribution Neutral
baseurl=http://download.webmin.com/download/yum
enabled=1
EOF
</pre>

or if you prefer using an editor

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/yum.repos.d/webmin.repo
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >[Webmin]
name=Webmin Distribution Neutral
baseurl=http://download.webmin.com/download/yum
enabled=1
</pre>

Now to install Webmin

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install webmin
</pre>

Finally browse to your machine on port 10000

http://centos01.how2centos.com:10000

**Video: Installing Webmin on CentOS 5.5 Tutorial**