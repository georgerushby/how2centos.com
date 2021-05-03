---
id: 293
title: XSLCache is in PECL
date: 2009-08-28T10:44:34+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=293
permalink: /xslcache-is-in-pecl/
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
  - "43376956"
categories:
  - CentOS
tags:
  - Apache
  - pecl
  - xslcache
---
Originally developed by NYTimes, XSLCache extension for PHP is finally in PECL repository and ready for it&#8217;s first PECL-release. 

&#8220;The XSL Cache extension is a modification of PHP&#8217;s standard XSL extension that caches the parsed XSL stylesheet representation between sessions for 2.5x boost in performance for sites that repeatedly apply the same transform. Although there is still some further work that could be done on the extension, this code is already proving beneficial in production use for a few applications on the New York Times&#8217; website.&#8221;

Installation, from now on, should be as simple as pecl install xslcache

I have modified the linux installing guide, [Installing and upgrading to PHP 5.2.9 on CentOS and Red Hat Linux 5.3 x86_64](http://www.how2centos.com/installing-and-upgrading-to-php-529-on-centos-and-red-hat-linux-53-x86_64/) to reflect this change.