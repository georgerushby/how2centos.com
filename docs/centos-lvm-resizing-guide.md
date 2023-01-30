---
id: 1322
title: CentOS LVM Resize
date: 2010-11-03T10:20:24+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1322
permalink: /centos-lvm-resizing-guide/
dsq_thread_id:
  - "166318175"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
  - CentOS 5.8 Tutorials
tags:
  - CentOS
  - CentOS 5.5
---
Since the release of CentOS 5.5 we noticed that the default CentOS install assigns a considerable amount of its available storage space to Swap. If you don&#8217;t catch this during the installation don&#8217;t worry, Logical Volume Manager or LVM will rectify this post install.

LVM is a logical volume manager for the Linux kernel; it manages disk drives and similar mass-storage devices, in particular large ones. The term &#8220;volume&#8221; refers to a disk drive or partition thereof.  
<!--more-->

Before we start just some general housekeeping. The XEN virtual CentOS 5.5 server (base install) in this tutorial was assigned 10GB of storage with the default partitioning layout.

Let check the amount of disk space available on the file system.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup00-LogVol00
                      4.9G  2.4G  2.3G  51% /
/dev/xvda1             99M   23M   72M  25% /boot
tmpfs                 4.0G     0  4.0G   0% /dev/shm
</pre>

Let see the attributes of the logical volumes like size, read/write status, snapshot information

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># lvdisplay
  --- Logical volume ---
  LV Name                /dev/VolGroup00/LogVol00
  VG Name                VolGroup00
  LV UUID                oFXKGg-nBPo-Fk27-3ino-zaHZ-cEcv-fk0dS0
  LV Write Access        read/write
  LV Status              available
  # open                 1
  LV Size                5.00 GB
  Current LE             160
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

  --- Logical volume ---
  LV Name                /dev/VolGroup00/LogVol01
  VG Name                VolGroup00
  LV UUID                Ntffw7-dqPh-Rjhy-rWLv-BGA0-iGik-LoyHET
  LV Write Access        read/write
  LV Status              available
  # open                 1
  LV Size                4.88 GB
  Current LE             156
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1
</pre>

Let&#8217;s begin but first turning off swap on the Swap Logical Volume

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># swapoff /dev/VolGroup00/LogVol01
</pre>

Once swap is off let’s take the disk space we require.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># lvresize -L -4GB /dev/VolGroup00/LogVol01
  WARNING: Reducing active logical volume to 896.00 MB
  THIS MAY DESTROY YOUR DATA (filesystem etc.)
Do you really want to reduce LogVol01? [y/n]: y
  Reducing logical volume LogVol01 to 896.00 MB
  Logical volume LogVol01 successfully resized
</pre>

Now let’s add what we removed to the main Logical Volume.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># lvresize -L +4GB /dev/VolGroup00/LogVol00
  Extending logical volume LogVol00 to 9.00 GB
  Logical volume LogVol00 successfully resized
</pre>

Resize the File System

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># resize2fs -p /dev/VolGroup00/LogVol00
resize2fs 1.39 (29-May-2006)
Filesystem at /dev/VolGroup00/LogVol00 is mounted on /; on-line resizing required
Performing an on-line resize of /dev/VolGroup00/LogVol00 to 2359296 (4k) blocks.
The filesystem on /dev/VolGroup00/LogVol00 is now 2359296 blocks long.
</pre>

Rebuild the swap partition.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mkswap /dev/VolGroup00/LogVol01
Setting up swapspace version 1, size = 939520 kB
</pre>

Turn swap on.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># swapon /dev/VolGroup00/LogVol01
</pre>

Let’s check the amount of disk space available and LVM attributes to see if our changes took effect.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/VolGroup00-LogVol00
                      8.8G  2.4G  6.0G  28% /
/dev/xvda1             99M   23M   72M  25% /boot
tmpfs                 4.0G     0  4.0G   0% /dev/shm
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># lvdisplay
  --- Logical volume ---
  LV Name                /dev/VolGroup00/LogVol00
  VG Name                VolGroup00
  LV UUID                oFXKGg-nBPo-Fk27-3ino-zaHZ-cEcv-fk0dS0
  LV Write Access        read/write
  LV Status              available
  # open                 1
  LV Size                9.00 GB
  Current LE             288
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0

  --- Logical volume ---
  LV Name                /dev/VolGroup00/LogVol01
  VG Name                VolGroup00
  LV UUID                Ntffw7-dqPh-Rjhy-rWLv-BGA0-iGik-LoyHET
  LV Write Access        read/write
  LV Status              available
  # open                 1
  LV Size                896.00 MB
  Current LE             28
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:1
</pre>