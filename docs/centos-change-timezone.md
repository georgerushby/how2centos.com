---
id: 2269
title: CentOS Change Timezone
date: 2011-10-29T17:22:32+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2269
permalink: /centos-change-timezone/
dsq_thread_id:
  - "456257531"
slider_image:
  - ""
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
---
After installing CentOS we sometimes see that the date is not in our desired timezone, instead it defaulted to the PST timezone.

Correcting your timezone is an easy operation, so here is a quick guide to change your CentOS timezone configuration file. 

Firstly you&#8217;ll need to know your timezone and/or country, a list can be found in /usr/share/zoneinfo/

The more generic procedure to change the timezone is to create a symlink to file /etc/localtime

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ln -sf /usr/share/zoneinfo/Africa/Johannesburg /etc/localtime
</pre>

OR you can copy and replace the current localtime setting

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cp /usr/share/zoneinfo/Africa/Johannesburg /etc/localtime
</pre>

To verify that your timezone is changed use the date command:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># date
</pre>