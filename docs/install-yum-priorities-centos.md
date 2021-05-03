---
id: 2741
title: Install yum priorities on CentOS
date: 2012-08-01T12:39:14+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2741
permalink: /install-yum-priorities-centos/
dsq_thread_id:
  - "788193163"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
  - CentOS 5.8 Tutorials
  - CentOS 6.0 Tutorials
  - CentOS 6.1 Tutorials
  - CentOS 6.2 Tutorials
---
The Yum Priorities plugin can be used to enforce ordered protection of repositories, by associating priorities to repositories.

The priorities plugin is a useful tool if properly configured, and used with an understanding of the functionality and a recognition of the limitations and potential issues. It can be used in conjunction with the &#8216;exclude&#8217; and/or &#8216;includepkg&#8217; options, as well as the &#8216;enabled=0&#8217; option to disable a repo by default. This can let you choose which packages a less important repo will supersede those of a more important one.

### Install Yum Priorities

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install yum-priorities
</pre>

<!--more-->

### Configure Yum Priorities

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/yum/pluginconf.d/priorities.conf
</pre>

Ensure the following lines exist

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >[main]
enabled=1
</pre>

Save and close the file

Open the CentOS base repository configuration file

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/yum.repos.d/CentOS-Base.repo
</pre>

Add the following text to the end of the base, updates and extras entries

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >priority=1
</pre>

Add the following line to the end of the centosplus, contrib entries

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >priority=2
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># CentOS-Base.repo
#
# The mirror system uses the connecting IP address of the client and the
# update status of each mirror to pick mirrors that are updated to and
# geographically close to the client.  You should use this for CentOS updates
# unless you are manually picking other mirrors.
#
# If the mirrorlist= does not work for you, as a fall back you can try the
# remarked out baseurl= line instead.
#
#

[base]
name=CentOS-$releasever - Base
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
priority=1

#released updates
[updates]
name=CentOS-$releasever - Updates
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
priority=1

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras
#baseurl=http://mirror.centos.org/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
priority=1

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus
#baseurl=http://mirror.centos.org/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
priority=2

#contrib - packages by Centos Users
[contrib]
name=CentOS-$releasever - Contrib
mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib
#baseurl=http://mirror.centos.org/centos/$releasever/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
priority=2
</pre>