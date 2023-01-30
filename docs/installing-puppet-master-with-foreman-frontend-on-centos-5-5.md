---
id: 1076
title: Installing Puppet Master with Foreman frontend on CentOS 5.5
date: 2010-08-27T15:24:39+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1076
permalink: /installing-puppet-master-with-foreman-frontend-on-centos-5-5/
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
  - "134357682"
categories:
  - CentOS 5.5 Tutorials
tags:
  - CentOS 5.5
  - Foreman
  - mysql
  - puppet
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif) In this CentOS 5.5 tutorial we will be installing Foreman on a CentOS 5.5 i386 server including Puppet Master and Puppet client. The assumption is that you have a basic to medium understanding of the software required but if you follow this tutorial you should be able to complete the task successfully. 

A bit on the software that we&#8217;ll be using:

**Foreman**  
[Foreman](http://theforeman.org/) is aimed to be a Single Address For All Machines Life Cycle Management.

[Foreman](http://theforeman.org/) integrates with Puppet (and acts as web front end to it).

[Foreman](http://theforeman.org/) takes care of bare bone provisioning until the point puppet is running, allowing Puppet to do what it does best.

[Foreman](http://theforeman.org/) shows you Systems Inventory (based on Facter) and provides real time information about hosts status based on Puppet reports.

[Foreman](http://theforeman.org/) creates everything you need when adding a new machine to your network. It&#8217;s goal being automatically managing everything you would normally manage manually &#8211; that would eventually include DNS, DHCP, TFTP, PuppetCA, CMDB and everything else you might consider useful.

With [Foreman](http://theforeman.org/) You Can Always Rebuild Your Machines From Scratch!

[Foreman](http://theforeman.org/) is designed to work in a large enterprise, where multiple domains, subnets and puppetmasters are required. 

<http://theforeman.org/>  
<!--more-->

**Preliminary Note:**  
I am using a CentOS 5.5 i386 base installation in this tutorial with root access.

* foreman.how2centos.com (IP 10.0.0.100): CentOS 5.5 i386 base installation

Lets begin by adding additional CentOS 5.5. repositories and installing the framework required by Foreman.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install yum-priorities
# rpm -Uhv http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.3.6-1.el5.rf.i386.rpm
# rpm -Uvh http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-4.noarch.rpm
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">cat > /etc/yum.repos.d/foreman.repo &lt;&lt; EOF
[foreman]
name=Foreman Repo
baseurl=http://yum.theforeman.org/stable
gpgcheck=0
enabled=1
EOF
</pre>

Lets begin installing the framework starting with Puppet Master, client and MySQL

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install puppet-server puppet
# yum install mysql mysql-server mysql-devel ruby-mysql rubygem-activerecord
</pre>

Let do a basic Puppet Master and client configuration.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/puppet/puppet.conf
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >[main]
    # The Puppet log directory.
    # The default value is '$vardir/log'.
    logdir = /var/log/puppet

    # Where Puppet PID files are kept.
    # The default value is '$vardir/run'.
    rundir = /var/run/puppet

    # Where SSL certificates are kept.
    # The default value is '$confdir/ssl'.
    ssldir = $vardir/ssl

[puppetd]
    # The file in which puppetd stores a list of the classes
    # associated with the retrieved configuratiion.  Can be loaded in
    # the separate ``puppet`` executable using the ``--loadclasses``
    # option.
    # The default value is '$confdir/classes.txt'.
    classfile = $vardir/classes.txt

    # Where puppetd caches the local configuration.  An
    # extension indicating the cache format is added automatically.
    # The default value is '$confdir/localconfig'.
    localconfig = $vardir/localconfig

	# Enable reporting for Foreman
	report = true
		
[puppetmasterd]
    storeconfigs = true
    dbadapter = mysql
    dbuser = puppet
    dbpassword = puppet
    dbserver = localhost
    dbsocket = /var/lib/mysql/mysql.sock
    rrddir=/var/lib/puppet/rrd
    rrdinterval=$runinterval
    rrdgraph=true
    reports=log, foreman
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/sysconfig/puppet
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># The puppetmaster server
PUPPET_SERVER=foreman.how2centos.com

# If you wish to specify the port to connect to do so here
#PUPPET_PORT=8140

# Where to log to. Specify syslog to send log messages to the system log.
PUPPET_LOG=/var/log/puppet/puppet.log

# You may specify other parameters to the puppet client here
#PUPPET_EXTRA_OPTS=--waitforcert=500
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mkdir /etc/puppet/manifests/classes/
# vi /etc/puppet/manifests/site.pp
</pre>

<pre lang="ruby" line="1">import "classes/*"

node default {
    include sudo
 }
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/puppet/manifests/classes/sudo.pp
</pre>

<pre lang="ruby" line="1">class sudo {
    file { "/etc/sudoers":
        owner => "root",
        group => "root",
        mode  => 440,
    }
}
</pre>

Start MySQL and add it to startup 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service mysqld start
# chkconfig mysqld on
</pre>

Add the Puppet Database

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mysql
mysql> create database puppet;
mysql> grant all privileges on puppet.* to puppet@localhost identified by 'puppet';
mysql> exit
Bye
</pre>

Install Foreman and configure the Database and enable reporting.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install foreman
</pre>

Foreman uses a database, by default, SQLite is used, if you want to use other database (e.g. MySQL) please modify the configuration file under config/database.yml.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mv  /etc/foreman/database.yml /etc/foreman/database.yml.old
# vi /etc/foreman/database.yml
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >production:
  adapter: mysql
  database: puppet
  username: puppet
  password: puppet
  host: localhost
  socket: "/var/lib/mysql/mysql.sock"
</pre>

To enable reporting in Foreman you'll be required to copy foreman-report.rb to your report directory, edit the $foreman_url=, and then add it to your master puppet.conf under the main section add:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cp /usr/share/foreman/extras/puppet/foreman/files/foreman-report.rb /usr/lib/ruby/site_ruby/1.8/puppet/reports/foreman.rb 
# vi /usr/lib/ruby/site_ruby/1.8/puppet/reports/foreman.rb
</pre>

<pre lang="ruby" line="1"># copy this file to your report dir - e.g. /usr/lib/ruby/1.8/puppet/reports/
# add this report in your puppetmaster reports - e.g, in your puppet.conf add:
# reports=log, foreman # (or any other reports you want)

# URL of your Foreman installation
$foreman_url="http://foreman.how2centos.com:3000"

require 'puppet'
require 'net/http'
require 'uri'

Puppet::Reports.register_report(:foreman) do
    Puppet.settings.use(:reporting)
    desc "Sends reports directly to Foreman"

    def process
      begin
        uri = URI.parse($foreman_url)
        http = Net::HTTP.new(uri.host, uri.port)
        if uri.scheme == 'https' then
          http.use_ssl = true
          http.verify_mode = OpenSSL::SSL::VERIFY_NONE
        end
        req = Net::HTTP::Post.new("/reports/create?format=yml")
        req.set_form_data({'report' => to_yaml})
        response = http.request(req)
      rescue Exception => e
        raise Puppet::Error, "Could not send report to Foreman at #{$foreman_url}/reports/create?format=yml: #{e}"
      end
    end
end
</pre>

Finally to initialize the database schema type:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /usr/share/foreman/
# RAILS_ENV=production rake db:migrate
</pre>

Let start, and add to startup, the various componants and browse to your newly installed Puppet Master and client with Foreman frontend.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service puppetmaster start
# service puppet start
# service foreman start
# chkconfig puppetmaster on
# chkconfig puppet on 
# chkconfig foreman on
</pre>

Point your bowser to http://foreman.how2centos.com:3000