---
id: 923
title: Create Fedora 13 local Yum Repository
date: 2010-06-24T07:11:59+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=923
permalink: /create-fedora-13-local-yum-repository/
dsq_thread_id:
  - "110825486"
categories:
  - Fedora
tags:
  - Fedora 13
  - yum
---
<img loading="lazy" class="size-full wp-image-258 alignleft" title="yum" src="http://www.how2centos.com/wp-content/uploads/2009/06/yum.png" alt="yum" width="90" height="40" /><img loading="lazy" class="size-full wp-image-235 alignleft" title="fedora" src="http://www.how2centos.com/wp-content/uploads/2009/06/fedora.gif" alt="fedora" width="40" height="40" /> Fedora 13 (Goddard) is out and it&#8217;s time to add it to our arsenal of local yum repositories. We all know the importance of creating a local YUM repository on your LAN. Not only do you decrease the time it takes to download and install updates, you also decrease bandwidth usage.

<a href="https://fedoraproject.org/wiki/F13_one_page_release_notes" target="_blank">Fedora 13 (Goddard) release notes</a>

This How To will show you a simple yet effective way to create your local Fedora 13 YUM repository and client.

**TIP:** Distribute your Fedora YUM configuration via your [Puppet Master](http://www.how2centos.com/how-to-install-a-puppet-master-and-client-server-on-centos-52/)

<!--more-->

**Preliminary Note:**

I am using two Fedora 13 installations in this tutorial with server1.example.co.za configured with Apache httpd

* server1.example.co.za (IP 10.0.0.100): _Fedora 13 YUM Repository and httpd server_  
* server2.example.co.za (IP 10.0.0.102): _Fedora 13 YUM client_

**Configure _Fedora YUM repo and httpd server_ as follows:**

Create the following Directories:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mkdir -p /var/www/html/fedora/13/os/i386
# mkdir -p /var/www/html/fedora/13/os/x86_64
</pre>

Create a bash script that will rsync your local _YUM Repository server_ with your local YUM mirror (Make sure the mirror supports rsync).

Fedora Mirror list &#8211; <http://mirrors.fedoraproject.org/publiclist>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi fedora-repo-update.sh
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/bin/sh
# This script will create a local Fedora 13 mirror via Rsync
# Note: This script will download Fedora 13
#

rsync="rsync -avrt --bwlimit=256"

mirror=mirrors.kernel.org::mirrors/fedora

verlist="13"
archlist="i386 x86_64"
baselist="os"
local=/var/www/html/fedora/

for ver in $verlist
do
 for arch in $archlist
 do
  for base in $baselist
  do
    remote=$mirror/releases/$ver/Fedora/$arch/$base/
    $rsync $remote $local/$ver/$base/$arch/
  done
 done
done
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chmod 755 fedora-repo-update.sh
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

# Update Local YUM repo update from fedora.mirror.facebook.net
	 0 	  1 	  * 	  * 	  * 	   /path/to/fedora-repo-update.sh
</pre>

**Configure _Fedora YUM client_ servers as follows:**

Rename all existing yum repositories from \*.repo to \*.old

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/yum.repos.d/localFedora.repo
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">[base]
name=Fedora $releasever - $basearch
failovermethod=priority
baseurl=http://server1.example.co.za/fedora/$releasever/$basearch/os/
enabled=1
gpgcheck=0
</pre>

Test your setup by running a yum update on your client machine.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum update

Loading "fastestmirror" plugin
Loading mirror speeds from cached hostfile
* update: server1.example.co.za
* base: server1.example.co.za
</pre>