---
id: 2473
title: CentOS 6 Puppet Install
date: 2012-03-28T14:51:05+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2473
permalink: /centos-6-puppet-install/
slider_image:
  - ""
dsq_thread_id:
  - "627103707"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 6.0 Tutorials
  - CentOS 6.1 Tutorials
  - CentOS 6.2 Tutorials
---
In this tutorial we&#8217;ll be covering the very basics of installing and configuring Puppet. Puppet is a system for automating system administration tasks. Its automation saves you countless hours of frustration, monotony and reinventing the wheel. It lets you perform administrative task from a central systems to any number of systems running any variant of operating system.

For a more complete description visit [Puppet Labs.](http://reductivelabs.com/trac/puppet/wiki/AboutPuppet)  
<!--more-->

### Installing the Puppet CentOS 6 packages

### Install the Puppet Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -ivh http://yum.puppetlabs.com/el/6/products/i386/puppetlabs-release-6-7.noarch.rpm</pre>

### Install the EPEL x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm</pre>

### Install the Puppet Master packages

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install puppet-server
</pre>

### Install the Puppet Client packages

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install puppet
</pre>

### A Simple Manifest: Managing Ownership of a File

Step 1: Create a minimal manifest file called site.pp in /etc/puppet/manifests with the following content:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/puppet/manifests/site.pp
</pre>

<pre lang="ruby" line="1"># /etc/puppet/manifests/site.pp

import "classes/*"

node default {
    include sudo
 }
</pre>

Step 2: Next create the sudo.pp class in /etc/puppet/manifests/classes/ with the following content:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/puppet/manifests/classes/sudo.pp
</pre>

<pre lang="ruby" line="1"># /etc/puppet/manifests/classes/sudo.pp

class sudo {
    file { "/etc/sudoers":
        owner => "root",
        group => "root",
        mode  => 440,
    }
}
</pre>

This class which will ensure that the owner, group, and mode of the /etc/sudoers file will be set consistently across all systems that belong to that class.

Step 3: Start the Puppet Master service and enable startup on boot 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service puppetmaster start
# chkconfig puppetmaster on
</pre>

### Configuring Puppet

Configure the puppet client to connect to the server and enable logging. Edit the file /etc/sysconfig/puppet and uncomment the PUPPET\_LOG and PUPPET\_SERVER line specifying the servers address.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/sysconfig/puppet
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># The puppetmaster server
PUPPET_SERVER=PuppetMaster

# If you wish to specify the port to connect to do so here
#PUPPET_PORT=8140

# Where to log to. Specify syslog to send log messages to the system log.
PUPPET_LOG=/var/log/puppet/puppet.log

# You may specify other parameters to the puppet client here
#PUPPET_EXTRA_OPTS=--waitforcert=500
</pre>

The client will automatically pull configuration from the server every 30 minutes, start it as a service and enable startup on boot

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service puppet start
# chkconfig puppet on
</pre>

### Sign the SSL key request from the Puppet Client

In order for the two systems to communicate securely we need to create signed SSL certificates. You should be logged into both the _Puppet Master_ and _Puppet_ machines for this next step. 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># puppetca &#45;&#45;list
server2.example.co.za
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># puppetca &#45;&#45;sign server2.example.co.za
Signed server2.example.co.za
</pre>

[+George Rushby](https://plus.google.com/u/1/112343473763635198843/about?rel=author)