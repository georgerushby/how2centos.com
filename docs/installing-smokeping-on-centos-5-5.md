---
id: 989
title: Installing Smokeping on CentOS 5.5
date: 2010-08-06T15:46:33+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=989
permalink: /installing-smokeping-on-centos-5-5/
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
  - "126761998"
slider_image:
  - ""
categories:
  - CentOS 5.5 Tutorials
tags:
  - Apache
  - CentOS 5.5
  - smokeping
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif) [<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/08/smokeping.png" alt="" title="smokeping" width="120" height="40" class="alignleft size-full wp-image-1038" />](http://www.how2centos.com/wp-content/uploads/2010/08/smokeping.png) In this CentOS 5.5 tutorial we will be installing Smokeping and SmokeTrace on a CentOS 5.5 i386 server. The assumption is that you have a basic to medium understanding of Apache but if you follow this tutorial you should be able to complete the task successfully. 

A bit on the software that we&#8217;ll be using:

**Smokeping**  
SmokePing keeps track of your network latency:

* Best of breed latency visualisation.  
* Interactive graph explorer.  
* Wide range of latency measurment plugins.  
* Master/Slave System for distributed measurement.  
* Highly configurable alerting system.  
* Live Latency Charts with the most &#8216;interesting&#8217; graphs.  
* Free and OpenSource Software written in Perl written by Tobi Oetiker, the creator of MRTG and RRDtool

<http://oss.oetiker.ch/smokeping/> 

**Preliminary Note:**  
I am using a CentOS 5.5 i386 base installation in this tutorial.

* www.how2centos.com (IP 10.0.0.100): CentOS 5.5 i386 base installation

Lets begin by installing the framework required by Smokeping.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install yum-priorities
</pre>

### Install the EPEL i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm</pre>

### Install the RPMForge i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.5.2-2.el5.rf.i386.rpm</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install httpd
# yum install rrdtool
# yum install fping
# yum install echoping
# yum install curl
# yum install perl perl-Net-Telnet perl-Net-DNS perl-LDAP perl-libwww-perl perl-RadiusPerl perl-IO-Socket-SSL perl-Socket6 perl-CGI-SpeedyCGI 
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://oss.oetiker.ch/smokeping/pub/smokeping-2.4.1.tar.gz
# tar zxvf smokeping-2.4.1.tar.gz
# mv smokeping-2.4.1 /opt/smokeping
# cd /opt/smokeping
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd bin/
# cp smokeping.dist smokeping
# cd ../htdocs/
# cp smokeping.cgi.dist smokeping.cgi
# cp tr.cgi.dist tr.cgi
# cd ../etc/
# cp config.dist config
# cp basepage.html.dist basepage.html
# cp smokemail.dist smokemail
# cp tmail.dist tmail
# cp smokeping_secrets.dist smokeping_secrets
# chmod 600 /opt/smokeping/etc/smokeping_secrets
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /opt/smokeping/bin/smokeping
</pre>

Replace this:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/usr/sepp/bin/perl-5.8.4 -w
# -*-perl-*-

use lib qw(/usr/pack/rrdtool-1.2.23-mo/lib/perl);
use lib qw(lib);

use Smokeping 2.004000;

Smokeping::main("etc/config.dist");
</pre>

With This:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/usr/bin/perl -w
# -*-perl-*-

use lib qw(/usr/lib/perl5/vendor_perl/5.8.8/i386-linux-thread-multi/auto/RRDs/);
use lib qw(/opt/smokeping/lib);

use Smokeping 2.004000;

Smokeping::main("/opt/smokeping/etc/config");
</pre>

or you can Patch the file: 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /opt/smokeping/bin
# vi /opt/smokeping/bin/smokeping.patch
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >*** /opt/smokeping/bin/smokeping.dist   2008-06-10 15:08:07.000000000 +0200
--- /opt/smokeping/bin/smokeping        2010-08-04 16:43:08.000000000 +0200
***************
*** 1,12 ****
! #!/usr/sepp/bin/perl-5.8.4 -w
  # -*-perl-*-

! use lib qw(/usr/pack/rrdtool-1.2.23-mo/lib/perl);
! use lib qw(lib);

  use Smokeping 2.004000;
!
! Smokeping::main("etc/config.dist");

  =head1 NAME

--- 1,12 ----
! #!/usr/bin/perl -w
  # -*-perl-*-

! use lib qw(/usr/lib/perl5/vendor_perl/5.8.8/i386-linux-thread-multi/auto/RRDs/);
! use lib qw(/opt/smokeping/lib);

  use Smokeping 2.004000;
!
! Smokeping::main("/opt/smokeping/etc/config");

  =head1 NAME

</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># patch -p1 -i smokeping.patch /opt/smokeping/bin/smokeping
patching file /opt/smokeping/bin/smokeping
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /opt/smokeping/htdocs/smokeping.cgi
</pre>

Replace this:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/usr/sepp/bin/speedy -w
# -*-perl-*-

use lib qw(/usr/pack/rrdtool-1.0.33-to/lib/perl);
use lib qw(/home/oetiker/data/projects/AADJ-smokeping/dist/lib);
use CGI::Carp qw(fatalsToBrowser);

use Smokeping 2.004000;

Smokeping::cgi("/home/oetiker/data/projects/AADJ-smokeping/dist/etc/config");
</pre>

With this:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/usr/bin/speedy -w
# -*-perl-*-

use lib qw(/usr/lib/perl5/vendor_perl/5.8.8/i386-linux-thread-multi/auto/RRDs);
use lib qw(/opt/smokeping/lib);
use CGI::Carp qw(fatalsToBrowser);

use Smokeping 2.004000;

Smokeping::cgi("/opt/smokeping/etc/config");
</pre>

or you can Patch the file:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /opt/smokeping/htdocs/
# vi /opt/smokeping/htdocs/smokeping_cgi.patch
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >***************
*** 1,13 ****
! #!/usr/sepp/bin/speedy -w
  # -*-perl-*-

! use lib qw(/usr/pack/rrdtool-1.0.33-to/lib/perl);
! use lib qw(/home/oetiker/data/projects/AADJ-smokeping/dist/lib);
  use CGI::Carp qw(fatalsToBrowser);

  use Smokeping 2.004000;

! Smokeping::cgi("/home/oetiker/data/projects/AADJ-smokeping/dist/etc/config");

  =head1 NAME

--- 1,13 ----
! #!/usr/bin/speedy -w
  # -*-perl-*-

! use lib qw(/usr/lib/perl5/vendor_perl/5.8.8/i386-linux-thread-multi/auto/RRDs);
! use lib qw(/opt/smokeping/lib);
  use CGI::Carp qw(fatalsToBrowser);

  use Smokeping 2.004000;

! Smokeping::cgi("/opt/smokeping/etc/config");

  =head1 NAME

</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># patch -p1 -i smokeping_cgi.patch /opt/smokeping/htdocs/smokeping.cgi
patching file /opt/smokeping/htdocs/smokeping.cgi
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /opt/smokeping/htdocs
# vi /opt/smokeping/htdocs/tr.cgi
</pre>

Change this:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/usr/sepp/bin/speedy-5.8.8 -w
use strict;
use lib qw(/home/oposs/smokeping/software/lib);
use lib qw(perl);
</pre>

To this:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/usr/bin/speedy -w
use strict;
use lib qw(/opt/smokeping/lib);
use lib qw(perl);
</pre>

or you can Patch the file:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /opt/smokeping/htdocs/tr_cgi.patch
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >*** /opt/smokeping/htdocs/tr.cgi.dist   2008-06-14 00:02:34.000000000 +0200
--- /opt/smokeping/htdocs/tr.cgi        2010-08-06 15:01:31.000000000 +0200
***************
*** 1,6 ****
! #!/usr/sepp/bin/speedy-5.8.8 -w
  use strict;
! use lib qw(/home/oposs/smokeping/software/lib);
  use lib qw(perl);

  use CGI;
--- 1,6 ----
! #!/usr/bin/speedy -w
  use strict;
! use lib qw(/opt/smokeping/lib);
  use lib qw(perl);

  use CGI;
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># patch -p1 -i tr_cgi.patch /opt/smokeping/htdocs/tr.cgi
patching file /opt/smokeping/htdocs/tr.cgi
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mkdir -p /var/www/html/smokeping/img /var/www/html/smokeping/script/ /opt/smokeping/data /opt/smokeping/var
# chown -R apache:apache /var/www/html/smokeping/img
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ln -s /opt/smokeping/htdocs/cropper /var/www/html/smokeping/cropper
# ln -s /opt/smokeping/htdocs/resource /var/www/html/smokeping/resource
# ln -s /opt/smokeping/htdocs/script/Tr.js /var/www/html/smokeping/script/Tr.js
# ln -s /opt/smokeping/htdocs/smokeping.cgi /var/www/html/smokeping/smokeping.cgi
# ln -s /opt/smokeping/htdocs/tr.cgi /var/www/html/smokeping/tr.cgi
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chmod 4775 /bin/traceroute
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">vi /etc/httpd/conf/httpd.conf
</pre>

change > #AddHandler cgi-script .cgi  
to > AddHandler cgi-script .cgi

Under <Directory &#8220;/var/www/html&#8221;>

change > Options Indexes FollowSymLinks  
to > Options Indexes FollowSymLinks ExecCGI

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /opt/smokeping/etc/basepage.html
</pre>

Change this:

<pre lang="html" line="1"></pre>

To this:

<pre lang="html" line="1"></pre>

or you can Patch the file:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /opt/smokeping/etc/
# vi /opt/smokeping/etc/basepage.patch
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >56,59c56,59
&lt; 
&lt; 
&lt; 
&lt; 
---
> 
> 
> 
> 
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># patch -p1 -i basepage.patch /opt/smokeping/etc/basepage.html
patching file /opt/smokeping/etc/basepage.html
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /opt/smokeping/htdocs/tr.html
</pre>

Change this:

<pre lang="html" line="1">
	

</pre>

To this:

<pre lang="html" line="1">
	

</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ln -s /opt/smokeping/htdocs/tr.html /var/www/html/smokeping/tr.html
</pre>

Lets create a basic Config file for Smokeping to get started:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /opt/smokeping/etc/config
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >*** General ***

owner    = Peter Random
contact  = some@address.nowhere
mailhost = my.mail.host
sendmail = /usr/sbin/sendmail
# NOTE: do not put the Image Cache below cgi-bin
# since all files under cgi-bin will be executed ... this is not
# good for images.
imgcache = /var/www/html/smokeping/img
imgurl   = http://www.how2centos.com/smokeping/img
datadir  = /opt/smokeping/data
piddir  = /opt/smokeping/var
cgiurl   = http://www.how2centos.com/smokeping/smokeping.cgi
smokemail = /opt/smokeping/etc/smokemail
tmail = /opt/smokeping/etc/tmail

# specify this to get syslog logging
syslogfacility = local0

# each probe is now run in its own process
# disable this to revert to the old behaviour
# concurrentprobes = no

*** Alerts ***
to = alertee@address.somewhere
from = smokealert@company.xy

+someloss
type = loss
# in percent
pattern = >0%,*12*,>0%,*12*,>0%
comment = loss 3 times  in a row

*** Database ***

step     = 300
pings    = 20

# consfn mrhb steps total

AVERAGE  0.5   1  1008
AVERAGE  0.5  12  4320
    MIN  0.5  12  4320
    MAX  0.5  12  4320
AVERAGE  0.5 144   720
    MAX  0.5 144   720
    MIN  0.5 144   720

*** Presentation ***

template = /opt/smokeping/etc/basepage.html

+ charts

menu = Charts
title = The most interesting destinations

++ stddev
sorter = StdDev(entries=>4)
title = Top Standard Deviation
menu = Std Deviation
format = Standard Deviation %f

++ max
sorter = Max(entries=>5)
title = Top Max Roundtrip Time
menu = by Max
format = Max Roundtrip Time %f seconds

++ loss
sorter = Loss(entries=>5)
title = Top Packet Loss
menu = Loss
format = Packets Lost %f

++ median
sorter = Median(entries=>5)
title = Top Median Roundtrip Time
menu = by Median
format = Median RTT %f seconds

+ overview

width = 600
height = 50
range = 10h

+ detail

width = 600
height = 200
unison_tolerance = 2

"Last 3 Hours"    3h
"Last 30 Hours"   30h
"Last 10 Days"    10d
"Last 400 Days"   400d

#+ hierarchies
#++ owner
#title = Host Owner
#++ location
#title = Location

*** Probes ***

+ FPing

binary = /usr/sbin/fping

*** Targets ***

menuextra = &lt;a target='_blank' href='/smokeping/tr.html{HOST}' class='{CLASS}' \
onclick="window.open(this.href,this.target, \
'width=800,height=500,toolbar=no,location=no,status=no,scrollbars=no'); \
return false;">(TR)&lt;/a>

probe = FPing

menu = Top
title = Network Latency Grapher
remark = Welcome to the SmokePing website of xxx Company. \
         Here you will learn all about the latency of our network.

+ hosts
menu= Targets

++ How2CentOS

menu = How2CentOS.com
title = How2CentOS.com
alerts = someloss
host = www.how2centos.com

++ CentOS

menu = CentOS.org
title = CentOS.org
alerts = someloss
host = www.centos.org
</pre>

Lets create a service startup script for Smokeping

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/init.d/smokeping
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/bin/bash
#
# chkconfig: 2345 80 05
# Description: Smokeping init.d script
# Hacked by : How2CentOS - http://www.how2centos.com

# Get function from functions library
. /etc/init.d/functions

# Start the service Smokeping
start() {
        echo -n "Starting Smokeping: "
        /opt/smokeping/bin/smokeping >/dev/null 2>&1
        ### Create the lock file ###
        touch /var/lock/subsys/smokeping
        success $"Smokeping startup"
        echo
}

# Restart the service Smokeping
stop() {
        echo -n "Stopping Smokeping: "
        kill -9 `ps ax | grep "/opt/smokeping/bin/smokeping" | grep -v grep | awk '{ print $1 }'` >/dev/null 2>&1 && killall speedy_backend
        ### Now, delete the lock file ###
        rm -f /var/lock/subsys/smokeping
        success $"Smokeping shutdown"
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
        status Smokeping
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

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chmod 755 /etc/init.d/smokeping
</pre>

Finally lets add Apache and Smokeping to startup and get them started:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chkconfig smokeping on
# chkconfig httpd on
# service smokeping start
Starting Smokeping:		<span style="color: #339966;">[  OK  ]</span>
# service httpd start
Starting httpd:		<span style="color: #339966;">[  OK  ]</span>
</pre>

Now browse to your new installed Smokeping and Smoketrace installation

http://www.how2centos.com/smokeping/smokeping.cgi