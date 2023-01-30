---
id: 1052
title: Dell and CentOS the Perfect Combination
date: 2010-08-10T21:01:51+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1052
permalink: /dell-and-centos-the-perfect-combination/
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
dsq_thread_id:
  - "128226766"
categories:
  - CentOS 5.2 Tutorials
  - CentOS 5.3 Tutorials
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
tags:
  - CentOS
  - Dell
---
A quick post to share this mostly unknown gem that Dell manages it&#8217;s own Open Manage Linux Repository. 

Read more: <http://linux.dell.com/wiki/index.php/Repository/OMSA> 

To get your CentOS server installed with Server Administrator set up the Dell Open Manage Repository like so:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget &#45;q &#45;O &#45; http://linux.dell.com/repo/hardware/latest/bootstrap.cgi | bash
</pre>

Then install the Server Administrator

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install srvadmin-all
</pre>

Finally browse to your newly installed Open Manage Server Administrator and monitor your Dell hardware.

https://your.Centos.Server:1311/

Log on screen &#8211; Use your root username and password

[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/08/openmanage_logon-e1284360207143.jpg" alt="Dell Open Manage Server Administrator" title="Dell Open Manage Server Administrator" width="580" height="328" class="aligncenter size-full wp-image-1059" />](http://www.how2centos.com/wp-content/uploads/2010/08/openmanage_logon-e1284360207143.jpg)

Main Screen after log on

[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/08/openmanage_main-e1284360124445.jpg" alt="Dell Open Manage Server Administrator Main" title="Dell Open Manage Server Administrator Main" width="580" height="284" class="aligncenter size-full wp-image-1061" />](http://www.how2centos.com/wp-content/uploads/2010/08/openmanage_main-e1284360124445.jpg)

TIP: After installing the Dell Server Administrator get your service tag number from your CentOS Linux server by running

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># dmidecode -s system-serial-number
ABCDEF1
</pre>