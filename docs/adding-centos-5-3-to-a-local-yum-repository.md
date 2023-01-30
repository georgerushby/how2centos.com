---
title: Adding CentOS 5.3 to Local YUM Repository
categories:
  - CentOS 5.3 Tutorials
tags:
  - centos 5.3
  - yum
---
[:octicons-file-code-24: Source](https://github.com/georgerushby/how2centos.com/blob/main/docs/adding-centos-5-3-to-a-local-yum-repository.md)

CentOS 5.3 has just been released and that means it's time to update our [Local CentOS 5.x YUM repo](http://www.how2centos.com/creating-a-local-yum-repository-on-centos-5x/).

## Adding CentOS 5.3 to the YUM repository script

Create the following additional Directories:

```
mkdir -p /var/www/html/centos/5.3/os/i386
```
```
mkdir -p /var/www/html/centos/5.3/updates/i386
```
```
mkdir -p /var/www/html/centos/5.3/os/x86_64
```
```
mkdir -p /var/www/html/centos/5.3/updates/x86_64
```

Add the CentOS 5.3 repo to the bash script which will rsync your local YUM Repo server with a CentOS 5.3 YUM mirror.

CentOS Mirror list <http://www.centos.org/modules/tinycontent/index.php?id=30>

```
vi yum-repo-update.sh
```

```
#!/bin/sh

rsync="rsync -avrt --bwlimit=256"

mirror=ftp.is.co.za::IS-Mirror/centos

verlist="5 5.2 5.3&lt;/span>"
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

**NB!** Please read creating a <a href="http://www.how2centos.com/creating-a-local-yum-repository-on-centos-5x/">Local CentOS YUM repository</a> on CentOS 5.x before implementing.</p> 


## Upgrading from CentOS 5.2 ( or CentOS 5.0 / 5.1 ):

If you are already running CentOS 5.3 or an older CentOS 5 distro, all you need to do is update your machine via yum by running :

```
yum update
```