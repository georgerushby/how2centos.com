---
title: Adding CentOS 5.4 to local YUM Repository
tags:
  - centos 5.4
  - yum
---
[:octicons-file-code-24: Source](https://github.com/georgerushby/how2centos.com/blob/main/docs/adding-centos-5-4-to-a-local-yum-repository.md)

## What is CentOS

CentOS is 100% compatible rebuild of the Red Hat Enterprise Linux, in full compliance with Red Hat's redistribution requirements. CentOS is for people who need an enterprise class operating system stability without the cost of certification and support. 

Following the release of RHEL 5.4, CentOS 5.4 has just hit the CentOS mirrors. Time to update the [Local CentOS YUM repository](http://www.how2centos.com/creating-a-local-yum-repository-on-centos-5x/) script.

## Adding CentOS 5.4 to the YUM repository script

Create the following additional Directories for CentOS 5.4:

```
mkdir -p /var/www/html/centos/5.4/os/i386
```
```
mkdir -p /var/www/html/centos/5.4/updates/i386
```
```
mkdir -p /var/www/html/centos/5.4/os/x86_64
```
```
mkdir -p /var/www/html/centos/5.4/updates/x86_64
```

Add the CentOS 5.4 repository to the bash script which will rsync your local YUM repository server with a CentOS 5.4 YUM mirror.

CentOS Mirror list  <http://www.centos.org/modules/tinycontent/index.php?id=30>

```
vi yum-repo-update.sh
```

``` bash
#!/bin/sh

rsync="rsync -avrt --bwlimit=256"

mirror=ftp.is.co.za::IS-Mirror/centos

verlist="5.3 5.4"
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

## Upgrading from CentOS 5.3 ( or CentOS 5.0 / 5.1 / 5.2 )

If you are already running CentOS 5.3 or an older CentOS 5 distro, all you need to do is update your machine via yum by running :

```
yum update
```