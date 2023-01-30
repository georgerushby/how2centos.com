---
id: 143
title: Installing FFmpeg on CentOS 5.x
date: 2009-04-14T21:35:58+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=143
permalink: /installing-ffmpeg-on-centos/
dsq_thread_id:
  - "44472054"
categories:
  - CentOS 5.2 Tutorials
  - CentOS 5.3 Tutorials
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
  - CentOS 5.8 Tutorials
tags:
  - amr
  - centos 5.3
  - ffmpeg
  - php 5.2.9
---
FFmpeg is a complete, cross-platform solution to record, convert and stream audio and video. It includes libavcodec &#8211; the leading audio/video codec library.

### Install the RPMForge i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.5.2-2.el5.rf.i386.rpm</pre>

### Installing FFmpeg on CentOS

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install ffmpeg ffmpeg-devel ffmpeg-libpostproc opencore-amr
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ffmpeg --help | more
FFmpeg version 0.6.1, Copyright (c) 2000-2010 the FFmpeg developers
  built on Dec  4 2010 15:35:31 with gcc 4.1.2 20080704 (Red Hat 4.1.2-48)
  configuration: --prefix=/usr --libdir=/usr/lib64 --shlibdir=/usr/lib64 --mandir=/usr/share/man --incdir=/usr/include --disable-avisynth --extra-cflags='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector --param=ssp-buffer-size=4 -m64 -mtune=generic -fPIC' --enable-avfilter --enable-avfilter-lavf --enable-libdirac --enable-libfaac --enable-libfaad --enable-libfaadbin --enable-libgsm --enable-libmp3lame --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libx264 --enable-gpl --enable-nonfree --enable-postproc --enable-pthreads --enable-shared --enable-swscale --enable-vdpau --enable-version3 --enable-x11grab
</pre>

### Optional: Installing FFmpeg-php

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://garr.dl.sourceforge.net/sourceforge/ffmpeg-php/ffmpeg-php-0.6.0.tbz2
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># tar -xjf ffmpeg-php-0.6.0.tbz2
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd ffmpeg-php-0.6.0
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># phpize
# ./configure
# make
# make install
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># echo "extension=ffmpeg.so" > /etc/php.d/ffmpeg.ini
</pre>

If you get the following error when running the command make to compile FFmpeg:

**make: \*** [ffmpeg_frame.lo] Error 1**

In the version 0.6.0 of ffmpeg-php you will need to modify the file: ffmpeg\_frame.c and replace every instance of PIX\_FMT\_RGBA32 with PIX\_FMT_RGB32

Using Linux text editor, vi run the following commands:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi ffmpeg_frame.c
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">:%s/PIX_FMT_RGBA32/PIX_FMT_RGB32
:wq!
</pre>

Restart the compile and install FFmpeg-Php:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># phpize
# ./configure
# make
# make install
</pre>