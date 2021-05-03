---
id: 596
title: How to keep a detailed audit trail on your CentOS 5.4 servers
date: 2009-11-03T23:59:42+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=596
permalink: /how-to-keep-a-detailed-audit-trail-on-your-centos-5-4-servers/
dsq_thread_id:
  - null
categories:
  - CentOS 5.4 Tutorials
tags:
  - centos 5.4
  - psacct
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="centos" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif)The psacct package contains several utilities for monitoring process activities, including ac, lastcomm, accton and sa. The ac command displays statistics about how long users have been logged on. The lastcomm command displays information about previous executed commands. The accton command turns process accounting on or off. The sa command summarizes information about previously executed commmands. Install the psacct package if you&#8217;d like to use its utilities for monitoring process activities on your CentOS 5.4 system.

**Installing the psacct package**

Use yum command if you are using CentOS 5.4 / Fedora 11 / RHEL 5.4:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install psacct
</pre>

<!--more-->

  
**Start psacct service**

By default service is not started on RHEL 5.4 / Fedora 11 / CentOS 5.4 you need to start psacct service manually. Type the following two commands to create /var/account/pacct file and start services:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chkconfig psacct on
# service psacct start
</pre>

**Display statistics about users&#8217; connect time**

ac prints out a report of connect time (in hours) based on the logins/logouts in the current wtmp file. A total is also printed out.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ac
</pre>

<pre class="brush: text">total       95.08
</pre>

Print totals for each day rather than just one big total at the end.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ac -d
</pre>

<pre class="brush: text">Jul  3  total     1.17
Jul  4  total     2.10
Jul  5  total     8.23
Jul  6  total     2.10
Jul  7  total     0.30
</pre>

Print time totals for each user in addition to the usual everything-lumped-into-one value.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># ac -p
</pre>

<pre class="brush: text">bob       8.06
goff      0.60
maley     7.37
root      0.12
total    16.15
</pre>

**Find out information about previously executed user commands**

lastcomm prints out information about previously executed commands. If no arguments are specified, lastcomm will print info about all of the commands in acct (the record file).

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># lastcomm root
</pre>

<pre class="brush: text">userhelper        S   X	root  pts/0      0.00 secs Mon Nov 13 23:58
userhelper        S     root  pts/0      0.00 secs Mon Nov 13 23:45
rpmq                    root  pts/0      0.01 secs Mon Nov 13 23:45
rpmq                    root  pts/0      0.00 secs Mon Nov 13 23:45
rpmq                    root  pts/0      0.01 secs Mon Nov 13 23:45
gcc                     root  pts/0      0.00 secs Mon Nov 13 23:45
which                   root  pts/0      0.00 secs Mon Nov 13 23:44
bash               F    root  pts/0      0.00 secs Mon Nov 13 23:44
ls                      root  pts/0      0.00 secs Mon Nov 13 23:43
rm                      root  pts/0      0.00 secs Mon Nov 13 23:43
vi                      root  pts/0      0.00 secs Mon Nov 13 23:43
ping              S     root  pts/0      0.00 secs Mon Nov 13 23:42
ping              S     root  pts/0      0.00 secs Mon Nov 13 23:42
ping              S     root  pts/0      0.00 secs Mon Nov 13 23:42
cat                     root  pts/0      0.00 secs Mon Nov 13 23:42
netstat                 root  pts/0      0.07 secs Mon Nov 13 23:42
su                S     root  pts/0      0.00 secs Mon Nov 13 23:38
</pre>

<pre class="brush: text">For each entry the following information is printed:
          + command name of the process
          + flags, as recorded by the system accounting routines:
               S -- command executed by super-user
               F -- command executed after a fork but without a following exec
               C -- command run in PDP-11 compatibility mode (VAX only)
               D -- command terminated with the generation of a core file
               X -- command was terminated with the signal SIGTERM
          + the name of the user who ran the process
          + time the process exited
</pre>

Search the accounting logs by command name:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># lastcomm rm
# lastcomm passwd
</pre>

<pre class="brush: text">rm                      root     pts/0      0.00 secs Tue Nov  3 07:34
rm                      root     pts/0      0.00 secs Tue Nov  3 07:34
rm                      root     pts/0      0.00 secs Tue Nov  3 07:33
rm                      root     pts/0      0.00 secs Tue Nov  3 07:33
rm                      root     pts/0      0.00 secs Tue Nov  3 07:14
rm                      root     pts/0      0.00 secs Tue Nov  3 07:14
</pre>

Search the accounting logs by terminal name pts/0

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># lastcomm pts/0
</pre>

**Summarizes accounting information**

sa summarizes information about previously executed commands as recorded in the acct file. In addition, it condenses this data into a summary file named savacct which contains the number of times the command was called and the system resources used. The information can also be summarized on a per-user basis; sa will save this information into a file named usracct.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># sa
</pre>

<pre class="brush: text">26100      26.62re       0.00cp      931k   who
   75980     161.69re       0.00cp      979k   grep
   23756       3.93re       0.00cp      938k   cut
    1283     815.91re       0.00cp     1327k   crond*
       6       4.89re       0.00cp     2102k   sshd*
       4       0.01re       0.00cp     1274k   grotty
       2      33.19re       0.00cp     1624k   scp
      25       0.04re       0.00cp      447k   mail
      15       0.05re       0.00cp      472k   ntpdate
</pre>

Take example of first line:

<pre class="brush: text">26100      26.62re       0.00cp      931k   who
</pre>

Where,

* 26.62re &#8220;real time&#8221; in wall clock minutes  
* 0.00cp sum of system and user time in cpu minutes  
* 931k cpu-time averaged core usage, in 1k units  
* who command name

For each command in the accounting file, print the userid and command name.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># sa -u
</pre>

<pre class="brush: text">root       0.00 cpu      595k mem accton
root       0.00 cpu    12488k mem initlog
root       0.00 cpu    12488k mem initlog
root       0.00 cpu    12482k mem touch
root       0.00 cpu    13226k mem psacct
root       0.00 cpu      595k mem consoletype
root       0.00 cpu    13192k mem psacct           *
root       0.00 cpu    13226k mem psacct
root       0.00 cpu    12492k mem chkconfig
</pre>

Print the number of processes and number of CPU minutes on a per-user basis.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># sa -m
</pre>

<pre class="brush: text">973902  107317.71re    1337.92cp     2101k
root                               781510   95856.57re     559.01cp     1795k
apache                                334    9007.96re     513.05cp    25916k
nagios                             192035    2447.62re     265.85cp     3303k
smmsp                                  17       0.67re       0.00cp     2033k
sshd                                    6       4.89re       0.00cp     2102k
</pre>