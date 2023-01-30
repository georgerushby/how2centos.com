---
id: 196
title: Installing and upgrading to PHP 5.2 on CentOS and RedHat Linux 5.3 x86_64
date: 2009-05-14T11:21:10+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=196
permalink: /installing-and-upgrading-to-php-52-on-centos-and-red-hat-linux-53-x86_64/
dsq_thread_id:
  - "44446761"
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
  - CentOS 5.3 Tutorials
tags:
  - centos 5.3
  - php 5.2.9
---
<img loading="lazy" class="alignleft size-full wp-image-226" title="php" src="http://www.how2centos.com/wp-content/uploads/2009/05/php.gif" alt="php" width="75" height="40" /><img loading="lazy" class="alignleft size-full wp-image-225" title="centos" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="centos" width="42" height="40" />This Linux installing guide will walk you through installing PHP 5.2 (Supports the Kohana Framework) with APC, Memcached, ImageMagick and FFmpeg with AMR (3gp support) on CentOS and RHEL

_note: This upgrade also repairs the error_

<pre>"Warning: preg_match(): Compilation failed: support for \P, \p, and \X has not been compiled at offset 2 in file: /var/www/kohana_2.3.1/system/core/utf8.php on line: 65"</pre>

<!--more-->

  
**Preliminary Note:**

I am using a CentOS 5.2 x86_64 server with the [RPM Forge YUM repo](http://dag.wieers.com/rpm/FAQ.php#B) enabled in this tutorial:

* server1.example.co.za (IP 10.0.0.100)

**House Keeping**

Edit yum.conf and add the &#8216;exclude&#8217; switch to the configuration and this will eliminate 32bit RPM&#8217;s from being installed. You can however ignore this step if you&#8217;re installing on a 32bit CentOS or RHEL system.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/yum.conf</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">[main]
exclude=*.i386 *.i586 *.i686</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum remove openssl.i686 glibc.i686</pre>

If you haven&#8217;t installed the EPEL, IUS and RPMForge CentOS and RHEL repository, here&#8217;s how:

### Install the EPEL i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm</pre>

### Install the RPMForge i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.5.2-2.el5.rf.i386.rpm</pre>

### Install the IUS i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/i386/ius-release-1.0-10.ius.el5.noarch.rpm</pre>

Next we install the various packages that will be required for our PHP Linux installation

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install php52-pear php52 php52-cli php52-common php52-devel php52-gd php52-mbstring php52-mcrypt php52-mysql php52-pdo php52-soap php52-xml php52-xmlrpc php52-bcmath php52-pecl-apc php52-pecl-memcache</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install ImageMagick ImageMagick-devel memcached zlib-devel php-pear httpd-devel gcc mysql-devel libxslt-devel re2c ffmpeg ffmpeg-devel lame amrnd
# chkconfig memcached on</pre>

**_NB._** Don&#8217;t forget to read through the installing guide for [FFmpeg with AMR](http://www.how2centos.com/installing-ffmpeg-with-amr-on-centos-5x-64-bit/) support before continuing.

PECL install the relevant .so modules required by PHP

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># pecl install memcache

# pecl install apc

# pecl install PDO

# pecl install pdo_mysql

# pecl install imagick

# pecl install xslcache</pre>

and download the source and install the .so files not available via PECL

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://garr.dl.sourceforge.net/sourceforge/ffmpeg-php/ffmpeg-php-0.6.0.tbz2
# tar -xjf ffmpeg-php-0.6.0.tbz2
# cd ffmpeg-php-0.6.0
# phpize
# ./configure
# make
# make install</pre>

Create inclusion files for all the .so modules

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># echo "extension=xslcache.so" &gt; /etc/php.d/xslcache.ini

# echo "extension=apc.so" &gt; /etc/php.d/apc.ini

# echo "extension=pdo.so" &gt; /etc/php.d/pdo.ini

# echo "extension=pdo_mysql.so" &gt; /etc/php.d/pdo_mysql.ini

# echo "extension=memcache.so" &gt; /etc/php.d/memcache.ini

# echo "extension=mcrypt.so" &gt; /etc/php.d/mcrypt.ini

# echo "extension=imagick.so" &gt; /etc/php.d/imagick.ini

# echo "extension=ffmpeg.so" &gt; /etc/php.d/ffmpeg.ini</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /var/www/html/phpinfo.php</pre>

<pre lang="php">&lt;?php

// Show all information, defaults to INFO_ALL
phpinfo();

// Show just the module information.
// phpinfo(8) yields identical results.
phpinfo(INFO_MODULES);

?&gt;</pre>

Restart httpd/apache and point your browser at http://your-web-server/phpinfo.php and confirm your installation.