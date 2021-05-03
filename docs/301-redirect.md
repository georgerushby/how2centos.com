---
title: 301 Redirect
tags:
  - Apache
---
[:octicons-file-code-24: Source](https://github.com/georgerushby/how2centos.com/blob/main/docs/301-redirect.md)

Search engines may think www.example.co.za and example.co.za are two different sites.You should set up a permanent redirect (technically called a 301 redirect) between these sites. Once you do that, you will get full search engine credit for your work on these sites.

In example.co.za html root folder create the following:

```
vi .htaccess
```

```
RewriteEngine on
rewritecond %{http_host} ^example.co.za [nc]
rewriterule ^(.*)$ http://www.example.co.za/$1 [r=301,nc]
```
