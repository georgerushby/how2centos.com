---
id: 894
title: Enabling Multimedia support on CentOS 5.5
date: 2010-06-01T12:42:34+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=894
permalink: /enabling-multimedia-support-on-centos-5-5/
dsq_thread_id:
  - "103100391"
categories:
  - CentOS 5.5 Tutorials
tags:
  - audio
  - CentOS
  - multimedia
  - video
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif)The CentOS base repository has no way to install the codecs and utilities we need to be able to play an MP3 or watch a DIVX movie. In this guide I will show you how to get all the multimedia support you want, as well as being able to use flash to view flash enabled websites.

Firstly we need to install the RPMForge repository, so that we can get access to all the codecs and applications we need. The repository comes in two flavors, namely 64-bit and 32-bit. Make sure you select the correct repository to install. When in doubt, select 32-bit.

For 32-bit installations (i.e. CentOS 5.x 32-bit installed), enter the following into your console/terminal:  
<!--more-->

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.1-1.el5.rf.i386.rpm
# su -c 'rpm -Uvh rpmforge-release-0.5.1-1.el5.rf.i386.rpm'
</pre>

Enter your root password to complete the installation.

For 64-bit installations (i.e. CentOS 5.x 64-bit installed), enter the following into your console/terminal::

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.1-1.el5.rf.x86_64.rpm
# su -c 'rpm -Uvh rpmforge-release-0.5.1-1.el5.rf.x86_64.rpm'
</pre>

Enter your root password to complete the installation.

Next we need to install the Macromedia repository which will give us the ability to install the latest flash player. Enter the following into your console/terminal:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># su -c 'rpm -Uhv http://linuxdownload.adobe.com/adobe-release/adobe-release-i386-1.0-1.noarch.rpm'
</pre>

Enter your root password to complete the installation.

The next thing we need to do is install all the multimedia applications that we want to use. Enter the following into your console/terminal:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># su -c 'yum -y install libdvdcss libdvdread libdvdplay libdvdnav lsdvd mplayerplug-in mplayer mplayer-gui compat-libstdc++-33 flash-plugin gstreamer-plugins-bad gstreamer-plugins-ugly'
</pre>

Enter your root password to complete the installation.

Now we need to install all the codecs we want to use, luckily this step provides all the codecs we will ever need. Enter the following into your console/terminal:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget www1.mplayerhq.hu/MPlayer/releases/codecs/mplayer-codecs-20061022-1.i386.rpm
# su -c 'rpm -ivh mplayer-codecs-20061022-1.i386.rpm'
# wget www1.mplayerhq.hu/MPlayer/releases/codecs/mplayer-codecs-extra-20061022-1.i386.rpm
# su -c 'rpm -ivh mplayer-codecs-extra-20061022-1.i386.rpm'
</pre>

Enter your root password to complete the installation.

Well done! You should now have all the multimedia support you would ever want. Double click your MP3 or Movie to start having some fun.

Thank you for reading this guide, please leave me some comments.