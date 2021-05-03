---
id: 2261
title: Remove all i386 packages from CentOS 5 x86_64
date: 2011-10-29T11:27:55+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2261
permalink: /removing-all-i386-packages-from-centos-5-x86_64-server/
dsq_thread_id:
  - "456094129"
slider_image:
  - ""
categories:
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
---
A quick solution to duplicate packet installation. This tutorial will show you how to remove all i386 packages from CentOS 5 x86_64 server

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># yum remove \*.i\?86
</pre>