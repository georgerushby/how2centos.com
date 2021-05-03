---
id: 661
title: Remus applied to the official Xen repository
date: 2009-11-12T11:02:09+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=661
permalink: /remus-applied-to-the-official-xen-repository/
dsq_thread_id:
  - "45085131"
categories:
  - CentOS 5.2 Tutorials
tags:
  - centos 5.4
  - fedora 11
  - remus
  - xen
---
The one problem with Xen is transparent high availability of your servers, sure you can snapshot an image but thats only of you shutdown the virtual machine. How excited was I when Remus announced that it has been applied to the official Xen repository, and is expected to be included with the next major release. Bookmark this page for a soon to be released how to install Xen with Remus support on CentOS 5.4!

**What is Remus?**

Remus provides transparent, comprehensive high availability to ordinary virtual machines running on the Xen virtual machine monitor. It does this by maintaining a completely up-to-date copy of a running VM on a backup server, which automatically activates if the primary server fails. Key features:

* The backup VM is an exact copy of the primary VM. When failure happens, it continues running on the backup host as if failure had never occurred.  
* The backup is completely up-to-date. Even active TCP sessions are maintained without interruption.  
* Protection is transparent. Existing guests can be protected without modifying them in any way.

For a full description and evaluation, see their NSDI paper.

Visit the project: http://dsg.cs.ubc.ca/remus/