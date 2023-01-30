---
id: 1137
title: Installing Cherokee on CentOS 5.8
date: 2010-09-06T11:25:29+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1137
permalink: /installing-cherokee-on-centos-5-5/
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
  - "138121732"
categories:
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
  - CentOS 5.8 Tutorials
tags:
  - CentOS 5.5
  - CentOS 5.8 Tutorials
  - Cherokee
---
This tutorial is intended for system administrators wanting to install Cherokee web server on a CentOS 5.8 x86_64 

Cherokee is a very fast, flexible and easy to configure web server. It supports the widespread technologies nowadays: FastCGI, SCGI, PHP, CGI, TLS and SSL encrypted connections, virtual hosts, authentication, on the fly encoding, load balancing, Apache compatible log files, and much more. 

### Install the RPMForge x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://apt.sw.be/redhat/el5/en/x86_64/rpmforge/RPMS/rpmforge-release-0.5.2-2.el5.rf.x86_64.rpm</pre>

### Install the EPEL x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install yum-priorities
# yum update
</pre>

### Install MySQL and MySQL server

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install mysql mysql-server
</pre>

### Install RRDTool

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install rrdtool
</pre>

### Install Cherokee web server

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install cherokee
# chkconfig cherokee on
# service cherokee start
</pre>

Now direct your browser to http://10.0.0.3

You should see the Cherokee placeholder page.

Cherokee can be configured through a web-based control panel which we can start as follows:

cherokee-admin -b

(By default cherokee-admin binds only to 127.0.0.1 (localhost), with the -b parameter you can specify the network address to listen to. If no IP is provided, it will bind to all interfaces.)

Output should be similar to this one:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cherokee-admin -b

Login:
  User:              admin
  One-time Password: gtVzmvy6Rqy9idKy

Web Interface:
  URL:               http://localhost:9090/

Cherokee Web Server 1.0.6 (Aug  6 2010): Listening on port ALL:9090, TLS
disabled, IPv6 enabled, using epoll, 4096 fds system limit, max. 2041
connections, caching I/O, single thread
</pre>

The admin web interface can be found on http://10.0.0.3:9090/ (make sure to enter your one-time password)

To stop cherokee-admin, type CTRL+C on the shell.

### Managing Varnish with Puppet

If you&#8217;re running Puppet we have included the manifest for installing Cherokee on CentOS 5. If you&#8217;re not running Puppet then you can install it by following the instructions outlined in our <a href="http://www.how2centos.com/centos-5-puppet-install/" title="CentOS 5 Puppet Install" target="_blank">CentOS 5 Puppet Install</a>.

This is only the manifest and doesn&#8217;t include any of the files (i.e. cherokee.conf).

<pre lang="ruby" line="1">class cherokee::repo {
 
	Package {
		provider => rpm,
		ensure => installed
	}
 
	package {"epel-release": source => "http://download.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm";
		"rpmforge-release": source => "http://apt.sw.be/redhat/el5/en/x86_64/rpmforge/RPMS/rpmforge-release-0.5.2-2.el5.rf.x86_64.rpm"
	}
}

class cherokee::install {

	$packagelist = [
		"cherokee",
		"rrdtool",
		"mysql",
		"mysql-server",
	]

	package { $packagelist:
		ensure => latest,
		require => Class ["cherokee::repo"],
	}
}

class cherokee::conf {

	File {
		owner => "root",
		group => "root",
		mode => 0644,
		require => Class ["cherokee::install"],
		notify => Class ["cherokee::service"]
	}

	file { "/var/www/cherokee/phpinfo.php":
		source => "puppet:///modules/cherokee/phpinfo.php"
	}

	file { "/etc/cherokee/cherokee.conf":
		source => "puppet:///modules/cherokee/cherokee.conf"
	}
}

class cherokee::service {

	$servicelist = [ 
		"cherokee",
		"mysqld",
	]

	service { $servicelist:
		require => Class ["cherokee::install"],
		ensure => true,
		enable => true,
		hasrestart => false,
		hasstatus => true
	}
}

class cherokee {
		include cherokee::repo, cherokee::install, cherokee::conf, cherokee::service
}
</pre>

**Links**

Cherokee: <http://www.cherokee-project.com/>  
MySQL: <http://www.mysql.com/>  
CentOS: <http://centos.org/>

[+George Rushby](https://plus.google.com/112343473763635198843/about?rel=author)