---
id: 2312
title: 'CentOS Warning: RPMDB altered outside of yum'
date: 2012-01-03T14:07:47+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2312
permalink: /centos-warning-rpmdb-altered-outside-of-yum/
dsq_thread_id:
  - "525039189"
slider_image:
  - ""
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
  - CentOS 6.0 Tutorials
---
Yum is an automatic updater and package installer/remover for rpm-based systems. It automatically computes dependencies and figures out what things should occur in order to safely install, remove, and update rpm packages. Yum also efficiently and easily retrieves information on any package installed or available in a repository to the installer.

When trying to update your server via the yum command you might see the following error / warning message:

Warning: RPMDB altered outside of yum.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install nethogs
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.cogentco.com
 * epel: mirror.cogentco.com
 * extras: mirror.vcu.edu
 * ius: mirror.rackspace.com
 * updates: centos.aol.com
Setting up Install Process
Resolving Dependencies
--> Running transaction check
---> Package nethogs.x86_64 0:0.8.0-1.el6 set to be updated
--> Finished Dependency Resolution

Dependencies Resolved


<pre>
====================================================================
 Package       Arch            Version          Repository     Size
====================================================================
Installing:
 nethogs      x86_64           0.8.0-1.el6      epel          28 k

Transaction Summary
====================================================================
Install       1 Package(s)
Upgrade       0 Package(s)
</pre>


<p>
  Total download size: 28 k<br />
  Installed size: 53 k<br />
  Is this ok [y/N]: y<br />
  Downloading Packages:<br />
  nethogs-0.8.0-1.el6.x86_64.rpm                                   |  28 kB     00:00<br />
  Running rpm_check_debug<br />
  Running Transaction Test<br />
  Transaction Test Succeeded<br />
  Running Transaction
</p>


<h4>
  Warning: RPMDB altered outside of yum.
</h4>


<p>
  Installing     : nethogs-0.8.0-1.el6.x86_64                                       1/1
</p>


<p>
  Installed:<br />
    nethogs.x86_64 0:0.8.0-1.el6
</p>


<p>
  Complete!
</p>


<h3>
  How do you fix this problem?
</h3>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# yum clean all
</pre>


<p>
  <a href="https://plus.google.com/112343473763635198843/about?rel=author">+George Rushby</a>
</p>