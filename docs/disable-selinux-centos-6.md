---
id: 2219
title: Disable SELinux CentOS 6
date: 2011-10-17T17:33:13+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2219
permalink: /disable-selinux-centos-6/
dsq_thread_id:
  - "446005175"
slider_image:
  - ""
categories:
  - CentOS 6.0 Tutorials
  - CentOS 6.2 Tutorials
---
You need to be aware that by disabling SELinux you will be removing a security mechanism on your CentOS system. Think about this carefully, and if your system is on the Internet and accessed by the public, then think about it some more. 

Applications should be fixed to work with SELinux, rather than disabling the OS security mechanism.

You could even switch to Permissive mode where every operation is allowed. Operations that would be denied are allowed and a message is logged identifying that it would be denied.

**If you really need to disable SELinux on CentOS 6 please consider the following:**

<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/11/warning.gif" alt="SELinux Warning" title="warning" width="412" height="130" class="aligncenter size-full wp-image-1640" srcset="https://www.how2centos.com/wp-content/uploads/2010/11/warning.gif 412w, https://www.how2centos.com/wp-content/uploads/2010/11/warning-300x94.gif 300w" sizes="(max-width: 412px) 100vw, 412px" /> 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/selinux/config
</pre>

Change SELINUX=enforcing

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
# SELINUXTYPE= can take one of these two values:
#     targeted - Targeted processes are protected,
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
</pre>

to SELINUX=disabled

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#       enforcing - SELinux security policy is enforced.
#       permissive - SELinux prints warnings instead of enforcing.
#       disabled - SELinux is fully disabled.
SELINUX=disabled
# SELINUXTYPE= type of policy in use. Possible values are:
#       targeted - Only targeted network daemons are protected.
#       strict - Full SELinux protection.
SELINUXTYPE=targeted
</pre>

This will disable SELinux on your next reboot.