---
title: Adding CentOS 5.6 to a local YUM Repository
categories:
  - CentOS 5.6 Tutorials
---
[:octicons-file-code-24: Source](https://github.com/georgerushby/how2centos.com/blob/main/docs/adding-centos-5-6-to-a-local-yum-repository.md)

**NB!** Please read creating a [Local CentOS YUM repository](http://www.how2centos.com/creating-a-local-yum-repository-on-centos-5x/) on CentOS 5.x before implementing.

## Adding CentOS 5.6 to the YUM repository script

Create the following additional directories for CentOS 5.6:

```
mkdir -pv /var/www/html/centos/5.6/{os,updates}/{i386,x86_64}
```

Add the CentOS 5.6 repository to the bash script which will rsync your local YUM repository server with a CentOS 5.6 YUM mirror.

CentOS Mirror list <http://www.centos.org/modules/tinycontent/index.php?id=30>

```
vi yum-repo-update.sh
```

```bash
#!/bin/sh
# This script will create a local CentOS mirror via Rsync
# Note: This script will download CentOS 5.6
#

rsync="rsync -avrt --bwlimit=256"

mirror=ftp.is.co.za::IS-Mirror/centos

verlist="5.6"
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

### Upgrading from CentOS 5.5

If you are already running CentOS 5.5 or an older CentOS 5 distro, all you need to do is update your machine via yum by running :

```
yum update
```