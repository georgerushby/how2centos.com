---
id: 2912
title: How to setup a SFTP server on CentOS
date: 2014-07-29T17:31:34+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2912
permalink: /setup-sftp-server-centos/
dsq_thread_id:
  - "2882839973"
categories:
  - CentOS
---
### What is SFTP?

SFTP, is the acronym for SSH File Transfer Protocol, or Secure File Transfer Protocol, is a protocol packaged with SSH that works in a similar way as FTP but over a secure connection. The advantage is the ability to leverage a secure connection to transfer files. In almost all cases, SFTP is preferable to FTP because of its underlying security features and ability to piggy-back on an SSH connection. 

FTP is an insecure protocol that shouldn&#8217;t be used.

### Enable PasswordAuthentication in the sshd config file

Backup the current sshd_config

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true "># cp /etc/ssh/sshd_config /etc/ssh/sshd_config.orig
</pre>

Change it to READ-ONLY to ensure it don&#8217;t get overwritten

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># chmod a-w /etc/ssh/sshd_config.orig
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># vim /etc/ssh/sshd_config
</pre>

Find the line with the phrase PasswordAuthentication and make it read:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:vim decode:true " >PasswordAuthentication yes
</pre>

Save your new sshd_config file and then restart the host machine&#8217;s ssh service:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># service sshd restart

Stopping sshd:                                             [  OK  ]
Starting sshd:                                             [  OK  ]
</pre>

### Connect to your host and login to your user account

To open an SFTP shell terminal as <username> on the host machine, open a Terminal on your client machine and enter the following command, replacing <hostname> with your host machine&#8217;s IP address hostname:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># sftp &lt;username>@&lt;hostname>
Connected to host.
sftp>
</pre>