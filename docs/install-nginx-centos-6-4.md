---
id: 2828
title: Install Nginx CentOS 6.4 (reverse proxy)
date: 2013-07-29T16:51:54+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2828
permalink: /install-nginx-centos-6-4/
dsq_thread_id:
  - "1544589121"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 6.4 Tutorials
---
Nginx (pronounced &#8220;engine x&#8221;) is a free, open-source, high-performance HTTP server. Nginx is known for its stability, rich feature set, simple configuration, and low resource consumption. This tutorial will guide you through setting up Nginx as a reverse proxy in front of a Apache back-end.

The assumption for installing Nginx on CentOS 6.4 is that you are running as root and have a basic understanding of the software required but if you follow this tutorial you should be able to complete the task successfully.  
<!--more-->

### Install Yum Priorities

For a brief overview on and how to configure Yum Priorities you can follow the instructions outlined in our <a href="http://www.how2centos.com/install-yum-priorities-centos/" target="_blank">Install YUM Priorities on CentOS</a> tutorial.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># yum install yum-priorities</pre>

### Installing Nginx on CentOS 6.4 x86_64

### Install the EPEL x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm</pre>

### Install the IUS x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://dl.iuscommunity.org/pub/ius/stable/Redhat/6/x86_64/ius-release-1.0-10.ius.el6.noarch.rpm</pre>

### Install Nginx (and Apache if you haven&#8217;t already)

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># yum install nginx
# yum install httpd
</pre>

### Configure Nginx

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># vim /etc/nginx/conf.d/how2centos.conf 
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >## Apache backend for http://www.how2centos.com/ ##
upstream apachebackend  {
    server 127.0.0.1:8080; #apachebackend
}

## Start http://www.how2centos.com/ ##
server {
    listen       80 default;
    server_name  www.how2centos.com how2centos.com;
    access_log  /var/log/nginx/how2centos.com.access.log  main;
    error_log  /var/log/nginx/how2centos.com.error.log;
    root   /usr/share/nginx/html;
    index  index.html index.htm index.php;

## send request back to apachebackend ##
    location / {
        proxy_pass  http://apachebackend;
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_redirect off;
        proxy_buffering on;
        proxy_buffers 12 12k;
        proxy_set_header        Host            $host;
        proxy_set_header        X-Real-IP       $remote_addr;
        proxy_set_header        X-Forwarded-For $remote_addr;
    }
}
## End http://www.how2centos.com/ ##
</pre>

### Configure Apache

Edit the httpd.conf file so that Apache listens on port 8080 otherwise you&#8217;ll have a conflict with Nginx

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># vim /etc/httpd/conf/httpd.conf
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># Listen: Allows you to bind Apache to specific IP addresses and/or
# ports, in addition to the default. See also the &lt;VirtualHost&gt;
# directive.
#
# Change this to Listen on specific IP addresses as shown below to
# prevent Apache from glomming onto all bound IP addresses (0.0.0.0)
#
#Listen 80
Listen 8080
</pre>

### Add Apache and Nginx to the system startup

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># chkconfig httpd on
# chkconfig nginx on
</pre>

### Restart Apache and Nginx

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># service httpd restart
# service nginx restart
</pre>