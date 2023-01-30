---
id: 705
title: 'Subversion: Failed to get lock on destination repository'
date: 2010-02-15T13:13:09+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=705
permalink: /subversion-failed-to-get-lock-on-destination-repository/
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
  - "67003219"
categories:
  - CentOS 5.3 Tutorials
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
  - CentOS 5.8 Tutorials
---
In the event that your Subversion server was powered down incorrectly or just crashed during one of its subversion mirror synchronizations you might end up with a lock on the destination repository. 

This basic tutorial will help you to remove the lock and continue with your synchronizations.

Below is the output from the svnsync showed this:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># /usr/bin/svnsync &#45;&#45;non-interactive synchronize svn://{destination repository}/opt/svn/{repo}

Failed to get lock on destination repos, currently held by 'svn.how2centos.com:fbc2c0c8-957f-0410-8a06-87f31314868b'
Failed to get lock on destination repos, currently held by 'svn.how2centos.com:fbc2c0c8-957f-0410-8a06-87f31314868b'
Failed to get lock on destination repos, currently held by 'svn.how2centos.com:fbc2c0c8-957f-0410-8a06-87f31314868b'
Failed to get lock on destination repos, currently held by 'svn.how2centos.com:fbc2c0c8-957f-0410-8a06-87f31314868b'
Failed to get lock on destination repos, currently held by 'svn.how2centos.com:fbc2c0c8-957f-0410-8a06-87f31314868b'
Failed to get lock on destination repos, currently held by 'svn.how2centos.com:fbc2c0c8-957f-0410-8a06-87f31314868b'
Failed to get lock on destination repos, currently held by 'svn.how2centos.com:fbc2c0c8-957f-0410-8a06-87f31314868b'
Failed to get lock on destination repos, currently held by 'svn.how2centos.com:fbc2c0c8-957f-0410-8a06-87f31314868b'
Failed to get lock on destination repos, currently held by 'svn.how2centos.com:fbc2c0c8-957f-0410-8a06-87f31314868b'
Failed to get lock on destination repos, currently held by 'svn.how2centos.com:fbc2c0c8-957f-0410-8a06-87f31314868b'
svnsync: Couldn't get lock on destination repos after 10 attempts
</pre>

To remove the lock you&#8217;ll need to do the following:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># svn propdel svn:sync-lock &#45;&#45;revprop -r 0 svn://{destination repository}/opt/svn/{repo}
# /usr/bin/svnsync &#45;&#45;non-interactive synchronize svn://{destination repository}/opt/svn/{repo}

Committed revision 1694.
Copied properties for revision 1694.
Committed revision 1695.
Copied properties for revision 1695.
</pre>