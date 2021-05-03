---
id: 1879
title: Installing SSMTP on CentOS 5.6
date: 2011-06-06T12:44:43+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=1879
permalink: /installing-ssmtp-on-centos-5-6/
dsq_thread_id:
  - "323491507"
categories:
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
---
<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="CentOS Logo" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" /><img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2011/06/mail.jpg" alt="" title="mail" width="47" height="48" class="alignleft size-full wp-image-1886" /> This tutorial will guide you through installing SSMTP on CentOS 5.6

SSMTP is an extremely simple MTA to get mail off the system to a mail hub. It contains no suid-binaries or other dangerous things &#8211; no mail spool to poke around in, and no daemons running in the background. Mail is simply forwarded to the configured mailhost. Extremely easy configuration. WARNING: the above is all it does; it does not receive mail, expand aliases or manage a queue.

Firstly lets install the EPEL repository as SSMTP is not native to CentOS 5.6 base installations.

### Install the EPEL i386 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm</pre>

Remove Sendmail as the binaries can conflict.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum remove sendmail
</pre>

Now install SSMTP

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install ssmtp
</pre>

<!--more-->

SSMTP is now installed and all that is let is to configure it.

For a really basic installation uncomment and replace with your mailserver and domain details mailhub and RewriteDomain

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cp /etc/ssmtp/ssmtp.conf /etc/ssmtp/ssmtp.conf.orig
# vi /etc/ssmtp/ssmtp.conf
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >#
# /etc/ssmtp.conf -- a config file for sSMTP sendmail.
#
# See the ssmtp.conf(5) man page for a more verbose explanation of the
# available options.
#

# The person who gets all mail for userids &lt; 500
# Make this empty to disable rewriting.
root=postmaster

# The place where the mail goes. The actual machine name is required
# no MX records are consulted. Commonly mailhosts are named mail.domain.com
# The example will fit if you are in domain.com and your mailhub is so named.
mailhub=mail.how2centos.com

# Example for SMTP port number 2525
# mailhub=mail.your.domain:2525
# Example for SMTP port number 25 (Standard/RFC)
# mailhub=mail.your.domain
# Example for SSL encrypted connection
# mailhub=mail.your.domain:465

# Where will the mail seem to come from?
RewriteDomain=how2centos.com

# The full hostname
#Hostname=

# Set this to never rewrite the "From:" line (unless not given) and to
# use that address in the "from line" of the envelope.
#FromLineOverride=YES

# Use SSL/TLS to send secure messages to server.
#UseTLS=YES

# Use SSL/TLS certificate to authenticate against smtp host.
#UseTLSCert=YES

# Use this RSA certificate.
#TLSCert=/etc/pki/tls/private/ssmtp.pem

# Get enhanced (*really* enhanced) debugging information in the logs
# If you want to have debugging of the config file parsing, move this option
# to the top of the config file and uncomment
#Debug=YES
</pre>