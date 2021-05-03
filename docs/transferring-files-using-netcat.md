---
id: 2884
title: Transferring Files using Netcat
date: 2014-03-04T15:42:20+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2884
permalink: /transferring-files-using-netcat/
dsq_thread_id:
  - "2357828413"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS
  - CentOS 5.2 Tutorials
  - CentOS 5.3 Tutorials
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
  - CentOS 5.8 Tutorials
  - CentOS 6.0 Tutorials
  - CentOS 6.1 Tutorials
  - CentOS 6.2 Tutorials
  - CentOS 6.3 Tutorials
  - CentOS 6.4 Tutorials
  - Fedora
  - MISC
---
<a href="http://netcat.sourceforge.net/" title="Netcat" target="_blank">Netcat</a> is a great cross platform tool, it can be used for just about all things related to or involving TCP or UDP. Its most practical use is transferring files using Netcat from one machine to another via a network. Where non *nix people usually don’t have SSH installed or set-up, it is much faster to transfer files using Netcat than setup SSH. Netcat is just a single executable, and works across all platforms (Windows,Mac OSX, <a href="http://en.wikipedia.org/wiki/Linux" title="Linux" target="_blank">Linux</a>).

### On the Netcat receiving end

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># nc -l 1234 &gt; out.file
</pre>

This will start Netcat listening on port 1234.

### On the Netcat sending end

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># nc -w 3 [destination] 1234 &lt; out.file
</pre>

This will connect to the receiver and begin transferring files using Netcat.

If you&#8217;d like to transfer files quicker (*nix only I am afraid), you can compress the file during sending process

### On the Netcat receiving end

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># nc -l -p 1234 | uncompress -c | tar xvfp -
</pre>

### On the Netcat sending end

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># tar cfp - /some/dir | compress -c | nc -w 3 [destination] 1234
</pre>