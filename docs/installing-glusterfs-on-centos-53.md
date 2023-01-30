---
id: 110
title: Installing GlusterFS on CentOS 5.3
date: 2009-09-29T12:00:58+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=110
permalink: /installing-glusterfs-on-centos-53/
dsq_thread_id:
  - "43376992"
categories:
  - CentOS 5.3 Tutorials
tags:
  - centos 5.3
  - glusterfs
---
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/03/gluster.png" alt="gluster" title="gluster" width="192" height="40" class="alignleft size-full wp-image-276" /><img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="centos" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />GlusterFS is a clustered file-system capable of scaling to several peta-bytes. It aggregates various storage bricks over Infiniband RDMA or TCP/IP interconnect into one large parallel network file system. GlusterFS is one of the most sophisticated file system in terms of features and extensibility.  
<!--more-->

  
  
**Prerequisites**

We need to add the DAG yum repo for fuse and dependencies

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># rpm -Uhv http://apt.sw.be/redhat/el5/en/x86_64/rpmforge/RPMS//rpmforge-release-0.3.6-1.el5.rf.x86_64.rpm
</pre>

**Install GlusterFS dependencies**

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install make gcc gcc-c++ sshfs build-essential flex bison byacc vim wget kernel-xen-devel fuse dkms dkms-fuse openib libibverbs iftop
</pre>

Download the latest GlusterFS from the repo http://ftp.gluster.com/pub/gluster/glusterfs/2.0/LATEST/CentOS/ and install !

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://ftp.gluster.com/pub/gluster/glusterfs/2.0/LATEST/CentOS/glusterfs-client-2.0.6-1.el5.x86_64.rpm
# wget http://ftp.gluster.com/pub/gluster/glusterfs/2.0/LATEST/CentOS/glusterfs-common-2.0.6-1.el5.x86_64.rpm
# wget http://ftp.gluster.com/pub/gluster/glusterfs/2.0/LATEST/CentOS/glusterfs-devel-2.0.6-1.el5.x86_64.rpm
# wget http://ftp.gluster.com/pub/gluster/glusterfs/2.0/LATEST/CentOS/glusterfs-server-2.0.6-1.el5.x86_64.rpm
# yum install ./* --nogpgcheck
</pre>

Do a yum update, to get the new kernel and updates, after the reboot we&#8217;ll continue with fuse

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum update
</pre>

REBOOT so that changes may take affect &#8230;

**Install FUSE with glusterfs patches** 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://ftp.gluster.com/pub/gluster/glusterfs/fuse/fuse-2.7.4glfs11.tar.gz
# tar -zxvf fuse-2.7.4glfs11.tar.gz
# cd fuse-2.7.4glfs11
# ./configure
# make 
# make install
</pre>

**Create the partition && reclaim space reserved for super user, we set it to 1% instead of 5%**

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mkfs.ext3 -I 256 /dev/sda1
# tune2fs -m 1 /dev/sda1
</pre>

Add the GlusterFS partition, mine is /dev/sda1 so add below to /etc/fstab

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >/dev/sda1		/data			ext3	defaults	0 0
</pre>

and create the mount point e.g 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mkdir /data
</pre>

and mount all the shares with

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mount -a
</pre>

**Time to configure Glusterfs (the server) (should be on all nodes)**

Create Gluster State folders

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mkdir /data/export
# mkdir /data/export-ns
# mkdir -p /etc/glusterfs/
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/glusterfs/glusterfsd.vol
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >volume posix
   type storage/posix
   option directory /data/export
 end-volume  
 
 volume locks
   type features/locks
   subvolumes posix
 end-volume 
 
 volume brick
   type performance/io-threads
   option thread-count 8
   subvolumes locks
 end-volume
 
 volume posix-ns
   type storage/posix
   option directory /data/export-ns
 end-volume
  
 volume locks-ns
   type features/locks
   subvolumes posix-ns
 end-volume
 
 volume brick-ns
   type performance/io-threads
   option thread-count 8
   subvolumes locks-ns
 end-volume
 
 volume server
   type protocol/server
   option transport-type tcp
   option auth.addr.brick.allow *
   option auth.addr.brick-ns.allow *
   subvolumes brick brick-ns
 end-volume
</pre>

Now configure the client (should be on all nodes)

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/glusterfs/glusterfs.vol
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >### Add client feature and attach to remote subvolume of server1
 volume brick1
  type protocol/client
  option transport-type tcp/client
  option remote-host 10.0.0.5       # IP address of the remote brick
  option remote-subvolume brick         # name of the remote volume
 end-volume
   
 ### Add client feature and attach to remote subvolume of server2
 volume brick2
  type protocol/client
  option transport-type tcp/client
  option remote-host 10.0.0.6       # IP address of the remote brick
  option remote-subvolume brick         # name of the remote volume
 end-volume
 
 ### Add client feature and attach to remote subvolume of server3
 volume brick3
  type protocol/client
  option transport-type tcp/client
  option remote-host 10.0.0.8       # IP address of the remote brick
  option remote-subvolume brick         # name of the remote volume
 end-volume
 
 ### Add client feature and attach to remote subvolume of server4
 volume brick4
  type protocol/client
  option transport-type tcp/client
  option remote-host 10.0.0.9       # IP address of the remote brick
  option remote-subvolume brick         # name of the remote volume
 end-volume
  
 ### The file index on server1
 volume brick1-ns
  type protocol/client
  option transport-type tcp/client
  option remote-host 10.0.0.5       # IP address of the remote brick
  option remote-subvolume brick-ns      # name of the remote volume
 end-volume
  
 ### The file index on server2
 volume brick2-ns
  type protocol/client
  option transport-type tcp/client
  option remote-host 10.0.0.6       # IP address of the remote brick
  option remote-subvolume brick-ns      # name of the remote volume
 end-volume
 
 ### The file index on server3
 volume brick3-ns
  type protocol/client
  option transport-type tcp/client
  option remote-host 10.0.0.8       # IP address of the remote brick
  option remote-subvolume brick-ns      # name of the remote volume
 end-volume
 
 ### The file index on server4
 volume brick4-ns
  type protocol/client
  option transport-type tcp/client
  option remote-host 10.0.0.9       # IP address of the remote brick
  option remote-subvolume brick-ns      # name of the remote volume
 end-volume
  
 #The replicated volume with data
 volume afr1
  type cluster/afr
  subvolumes brick1 brick2
 end-volume
 
 #The replicated volume with data
 volume afr2
  type cluster/afr
  subvolumes brick3 brick4
 end-volume
  
 #The replicated volume with indexes
 volume afr-ns
  type cluster/afr
  subvolumes brick1-ns brick2-ns brick3-ns brick4-ns
 end-volume
  
 #The unification of all afr volumes (used for > 2 servers)
 volume unify
   type cluster/unify
   option scheduler rr # round robin
   option namespace afr-ns
   subvolumes afr1 afr2
 end-volume
</pre>

We are going to mount the glusterfs disk to /mnt/glusterfs/ if /mnt/ is empty, do the following

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mkdir -p  /mnt/glusterfs/
</pre>

Disable standard Glusterfsd startup

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chkconfig glusterfsd off 
</pre>

now add these lines to /etc/rc.local

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># /usr/sbin/glusterfs --disable-direct-io-mode --volfile=/etc/glusterfs/glusterfsd.vol 
# /usr/sbin/glusterfs --disable-direct-io-mode --volfile=/etc/glusterfs/glusterfs.vol /mnt/glusterfs/
</pre>

Reboot one last time and your done .. 

df -h should look similar to,

<pre class="brush: bash">Filesystem                               Size   Used  Avail  Use%  Mounted on
glusterfs#/etc/glusterfs/glusterfs.vol   445G   12G   411G   3%    /mnt/glusterfs/
</pre>

<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/09/gluster.png" alt="gluster" title="gluster" width="495" height="332" class="aligncenter size-full wp-image-412" srcset="https://www.how2centos.com/wp-content/uploads/2009/09/gluster.png 727w, https://www.how2centos.com/wp-content/uploads/2009/09/gluster-300x200.png 300w" sizes="(max-width: 495px) 100vw, 495px" />