---
id: 2644
title: CentOS 6 NTP Server
date: 2012-05-24T15:01:07+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2644
permalink: /centos-6-ntp-server/
dsq_thread_id:
  - "702076149"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 6.0 Tutorials
  - CentOS 6.1 Tutorials
  - CentOS 6.2 Tutorials
---
It is important for systems administrators to make sure that mission-critical servers are always using the correct system time. 

The ntpd (Network Time Protocol daemon) program is an operating system daemon which sets and maintains the system time of day in synchronism with Internet standard time servers. Make sure that the time zone configuration of your computer is correct. ntpd itself does not do anything about the time zones, it just uses UTC internally.  
<!--more-->

### Install Network Time Protocol (NTP) daemon

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install ntp
</pre>

### Add NTP daemon to startup

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chkconfig ntpd on
</pre>

### Edit the NTPD config file

Here you can either use the default NTP public servers or add servers closer to your region.

Visit <http://www.pool.ntp.org/en/> and either considder joining or getting your regional NTP pool servers

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/ntp.conf
</pre>

<pre lang="vim" line="20"># Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
server 0.centos.pool.ntp.org
server 1.centos.pool.ntp.org
server 2.centos.pool.ntp.org
</pre>

### Start the NTP daemon

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service ntpd start
</pre>

### Standard NTP query program (ntpq)

Print a list of the peers known to the server as well as a summary of their state.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
*javanese.kjsl.c 69.36.224.15     2 u  108  128  377    1.401    3.392   4.779
+66-191-139-149. 132.163.4.101    2 u   70  128  377   46.044   11.205   5.378
+ntp.sunflower.c 132.236.56.250   3 u   85  128  377   50.962   -2.129  14.112
</pre>

### Managing NTPd with Puppet

If you&#8217;re running Puppet we have included the manifest for installing Varnish on CentOS 6. If you&#8217;re not running Puppet then you can install it by following the instructions outlined in our <a href="http://www.how2centos.com/centos-6-puppet-install/" title="CentOS 6 Puppet Install" target="_blank">CentOS 6 Puppet Install</a>.

This is only the manifest and doesn&#8217;t include any of the files (i.e. ntp.conf).

<pre lang="ruby" line="1">class ntpd::install {
 
	$packagelist = ["ntp"]
 
	package { $packagelist:
		ensure => installed
	}
}
 
class ntpd::service {
 
	service { "ntpd":
		ensure => true,
		enable => true,
		hasrestart => true,
		hasstatus => true,
		require => Class ["ntpd::install"]
	}
}
 
class ntpd::conf {
 
	File {
		require => Class ["ntpd::install"],
		owner => "root",
		group => "root",
		mode => 644,
		notify => Class ["ntpd::service"]
	}
 
	file { "/etc/ntp.conf":
		source  => "puppet:///modules/ntpd/ntp.conf"
	}
}
 
class ntpd {
	include ntpd::install, ntpd::service, ntpd::conf
}
</pre>