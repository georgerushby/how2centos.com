---
id: 1833
title: Installing DenyHosts on CentOS 5.6
date: 2011-04-14T11:23:08+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1833
permalink: /installing-denyhosts-on-centos-5-6/
dsq_thread_id:
  - "279115056"
categories:
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
---
If you have a CentOS server with public IP address, then the server is probably vulnerable to attacks from outside. Brute force attacks are usually done by forcing entry [log in] with the variation of the username and password repeatedly.

### What is <a target="_blank" href="http://denyhosts.sourceforge.net/">DenyHosts</a>?

<a target="_blank" href="http://denyhosts.sourceforge.net/">DenyHosts</a> is a script intended to be run by Linux system administrators to help thwart SSH server attacks (also known as dictionary based attacks and brute force attacks).

If you&#8217;ve ever looked at your ssh log (/var/log/secure on CentOS) you may be alarmed to see how many hackers attempted to gain access to your server. Hopefully, none of them were successful (but then again, how would you know?). Wouldn&#8217;t it be better to automatically prevent that attacker from continuing to gain entry into your system?

Read more on the <a target="_blank" href="http://denyhosts.sourceforge.net/">DenyHosts</a> website: <a target="_blank" href="http://denyhosts.sourceforge.net/">http://denyhosts.sourceforge.net/</a>  
<!--more-->

### Installing DenyHosts on CentOS 5.6

### Install the EPEL i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm</pre>

Now install DenyHosts

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install denyhosts  
</pre>

Finally add it to start-up and then start it up.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chkconfig denyhosts on
# service denyhosts start
</pre>

Any further configuration can be done by editing the configuration file /etc/denyhosts.conf

You can watch IP attackers get blacklisted in the /etc/host.deny

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># tail -f /etc/hosts.deny  
</pre>