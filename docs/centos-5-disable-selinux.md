---
id: 1600
title: CentOS 5 Disable SELinux
date: 2010-11-23T13:49:17+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1600
permalink: /centos-5-disable-selinux/
dsq_thread_id:
  - "177993037"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
---
NSA Security-Enhanced Linux (SELinux) is an implementation of a flexible mandatory access control architecture in the Linux operating system. The SELinux architecture provides general support for the enforcement of many kinds of mandatory access control policies, including those based on the concepts of Type Enforcement(R), Role Based Access Control, and Multi-Level Security. Background information and technical documentation about SELinux can be found at <a href="http://www.nsa.gov/research/selinux/index.shtml" target="_blank">http://www.nsa.gov/research/selinux/index.shtml</a>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># man selinux
</pre>

<!--more-->

**If you really need to disable SELinux on your system please consider the following:**

[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/11/warning.gif" alt="" title="warning" width="412" height="130" class="aligncenter size-full wp-image-1640" srcset="https://www.how2centos.com/wp-content/uploads/2010/11/warning.gif 412w, https://www.how2centos.com/wp-content/uploads/2010/11/warning-300x94.gif 300w" sizes="(max-width: 412px) 100vw, 412px" />](http://www.how2centos.com/wp-content/uploads/2010/11/warning.gif)

or you&#8217;re considering one of the alternates:

AppArmor <http://www.novell.com/linux/security/apparmor/>  
Bastille Linux <http://bastille-linux.sourceforge.net/>  
grsecurity <http://grsecurity.net/>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/selinux/config
</pre>

Change SELINUX=enforcing

<pre class="theme:github font:droid-sans-mono lang:vim decode:true\" ># This file controls the state of SELinux on the system.<br />
# SELINUX= can take one of these three values:<br />
#       enforcing - SELinux security policy is enforced.<br />
#       permissive - SELinux prints warnings instead of enforcing.<br />
#       disabled - SELinux is fully disabled.<br />
SELINUX=enforcing<br />
# SELINUXTYPE= type of policy in use. Possible values are:<br />
#       targeted - Only targeted network daemons are protected.<br />
#       strict - Full SELinux protection.<br />
SELINUXTYPE=targeted<br />
[/bash]</p>


<p>
  to SELINUX=disabled
</p>


<pre class="theme:github font:droid-sans-mono lang:vim decode:true\" >
# This file controls the state of SELinux on the system.<br />
# SELINUX= can take one of these three values:<br />
#       enforcing - SELinux security policy is enforced.<br />
#       permissive - SELinux prints warnings instead of enforcing.<br />
#       disabled - SELinux is fully disabled.<br />
SELINUX=disabled<br />
# SELINUXTYPE= type of policy in use. Possible values are:<br />
#       targeted - Only targeted network daemons are protected.<br />
#       strict - Full SELinux protection.<br />
SELINUXTYPE=targeted<br />
[/bash]</p>


<p>
  This will disable SELinux on your next reboot.
</p>