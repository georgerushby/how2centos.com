---
id: 718
title: 'Fight Spam: ASSP (Anti-Spam SMTP Proxy) On CentOS 5.4'
date: 2010-02-28T16:50:34+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=718
permalink: /fight-spam-with-assp-anti-spam-smtp-proxy-on-centos-5-4/
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
  - "71034887"
categories:
  - CentOS 5.4 Tutorials
tags:
  - anti-spam
  - assp
  - centos 5.4
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/02/Assp.png" alt="" title="Assp" width="90" height="40" class="alignleft size-full wp-image-730" />](http://www.how2centos.com/wp-content/uploads/2010/02/Assp.png) [<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif)The [Anti-Spam SMTP Proxy (ASSP)](http://sourceforge.net/apps/mediawiki/assp/index.php?title=Main_Page) server project is an Open Source, Perl based, platform-independent transparent SMTP proxy server that leverages numerous methodologies and technologies to both rigidly and adaptively identify e-mail spam. ASSP is easy to set up because it requires only minor changes to the configuration of your your Mail Transfer Agent.

Also please read the [ASSP documentation](http://sourceforge.net/apps/mediawiki/assp/index.php?title=ASSP_Documentation)

**Preliminary Note**

In this tutorial I use a base 32 bit server install with the hostname server1.example.co.za with the IP address 10.0.0.10. These settings might differ for you, so you have to replace them where appropriate.

Adjust any package names if installing the EL4 or 64 bit packages. If any dependencies are unsatisfied, install the required packages and retry  
<!--more-->

  
If you have already installed the RPM Forge repository do so by running the following command

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># rpm -Uhv http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.3.6-1.el5.rf.i386.rpm
</pre>

Now lets install the Perl components needed to run ASSP (Anti-Spam SMTP Proxy) on CentOS 5.4. Firstly we upgrade Perl and then install the Perl modules.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum upgrade perl
# yum install perl-IO-Compress perl-Net-DNS perl-Email-Valid perl-File-ReadBackwards perl-Mail-SPF-Query perl-Mail-SRS perl-LDAP perl-MIME-tools perl-File-Scan-ClamAV
</pre>

If and in order to run ClamAV anti-virus along with ASSP (Anti-Spam SMTP Proxy) install the ClamAV RPM

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install clamd
# chkconfig clamd on
# service clamd start
</pre>

Next we&#8217;ll download and install ASSP (Anti-Spam SMTP Proxy)

Firstly check and grab the link for the latest version of ASSP on thier website, the version used in this post is ASSP Version: 1.6.5.5(1.0.02). Copy the direct download link for the latest version and download it.

Lets get ASSP.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://downloads.sourceforge.net/project/assp/ASSP%20Installation/ASSP%201.6.5.5/ASSP_1.6.5.5-Install.zip?use_mirror=ufpr
</pre>

Make some preparations. Create the following directories that ASSP (Anti-Spam SMTP Proxy) will use to maintain the SPAM

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mkdir -p /usr/share/assp/spam
# mkdir /usr/share/assp/notspam
# mkdir /usr/share/assp/errors
# mkdir /usr/share/assp/errors/spam
# mkdir /usr/share/assp/errors/notspam
</pre>

Now unpack ASSP.

\# unzip ASSP_1.6.5.5-Install.zip  
\# cd ASSP_1.6.5.5-Install

And put ASSP in place.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mv -f ASSP/* /usr/share/assp
</pre>

Create following startup scripts. 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/init.d/assp
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#!/bin/bash
# 
# chkconfig: 2345 80 05
# Description: ASSP init.d script
# Hacked by : How2CentOS - http://www.how2centos.com

# Get function from functions library
. /etc/init.d/functions

# Start the service ASSP
start() {
        echo -n "Starting ASSP server: "
        cd /usr/share/assp
        perl assp.pl 2>&1 > /dev/null &
        ### Create the lock file ###
        touch /var/lock/subsys/ASSP
        success $"ASSP server startup"
        echo
}

# Restart the service ASSP
stop() {
        echo -n "Stopping ASSP server: "
        kill -9 `ps ax | grep "perl assp.pl" | grep -v grep | awk '{ print $1 }'`
        ### Now, delete the lock file ###
        rm -f /var/lock/subsys/ASSP
        success $"ASSP server shutdown"
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
        status ASSP
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

Lets start ASSP for the first time.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /usr/share/assp
# perl assp.pl
</pre>

You should be able to connect to ASSP web interface at this point. http://server1.example.co.za:55555. Specify anything for &#8220;username&#8221;, default &#8220;password&#8221; is **nospam4me**

At this point you shall see that ASSP is unable to bind to port 25. We need to stop and disable CentOS default mailer Sendmail.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service sendmail stop
# chkconfig sendmail off
</pre>

or

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum remove sendmail
</pre>

Now let&#8217;s shutdown ASSP with &#8220;Ctrl-C&#8221;. Now add the init script to chkconfig, set the run levels and start ASSP. 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chmod 755 /etc/init.d/assp
# chkconfig --add assp
# chkconfig assp on
# service assp start
</pre>

Now lets do some minimal ASSP Configuration, point you browser to http://server1.example.co.za:55555

Network Setup

* SMTP Listen Port 25  
* SMTP Destination 25 

SMTP Session Limits  
Testmode

* All Testmode ON &#8211; Running in this mode for two weeks to build the spamdb and whitelist 

SPAM Control 

* Prepend Spam Subject {ASSP-SPAM}

Copy Spam & Ham  
Spam Lover/Hater  
No Processing  
Redlisting/Whitelisting  
Relaying  
Recipients  
Validate Helo  
Validate Sender  
IP Blocking  
Penalty Box  
Delaying  
SPF/SRS  
DNSSBL  
URIBL  
Attachment Blocking  
ClamAV 

* Port or file socket for ClamAV (AvClamdPort) &#8211; /var/run/clamav/clamd.sock 

Regex Filters / Spambomb  
Bayesian Options  
Backscatter Detection  
Email Interface  
File Paths  
Collecting  
Logging  
LDAP Setup  
DNS Setup  
Server Setup

After a week rebuild the bayes database. Check the directories /usr/share/assp/spam and nospam for wrong entries, if good mail ends up in the spam directory please move it to the nospam directory and vice versa. After that do:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /usr/share/assp
# perl rebuildspamdb.pl
</pre>

Finally let&#8217;s add the bayes database rebuild to crontab.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># crontab -e
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#       minute (0-59),
#       |       hour (0-23),
#       |       |       day of the month (1-31),
#       |       |       |       month of the year (1-12),
#       |       |       |       |       day of the week (0-6 with 0=Sunday).
#       |       |       |       |       |       commands
# -----------[ cron jobs  ]------------ #

# Weekly rebuild of the bayes database
	0 	  */2 	  * 	  * 	  * 	   cd /usr/share/assp && perl rebuildspamdb.pl
</pre>