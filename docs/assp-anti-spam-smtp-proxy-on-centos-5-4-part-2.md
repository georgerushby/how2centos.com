---
id: 756
title: ASSP (Anti-Spam SMTP Proxy) On CentOS 5.4 part 2
date: 2010-04-03T14:32:07+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=756
permalink: /assp-anti-spam-smtp-proxy-on-centos-5-4-part-2/
dsq_thread_id:
  - "82120364"
categories:
  - CentOS 5.4 Tutorials
tags:
  - anti-spam
  - assp
  - centos 5.4
  - Microsoft Exchange 2007
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/02/Assp.png" alt="" title="Assp" width="90" height="40" class="alignleft size-full wp-image-730" />](http://www.how2centos.com/wp-content/uploads/2010/02/Assp.png) [<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif)This is a follow up post to [Fight Spam With #ASSP (Anti-Spam SMTP Proxy) On CentOS 5.4](http://www.how2centos.com/fight-spam-with-assp-anti-spam-smtp-proxy-on-centos-5-4/). If you haven&#8217;t already installed ASSP do so before continuing with this How To and remember that you need a working installation of Microsoft Exchange 2007 Server as well. This How To can be implemented on a live Microsoft Exchange 2007 server while it&#8217;s running. Leave the original ports (25 In and 25 Out, OWA and/or internal usage ports) alone and create new ones. Once you&#8217;re ready to go simply activate the new ports, then deactivate the original ports, your users will never see a glitch in service and will make rolling back just as seamless.

**Preliminary Note:**

I am using a CentOS 5.4 i386 base installation with ASSP (Anti-Spam SMTP Proxy) and Microsoft Exchange 2007 Server already installed and configured in this tutorial. 

* assp001.example.co.za (IP 10.0.0.100): CentOS 5.4 i386 ASSP installation  
* exchange001.example.co.za (IP 10.0.0.101): Microsoft Exchange 2007 server  
<!--more-->

  
**Setup Sendmail as your MTA relay**

In the previous How To we disabled Sendmail because it used the same port that we wanted ASSP to listen on. What we need to do is some configuration changes and start/install it again to be our MTA. Edit your sendmail.mc configuration file and change the following values (replace example.co.za with your domain).

If you uninstalled Sendmail as per the previous How To then reinstall it.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install sendmail sendmail-cf
</pre>

Lets edit the Sendmail configuration file.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/mail/sendmail.mc
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >DAEMON_OPTIONS(`Name=MTA,Port=125')
MASQUERADE_AS(`example.co.za')dnl
FEATURE(masquerade_entire_domain)dnl
MASQUERADE_DOMAIN(example.co.za)dnl
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># echo "example.co.za" >> /etc/mail/relay-domains
</pre>

Finally build the Sendmail database.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">#  m4 /etc/mail/sendmail.mc > /etc/mail/sendmail.cf
</pre>

Lets start Sendmail and test that it&#8217;s listening on the correct port.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service sendmail start
# chkconfig sendmail on
# telnet localhost 125

Trying 127.0.0.1...
Connected to localhost.localdomain (127.0.0.1).
Escape character is '^]'.
220 assp001.example.co.za ESMTP Sendmail 8.13.8/8.13.8; Thu, 1 Apr 2010 08:09:30 +0200
</pre>

So that&#8217;s the Sendmail MTA setup done, now onto the Exchange 2007 configuration.

**Setup Exchange 2007** 

Remember that this setup assumes that you already have a working installation of Microsoft Exchange 2007

1) Create an additional incoming (i.e. &#8220;ASSP Inbound&#8221; on port 125) using the Exchange Management Console.  
2) Create an additional outgoing connector (i.e. &#8220;ASSP Outbound&#8221;) using the Exchange Management Console. Set the outbound connector to transfer to a smarthost on 192.168.0.2 (ASSP) and check the box for to use the remote server DNS on the smarthost. The outbound connector will default to port 25 which we will change in the next step.  
3) Change the outbound connector &#8220;ASSP Outbound&#8221; port to 325 via the Exchange Management Shell using the Set-SendConnector command. 

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >Set-SendConnector -identity "ASSP Outbound" -port 325. 
</pre>

**In ASSP Admin panel, make the following changes**  
**</p> 

  * In NETWORK SETUP
</strong>

1) Insure the SMTP LISTEN PORT is 10.0.0.100:25  
2) Insure the SMTP DESTINATION is 10.0.0.101:125  
**</p> 

  * In RELAYING
</strong> 

3) Insure the RELAY PORT is 10.0.0.100:325  
4) Insure the RELAY HOST is 10.0.0.100:125

Here is a flow description of how everything fits together  
**</p> 

  * Incoming Mail
</strong>

Internet to Firewall on 25 **&#8211;>** Firewall passes to ASSP on 10.0.0.100 Port 25 **&#8211;>** relays to Exchange listening on 10.0.0.101 port 125  
**</p> 

  * Outgoing Mail
</strong>

Exchange from 10.0.0.101 port 325 smarthosts **&#8211;>** ASSP listening on 10.0.0.100 port 325 and relays **&#8211;>** to Sendmail MTA on listening on 10.0.0.100 port 125 **&#8211;>** MTA transmits to the internet.  
**</p> 

  * Exchange Settings &#8211; Final
</strong>

1) Disable outbound connector on port 25 in and enable &#8220;ASSP Outbound&#8221; connector in the Exchange Management Console.