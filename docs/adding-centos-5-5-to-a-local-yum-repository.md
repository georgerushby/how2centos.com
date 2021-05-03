---
title: Adding CentOS 5.5 to a local YUM Repository
categories:
  - CentOS 5.5 Tutorials
tags:
  - CentOS 5.5
  - yum
---
[:octicons-file-code-24: Source](https://github.com/georgerushby/how2centos.com/blob/main/docs/adding-centos-5-5-to-a-local-yum-repository.md)

## What is CentOS?

CentOS is 100% compatible rebuild of the Red Hat Enterprise Linux, in full compliance with Red Hat's redistribution requirements. CentOS is for people who need an enterprise class operating system stability without the cost of certification and support.

[CentOS Website](http://www.centos.org/)

## What is YUM?

The Yellowdog Updater, Modified (YUM) is an open-source command-line package-management utility for RPM-compatible Linux operating systems and has been released under the GNU General Public License. It was developed by Seth Vidal and a group of volunteer programmers. Though yum has a command-line interface, several other tools provide graphical user interfaces to yum functionality.

[Yum Website](http://yum.baseurl.org/)

Following the release of RHEL 5.5, CentOS 5.5 has just hit the CentOS 5.5 download mirrors. Time to update the [Local CentOS YUM repository](http://www.how2centos.com/creating-a-local-yum-repository-on-centos-5x/) script. This update script, via Rsync, will create a local CentOS 5.5 download mirror. Your local CentOS 5.5 Servers will then be able to to update from this local repository.

## Adding CentOS 5.5 to the YUM repository script

Create the following additional Directories for CentOS 5.5:

```
mkdir -pv /var/www/html/centos/5.5/{os,updates}/{i386,x86_64}
```

Add the CentOS 5.5 repository to the bash script which will rsync your local YUM repository server with a CentOS 5.5 YUM mirror.

CentOS Mirror list <http://www.centos.org/modules/tinycontent/index.php?id=30>

```
vi yum-repo-update.sh
```

```bash
#!/bin/sh
# This script will create a local CentOS mirror via Rsync
# Note: This script will download CentOS 5.5 and 5.4
#

rsync="rsync -avrt --bwlimit=256"

mirror=ftp.is.co.za::IS-Mirror/centos

verlist="5.4 5.5"
archlist="i386 x86_64"
baselist="os updates"
local=/var/www/html/centos/

for ver in $verlist
do
 for arch in $archlist
 do
  for base in $baselist
  do
    remote=$mirror/$ver/$base/$arch/
    $rsync $remote $local/$ver/$base/$arch/
  done
 done
done
```

**NB!** Please read creating a [Local CentOS YUM repository](http://www.how2centos.com/creating-a-local-yum-repository-on-centos-5x/) on CentOS 5.x before implementing.

## Upgrading from CentOS 5.4 ( or CentOS 5.0 / 5.1 / 5.2 / 5.3):

If you are already running CentOS 5.4 or an older CentOS 5 distro, all you need to do is update your machine via yum by running:

```
yum update
```
