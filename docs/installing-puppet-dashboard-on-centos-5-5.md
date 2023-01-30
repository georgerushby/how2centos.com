---
id: 866
title: Installing Puppet Dashboard on CentOS 5.5
date: 2010-05-25T22:29:15+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=866
permalink: /installing-puppet-dashboard-on-centos-5-5/
dsq_thread_id:
  - "101050035"
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
categories:
  - CentOS 5.5 Tutorials
tags:
  - CentOS 5.5
  - puppet
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif)The Puppet Dashboard is a web front end that keeps you informed and in control of everything going on in your Puppet ecosystem. It currently functions as a reporting dashboard and an external node repository and will soon do much more, including having better marketing copy.

Fundamentally, Dashboard lets you do two things: configure nodes using parameters, classes and groups for use as an external nodes tool; and monitor the status of nodes through real-time reporting and versioned change tracking.

To learn more about the Puppet Dashboard and it&#8217;s different views go read: [A tour of the Puppet Dashboard](http://www.puppetlabs.com/blog/a-tour-of-puppet-dashboard-0-1-0/)

**Preliminary Note:**  
I am using a CentOS 5.5 i386 base installation in this tutorial.

* puppetmaster.how2centos.com (IP 10.0.0.100): CentOS 5.5 i386 base installation

The assumption is that you have a working knowledge of installing/configuring Puppet. If not then read this article: [Installing Puppet Master and Client on CentOS](http://www.how2centos.com/how-to-install-a-puppet-master-and-client-server-on-centos-52/)  
<!--more-->

  


**Let&#8217;s Begin**

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install yum-priorities
# rpm -Uhv http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.3.6-1.el5.rf.i386.rpm
# rpm -Uvh http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-4.noarch.rpm
# yum update
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install ruby rubygems rubygem-rails rubygem-sqlite3-ruby ruby-devel ruby-mysql
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install mysql-server
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service mysqld start
# chkconfig mysqld on
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install puppet puppet-server
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://puppetlabs.com/downloads/dashboard/puppet-dashboard-1.0.4.tgz
# tar zxvf puppet-dashboard-1.0.4.tgz
#  mv puppetlabs-puppet-dashboard-071acf4/ /opt/puppet-dashboard
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /opt/puppet-dashboard
# cp config/database.yml.example config/database.yml
# vi config/database.yml
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># database.yml
# ============
#
# The `config/database.yml` file contains your custom settings for connecting
# the Dashboard to a database. You must create the databases you plan to use
# and add their connection details here to use the Dashboard.
#
# Format
# ------
#
# This file is split into sections describing different environments, each
# optimized for a different use. If you're only using the Dashboard, you
# only need to configure the "production" environment.
#
# Lines starting with an octothorpe ("#") are comments. Uncommented,
# unindented lines start a new environment (e.g., "production"). Indented
# lines below an unindented line are settings related to that environment.
#
# Example
# -------
#
#     # Section describing the "production" environment, don't change this line:
#     production:
#         # Connect to a database named "dashboard":
#         database: dashboard
#         # Connect to this database as the "root" user:
#         username: root
#         # Connect to this database using "mypassword" as the password:
#         password: mypassword
#         # Use "utf8" as the database character encoding, don't change this line:
#         encoding: utf8
#         # Use a MySQL database, don't change this line:
#         adapter: mysql
#
# Environments
# ------------
#
# The "production" environment is optimized for fast performance, like when
# you deploy the Dashboard for use in your organization. It is used when:
# * Starting a console with: script/console production
# * Starting a server with: scipt/server -e production
# * Running a rake task with: rake RAILS_ENV=production ...
production:
  database: dashboard
  username: root
  password:
  encoding: utf8
  adapter: mysql

# The "development" environment is optimized for those developing the
# Dashboard software and reloads code as it's modified. It is used when:
# * Starting a console with: script/console
# * Starting a server with: script/server
# * Running a rake task with: rake ...
development:
  database: dashboard
  username: root
  password:
  encoding: utf8
  adapter: mysql

# The "test" environment is used for running tests. WARNING: The database
# defined for this will be erased and re-generated from your development
# database when you run "rake". Do NOT set this db to the same as
# "development" or "production".
test:
  database: dashboard_test
  username: root
  password:
  encoding: utf8
  adapter: mysql

</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /opt/puppet-dashboard
# rake install
# rake RAILS_ENV=production db:create
# rake RAILS_ENV=production db:migrate
</pre>

Start up the Puppet Dashboard to see if it works

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># /opt/puppet-dashboard/script/server
=> Booting WEBrick
=> Rails 2.3.5 application starting on http://0.0.0.0:3000
=> Call with -d to detach
=> Ctrl-C to shutdown server
[2011-03-07 09:24:49] INFO  WEBrick 1.3.1
[2011-03-07 09:24:49] INFO  ruby 1.8.5 (2006-08-25) [x86_64-linux]
[2011-03-07 09:24:50] INFO  WEBrick::HTTPServer#start: pid=5235 port=3000
[2010-05-25 21:08:05] INFO  going to shutdown ...
[2010-05-25 21:08:05] INFO  WEBrick::HTTPServer#start done.
Exiting
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/sysconfig/puppet
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># The puppetmaster server
PUPPET_SERVER=puppetmaster.how2centos.com

# If you wish to specify the port to connect to do so here
#PUPPET_PORT=8140

# Where to log to. Specify syslog to send log messages to the system log.
PUPPET_LOG=/var/log/puppet/puppet.log

# You may specify other parameters to the puppet client here
#PUPPET_EXTRA_OPTS=--waitforcert=500
</pre>

Append the following line to the Puppet Master config: PUPPETMASTER\_EXTRA\_OPTS=”––reports puppet_dashboard”

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/sysconfig/puppetmaster
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" ># Location of the main manifest
#PUPPETMASTER_MANIFEST=/etc/puppet/manifests/site.pp

# Where to log general messages to.
# Specify syslog to send log messages to the system log.
#PUPPETMASTER_LOG=syslog

# You may specify an alternate port or an array of ports on which
# puppetmaster should listen. Default is: 8140
# If you specify more than one port, the puppetmaster ist automatically
# started with the servertype set to mongrel. This might be interesting
# if you'd like to run your puppetmaster in a loadbalanced cluster.
# Please note: this won't setup nor start any loadbalancer.
# If you'd like to run puppetmaster with mongrel as servertype but only
# on one (specified) port, you have to add --servertype=mongrel to
# PUPPETMASTER_EXTRA_OPTS.
# Default: Empty (Puppetmaster isn't started with mongrel, nor on a
# specific port)
#
# Please note: Due to reduced options in the rc-functions lib in RHEL/Centos
# versions prior to 5, this feature won't work. Fedora versions >= 8 are
# known to work.
#PUPPETMASTER_PORTS=""
# Puppetmaster on a different port, run with standard webrick servertype
#PUPPETMASTER_PORTS="8141"
# Example with multiple ports which will start puppetmaster with mongrel
# as a servertype
#PUPPETMASTER_PORTS=( 18140 18141 18142 18143 )

# You may specify other parameters to the puppetmaster here
#PUPPETMASTER_EXTRA_OPTS=--no-ca

PUPPETMASTER_EXTRA_OPTS="--reports puppet_dashboard"
</pre>

Append the following line to the Puppet Client config: report = true

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/puppet/puppet.conf
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >[main]
    # Where Puppet stores dynamic and growing data.
    # The default value is '/var/puppet'.
    vardir = /var/lib/puppet

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

    report = true
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">#  mkdir -p /var/lib/puppet/lib/puppet/reports/
#  cp /opt/puppet-dashboard/ext/puppet/puppet_dashboard.rb /var/lib/puppet/lib/puppet/reports/
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service puppetmaster start
# service puppet start
# chkconfig puppet on
# chkconfig puppetmaster on
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /opt/puppet-dashboard
# rake reports:import
(in /opt/puppet-dashboard)
Importing 0 reports from /var/lib/puppet/reports/
Importing:     100% |#############################################| Time: 00:00:00
0 of 0 reports imported
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/init.d/puppet-dashboard
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/bin/bash
#
# chkconfig: 2345 80 05
# Description: Puppet Dashboard init.d script
# Hacked by : How2CentOS - http://www.how2centos.com

# Get function from functions library
. /etc/init.d/functions

# Start the service Puppet Dashboard
start() {
        echo -n "Starting Puppet Dashboard: "
        /usr/bin/ruby /opt/puppet-dashboard/script/server >/dev/null 2>&1 &
        ### Create the lock file ###
        touch /var/lock/subsys/puppetdb
        success $"Puppet Dashboard startup"
        echo
}

# Restart the service Puppet Dashboard
stop() {
        echo -n "Stopping Puppet Dashboard: "
        kill -9 `ps ax | grep "/usr/bin/ruby /opt/puppet-dashboard/script/server" | grep -v grep | awk '{ print $1 }'` >/dev/null 2>&1
        ### Now, delete the lock file ###
        rm -f /var/lock/subsys/puppetdb
        success $"Puppet Dashboard shutdown"
        echo
}

### main logic ###
case "$1" in
  start)
        start
        ;;
  stop)
        stop
        ;;
  status)
        status Puppet DB
        ;;
  restart|reload|condrestart)
        stop
        start
        ;;
  *)
        echo $"Usage: $0 {start|stop|restart|reload|status}"
        exit 1
esac

exit 0
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chmod 755 /etc/init.d/puppet-dashboard
# chkconfig &#45;&#45;add puppet-dashboard
# chkconfig puppet-dashboard on
# service puppet-dashboard start
Starting Puppet Dashboard:                                  [ <span style="color: #008000;"> OK </span>]
</pre>