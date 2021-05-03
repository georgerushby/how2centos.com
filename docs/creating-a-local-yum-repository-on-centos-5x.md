---
id: 75
title: Creating A Local Yum Repository on CentOS 5.x
date: 2009-03-14T12:00:03+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=75
permalink: /creating-a-local-yum-repository-on-centos-5x/
dsq_thread_id:
  - "44438349"
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
categories:
  - CentOS 5.2 Tutorials
tags:
  - centos 5.2
  - yum
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="centos" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif)Reducing the costs of I.T without reducing the functionally of your systems is one of the major obstacles to overcome. One of these costs is bandwidth, especially in South Africa.

One of the first bandwidth saving tips any organization should know is the importance of creating a local YUM repository on your LAN. Not only do you decrease the time it takes to download and install updates, you also decrease bandwidth usage. This saving will definitely please the suites of any organization.

This How To show&#8217;s you a simple yet effective way of setting up your local YUM server and client. 

**TIP:** Distribute your YUM configuration via your [Puppet Master](http://www.how2centos.com/how-to-install-a-puppet-master-and-client-server-on-centos-52/)

<!--more-->

**Preliminary Note:**

We are using two empty CentOS 5.2 servers in this tutorial:

* server1.example.co.za (IP 10.0.0.100): _YUM Repo server_  
* server2.example.co.za (IP 10.0.0.102): _YUM client_

**Configure _YUM repo server_ as follows:**

Create the following Directories:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mkdir -p /var/www/html/centos/5.3/os/i386
# mkdir -p /var/www/html/centos/5.3/updates/i386
# mkdir -p /var/www/html/centos/5.3/os/x86_64
# mkdir -p /var/www/html/centos/5.3/updates/x86_64
# mkdir -p /var/www/html/centos/5.2/os/i386
# mkdir -p /var/www/html/centos/5.2/updates/i386
# mkdir -p /var/www/html/centos/5.2/os/x86_64
# mkdir -p /var/www/html/centos/5.2/updates/x86_64
# mkdir -p /var/www/html/centos/5/os/i386
# mkdir -p /var/www/html/centos/5/updates/i386
# mkdir -p /var/www/html/centos/5/os/x86_64
# mkdir -p /var/www/html/centos/5/updates/x86_64
</pre>

Create a bash script that will rsync your local _YUM Repo server_ with your local YUM mirror (Internet Solutions &#8211; ftp.is.co.za).  
CentOS Mirror list &#8211; <http://www.centos.org/modules/tinycontent/index.php?id=30>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi yum-repo-update.sh
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/bin/sh

rsync="rsync -avrt --bwlimit=256"

mirror=ftp.is.co.za::IS-Mirror/centos

verlist="5 5.2 5.3"
archlist="i386 x86_64"
baselist="os updates"
local=/var/www/html/centos/

for ver in $verlist
do
 for arch in $archlist
 do
  for base in $baselist
  do
    remote=$mirror/$ver/$base/$arch/
    $rsync $remote $local/$ver/$base/$arch/
  done
 done
done
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chmod 755 yum-repo-update.sh
</pre>

Add the bash script to your crontab to update your local repository every night (01H00 in this case)

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># crontab -e
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#       minute (0-59),
#       |       hour (0-23),
#       |       |       day of the month (1-31),
#       |       |       |       month of the year (1-12),
#       |       |       |       |       day of the week (0-6 with 0=Sunday).
#       |       |       |       |       |       commands
# -----------[ cron jobs  ]------------ #

# Update Local YUM repo update from ftp.is.co.za
		0 	  1 	  * 	  * 	  * 	   /path/to/yum-repo-update.sh
</pre>

**Configure _YUM client_ servers as follows:**

Rename all existing yum repositories from \*.repo to \*.old

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/yum.repos.d/localCentOS-Base.repo
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >[base]
name=CentOS-$releasever - Base
baseurl=http://server1.example.co.za/centos/$releasever/os/$basearch/
gpgcheck=0

[update]
name=CentOS-$releasever - Updates
baseurl=http://server1.example.co.za/centos/$releasever/updates/$basearch/
gpgcheck=0

[extras]
name=CentOS-$releasever - Extras
mirrorlist=http://server1.example.co.za/?release=$releasever&arch=$basearch&repo=extras
gpgcheck=0
enabled=0
</pre>

Test your setup by running a yum update on your client machine.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum update

Loading "fastestmirror" plugin
Loading mirror speeds from cached hostfile
 * update: server1.example.co.za
 * base: server1.example.co.za
</pre>