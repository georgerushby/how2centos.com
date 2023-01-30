---
title: Installing AIDE on CentOS 5.2
categories:
  - CentOS 5.2 Tutorials
  - CentOS 5.3 Tutorials
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
tags:
  - aide
  - centos 5.2
---
[:octicons-file-code-24: Source](https://github.com/georgerushby/how2centos.com/blob/main/docs/installing-aide-on-centos-5-2.md)

AIDE (Advanced Intrusion Detection Environment) is a free replacement for Tripwire. It does the same things as the semi-free Tripwire and more. This is a must have when maintaining the integrity of your servers.

## Installing Aide

```
yum install aide
```

## Initializing Aide's Records

The next thing we need to do is create the initial AIDE database. Run the following command:

```
/usr/sbin/aide --init
```

This will take a little bit of time to run, depending on the size of your file system and you'll have some disk churn while aide interrogates your system and creates a baseline database. Once this is done, we're going to test by doing an initial query of the system. Run the command below:

```
cp /var/lib/aide/aide.db.new.gz /var/lib/aide/aide.db.gz
```
```
/usr/sbin/aide --check
```

This copies the initial database to the current database, then checks them against each other. In theory you should not have any differences. If you do, investigate them. As we're still setting this up, they're likely to be mundane .viminfo files or something similar. Keep in mind that when you update applications via yum update that you may see aide go a bit nuts, just as tripwire or others would. You&#8217;re replacing files on your system when you update, and this is exactly what aide is designed to warn you about. In a perfect world, you should get some output like the text below:

```
aide --check
```

## Add the following to Crontab

```
crontab -e
```

```
# Daily AIDE integrity check
0  1  * * * /usr/sbin/aide --check | /bin/mail -s "$HOSTNAME - Daily AIDE integrity check" admin@example.com
```

This runs a check once a day.
