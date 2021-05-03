---
id: 214
title: 'Installing #Videocache on #CentOS 5.3'
date: 2009-06-02T05:38:32+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=214
permalink: /installing-videocache-on-centos-53/
dsq_thread_id:
  - "44446781"
categories:
  - CentOS 5.3 Tutorials
tags:
  - Apache
  - centos 5.3
  - videocache
---
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/06/videocache.jpg" alt="videocache" title="videocache" width="81" height="39" class="alignleft size-full wp-image-228" /><img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="centos" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />Videocache is a Squid URL rewriter plugin written in Python for bandwidth optimization while browsing famous video sharing portals/websites like Youtube, Metacafe etc. It helps you save bandwidth when a particular video is requested more than once from the same network/machine.

http://cachevideos.com/

<!--more-->

**Preliminary Note:**

Iâ€™m using a CentOS 5.3 x86_64 server base installation in this tutorial:

* server1.example.co.za (IP 10.0.0.100)

Videocache requires following packages to work.

Squid  
Python  
Python-urlgrabber  
Python-iniparse  
Apache (httpd) Web Server

**Installing Squid and Httpd on CentOS 5.3**

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"><pre>
# yum install httpd
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# yum install squid
</pre>


<p>
  <strong>Squid Basic Configuration</strong>
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# vi /etc/squid/squid.conf
</pre>


<p>
  Uncomment the following example ACL which allows access from your local network 10.0.0.0/24. Make sure you edit this to allow your internal IP network to be able to browse:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# Example rule allowing access from your local networks. Adapt
# to list your (internal) IP networks from where browsing should
# be allowed
acl our_networks src 10.0.0.0/24 
http_access allow our_networks
</pre>


<p>
  Start Squid Proxy and verify that port 3128 is open:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# service squid start
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# netstat -tulpn | grep 3128
tcp        0      0 0.0.0.0:3128                0.0.0.0:*                   LISTEN      1832/(squid)
</pre>


<p>
  <strong>Downloading and installing Videocache via RPM</strong>
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# wget http://cachevideos.com/sites/default/files/pub/videocache/videocache-1.9.1-1.noarch.rpm
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# rpm -ivh videocache-1.9.1-1.noarch.rpm
</pre>


<p>
  Append the following configuration to the end of your Squid Proxy configuration file
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# vi /etc/squid/squid.conf
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
# --BEGIN-- videocache config for squid
url_rewrite_program /usr/bin/python /usr/share/videocache/videocache.py
url_rewrite_children 7
acl videocache_allow_url url_regex -i \.youtube\.com\/get_video\?
acl videocache_allow_url url_regex -i \.googlevideo\.com\/videoplayback \.googlevideo\.com\/videoplay \.googlevideo\.com\/get_video\?
acl videocache_allow_url url_regex -i \.google\.com\/videoplayback \.google\.com\/videoplay \.google\.com\/get_video\?
acl videocache_allow_url url_regex -i \.google\.[a-z][a-z]\/videoplayback \.google\.[a-z][a-z]\/videoplay \.google\.[a-z][a-z]\/get_video\?
acl videocache_allow_url url_regex -i (25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\/videoplayback\?
acl videocache_allow_url url_regex -i (25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\/videoplay\?
acl videocache_allow_url url_regex -i (25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\/get_video\?
acl videocache_allow_url url_regex -i proxy[a-z0-9\-][a-z0-9][a-z0-9][a-z0-9]?\.dailymotion\.com\/
acl videocache_allow_url url_regex -i vid\.akm\.dailymotion\.com\/
acl videocache_allow_url url_regex -i [a-z0-9][0-9a-z][0-9a-z]?[0-9a-z]?[0-9a-z]?\.xtube\.com\/(.*)flv
acl videocache_allow_url url_regex -i bitcast\.vimeo\.com\/vimeo\/videos\/
acl videocache_allow_url url_regex -i va\.wrzuta\.pl\/wa[0-9][0-9][0-9][0-9]?
acl videocache_allow_url url_regex -i \.files\.youporn\.com\/(.*)\/flv\/
acl videocache_allow_url url_regex -i \.msn\.com\.edgesuite\.net\/(.*)\.flv
acl videocache_allow_url url_regex -i media[a-z0-9]?[a-z0-9]?[a-z0-9]?\.tube8\.com\/ mobile[a-z0-9]?[a-z0-9]?[a-z0-9]?\.tube8\.com\/
acl videocache_allow_url url_regex -i \.mais\.uol\.com\.br\/(.*)\.flv
acl videocache_allow_url url_regex -i \.video[a-z0-9]?[a-z0-9]?\.blip\.tv\/(.*)\.(flv|avi|mov|mp3|m4v|mp4|wmv|rm|ram)
acl videocache_allow_url url_regex -i video\.break\.com\/(.*)\.(flv|mp4)
acl videocache_allow_dom dstdomain .mccont.com dl.redtube.com .cdn.dailymotion.com
acl videocache_deny_url url_regex -i http:\/\/[a-z][a-z]\.youtube\.com http:\/\/www\.youtube\.com
url_rewrite_access deny videocache_deny_url
url_rewrite_access allow videocache_allow_url
url_rewrite_access allow videocache_allow_dom
redirector_bypass on
# --END-- videocache config for squid
</pre>


<p>
  Add Squid Proxy and Apache Webserver to your startup and reboot
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
chkconfig squid on
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
chkconfig httpd  on
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">


<pre>
reboot
</pre>


<p>
  [ad#3]
</p>