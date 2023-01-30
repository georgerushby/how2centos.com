---
title: Apache with mod_pagespeed on CentOS 5.5
categories:
  - CentOS 5.5 Tutorials
tags:
  - Apache
  - CentOS 5.5
  - Google
---
[:octicons-file-code-24: Source](https://github.com/georgerushby/how2centos.com/blob/main/docs/apache-with-mod-pagespeed-on-centos-5-5.md)

mod_pagespeed is an open-source Apache module that automatically optimizes web pages and resources on them. It does this by rewriting the resources using filters that implement web performance best practices.

Read more <http://code.google.com/speed/page-speed/docs/module.html>

This tutorial will show you how to install Apache with mod_pagespeed on CentOS 5.5.

Let's begin.

## Installing Apache

```
yum install yum-priorities
```

```
yum install httpd
```

## Add the mod_pagespeed YUM repository

```
vi /etc/yum.repos.d/mod-pagespeed.repo
```

CentOS 5.5 32 Bit

```
[mod-pagespeed]
name=mod-pagespeed
baseurl=http://dl.google.com/linux/mod-pagespeed/rpm/stable/i386
enabled=1
gpgcheck=0
```

CentOS 5.5 64 Bit

```
[mod-pagespeed]
name=mod-pagespeed
baseurl=http://dl.google.com/linux/mod-pagespeed/rpm/stable/x86_64
enabled=1
gpgcheck=0
```

## Installing mod_pagespeed

```
yum install mod-pagespeed-beta
```

Done! All that is left is to read more about using mod_pagespeed, its configuration and configuring handlers at [Using mod_pagespeed](http://code.google.com/speed/page-speed/docs/using_mod.html)