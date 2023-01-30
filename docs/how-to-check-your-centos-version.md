---
id: 1776
title: How to check your CentOS version
date: 2011-01-25T09:15:36+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1776
permalink: /how-to-check-your-centos-version/
dsq_thread_id:
  - "215562792"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.2 Tutorials
  - CentOS 5.3 Tutorials
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 6.0 Tutorials
---
Most Red Hat-based distributions, like CentOS, should have a file called redhat-release which will contain the CentOS version.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cat /etc/redhat-release
CentOS release 5.5 (Final)
</pre>

or

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># rpm -q centos-release
centos-release-5-5.el5.centos.1
</pre>

and finally

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># lsb_release -i
Distributor ID: CentOS
# lsb_release -r
Release: 5.5
# lsb_release -d
Description: CentOS release 5.5 (Final) 
</pre>