---
id: 2549
title: Install Varnish CentOS 6
date: 2012-05-19T13:11:33+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2549
permalink: /install-varnish-centos-6/
dsq_thread_id:
  - "696149590"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 6.0 Tutorials
  - CentOS 6.1 Tutorials
  - CentOS 6.2 Tutorials
---
This tutorial is intended for system administrators wanting to install Varnish on CentOS 6. The reader should know how to configure a web server or application server and have basic knowledge of the HTTP protocol. Once finished the reader should have a basic Varnish cache up and running with the default configuration.

[Varnish](https://www.varnish-cache.org "Varnish") is a web application accelerator. You install it in front of your web application and it will speed it up significantly.

Varnish web application accelerator homepage: [https://www.varnish-cache.org](https://www.varnish-cache.org "Varnish")  
<!--more-->

### Install the Varnish YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://repo.varnish-cache.org/redhat/varnish-3.0/el5/noarch/varnish-release-3.0-1.noarch.rpm</pre>

### Install Varnish web accelerator

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install varnish
</pre>

### Enable Varnish web accelerator at startup

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chkconfig varnish on
</pre>

### Basic default.vcl

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/varnish/default.vcl
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># This is a basic VCL configuration file for varnish.  See the vcl(7)
# man page for details on VCL syntax and semantics.
#
# Default backend definition.  Set this to point to your content
# server.
#
backend default {
  .host = "127.0.0.1";
  .port = "80";
}
</pre>

### Start Varnish web accelerator

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service varnish start
</pre>

You will now have a basic Varnish web accelerator running on port 8080

### Top 5 Varnish commands

##### varnishstat

Provides all the info you need to spot cache misses and errors.

##### varnishhist

Provides a histogram view of cache hits/misses

##### varnishlog

Provides detailed information on requests.

##### varnishtop

The varnishtop utility reads varnishd shared memory logs and presents a continuously updated list of the most commonly occurring log entries.

##### varnishadm

Command-line varnish administration used to reload vcl and purge urls.

### Managing Varnish with Puppet

If you&#8217;re running Puppet we have included the manifest for installing Varnish on CentOS 6. If you&#8217;re not running Puppet then you can install it by following the instructions outlined in our <a href="http://www.how2centos.com/centos-6-puppet-install/" title="CentOS 6 Puppet Install" target="_blank">CentOS 6 Puppet Install</a>.

This is only the manifest and doesn&#8217;t include any of the files (i.e. default.vcl).

<pre lang="ruby" line="1">class varnish::repo {

	Package {
		provider => rpm,
		ensure => installed
	}

	package { "varnish-release": source => "http://repo.varnish-cache.org/redhat/varnish-3.0/el5/noarch/varnish-release-3.0-1.noarch.rpm"
	}
}

class varnish::install {

	$packagelist = ["varnish"]

	package { $packagelist:
		require => Class ["varnish::repo"],
		ensure => installed
	}
}

class varnish::service {

	service { "varnish":
		ensure => true,
		enable => true,
		hasrestart => true,
		hasstatus => true,
		require => Class ["varnish::install"]
	}
}

class varnish::conf {

	File {
		require => Class ["varnish::install"],
		owner => "root",
		group => "root",
		mode => 644,
		notify => Class ["varnish::service"]
	}

	file { "/etc/varnish/default.vcl":
		source  => "puppet:///modules/varnish/default.vcl"
	}

	file { "/etc/sysconfig/varnish":
		source  => "puppet:///modules/varnish/varnish"
	}
}

class varnish {
	include varnish::repo, varnish::install, varnish::service, varnish::conf
}
</pre>