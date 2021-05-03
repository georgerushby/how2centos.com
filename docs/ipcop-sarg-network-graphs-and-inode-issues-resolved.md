---
id: 280
title: 'IPCop: SARG, Network Graphs and iNode issues resolved'
date: 2009-07-27T21:06:08+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=280
permalink: /ipcop-sarg-network-graphs-and-inode-issues-resolved/
dsq_thread_id:
  - "43376971"
categories:
  - MISC
tags:
  - IPCop
  - sarg
---
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/07/ipcop.jpg" alt="ipcop" title="ipcop" width="55" height="58" class="alignleft size-full wp-image-283" />  
After many a moon I noticed that IPCop wasn&#8217;t behaving like it should, so a quick glance at the System Status Graphs showed that the inodes usage was 100%. 

After laboriously troubleshooting the various possibilities I found that the SARG for IPCop I installed and then &#8220;hacked&#8221;, to work with the latest version, was the culprit. This caused the inodes not to be cleaned up correctly, which in turn slowly but surely filled the partition. 

<!--more-->

After removing SARG for IPCop everything returned to normal with 1% inodes usage.

Remove SARG from your IPCop installation:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"><pre>
#  /var/ipcop/sarg/setup/setup -u
</pre>


<p>
  However, it doesn&#8217;t stop there.
</p>


<p>
  After removing SARG from the IPCop box the network traffic graphs stopped working. I ran a couple of commands from the makegraphs script which turned up this error:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# /usr/bin/squid-graph -o=/home/httpd/html/sgraph --tcp-only &lt; /var/log/squid/access.log

Can't load '/usr/lib/perl5/site_perl/5.8.5/i386-linux/auto/GD/GD.so' for module GD: libfreetype.so.6: cannot open shared object file: No such file or directory at /usr/lib/perl5/5.8.5/i386-linux/DynaLoader.pm line 230.
at /usr/bin/squid-graph line 16
Compilation failed in require at /usr/bin/squid-graph line 16.
BEGIN failed--compilation aborted at /usr/bin/squid-graph line 16.
</pre>


<p>
  So removing SARG removed the following files that are required for network traffic graphs to function:
</p>


<p>
  usr/lib/libgd.so.2<br />
  usr/lib/libgd.so.2.0.0<br />
  usr/lib/libgd.so<br />
  usr/local/lib/libfreetype.so<br />
  usr/local/lib/libfreetype.so.6.3.7<br />
  usr/local/lib/libfreetype.so.6
</p>


<p>
  To recover these files download the SARG package, extract the archives and copy the relevant files back to their correct directories.
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# cd ~/
# tar zxvf sarg_ipcop_1.4.11.tar.gz
# cd sarg/
# tar zxvf files.tar.gz
# cd var/ipcop/sarg/setup/
# tar zxvf files.tar.gz
# cp usr/lib/libgd.so.2 /usr/lib/
# cp usr/lib/libgd.so.2.0.0 /usr/lib/
# cp usr/lib/libgd.so /usr/lib/
# cp usr/local/lib/libfreetype.so /usr/local/lib/
# cp usr/local/lib/libfreetype.so.6.3.7 /usr/local/lib/
# cp usr/local/lib/libfreetype.so.6 /usr/local/lib/
# cd ~/
# rm -Rf sarg sarg_ipcop_1.4.11.tar.gz
</pre>


<p>
  After 10 minutes your should see the network traffic graphs come to life.
</p>


<p>
  <strong><em>Moral of the story "Don't install a plugin that is not compatible with your version of IPCop"</em></strong>
</p>