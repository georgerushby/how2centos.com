---
id: 2844
title: Install Nginx with PHP-FPM on CentOS 6
date: 2013-09-08T19:31:56+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2844
permalink: /install-nginx-php-fpm-centos-6/
dsq_thread_id:
  - "1734082728"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS
  - CentOS 6.0 Tutorials
  - CentOS 6.1 Tutorials
  - CentOS 6.2 Tutorials
  - CentOS 6.3 Tutorials
  - CentOS 6.4 Tutorials
---
### What is Nginx?

Nginx (pronounced “engine x”) is a free, open-source, high-performance HTTP server. Nginx is known for its stability, rich feature set, simple configuration, and low resource consumption.

### What is PHP-FPM?

PHP-FPM (FastCGI Process Manager) is an alternative PHP FastCGI implementation with some additional features useful for sites of any size, especially busier sites. <a href="http://php-fpm.org/" title="http://php-fpm.org/" target="_blank"></a>

### Install the EPEL x86_64 YUM Repository

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true " ># rpm -Uvh http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm</pre>

### Install Nginx

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true "># yum install nginx
</pre>

### Install PHP and PHP-FPM

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true "># yum install php php-mysql php-fpm php-common
</pre>

<!--more-->

### Configure Nginx and PHP-FPM

Edit the www.conf pool and comment out the listen on TCP socket and add a unix socket listen = /var/run/php5-fpm.sock

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true "># vi /etc/php-fpm.d/www.conf
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >; Start a new pool named 'www'.
[www]

; The address on which to accept FastCGI requests.
; Valid syntaxes are:
;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific address on
;                            a specific port;
;   'port'                 - to listen on a TCP socket to all addresses on a
;                            specific port;
;   '/path/to/unix/socket' - to listen on a unix socket.
; Note: This value is mandatory.
;listen = 127.0.0.1:9000
listen =  /var/run/php5-fpm.sock</pre>

Create a Nginx config file based on your individual site. Below is a config file specific to WordPress running the W3 Total Cache plugin. Note the fastcgi_pass is the same as the unix socket specified in the php-fpm www.conf pool.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true "># vim /etc/nginx/conf.d/how2centos.conf
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >server {
    # Redirect how2centos.com to www.how2centos.com
    server_name      how2centos.com;
    rewrite ^(.*)    http://www.how2centos.com$1 permanent;
}
server {
    server_name    www.how2centos.com;

    access_log   /var/log/nginx/how2centos.com/access.log;
    error_log    /var/log/nginx/how2centos.com/error.log;

    root /var/www/www.how2centos.com;
    index index.php;

    gzip                on;
    gzip_disable        "msie6";
    gzip_vary           on;
    gzip_proxied        any;
    gzip_comp_level     5;
    gzip_buffers        16 8k;
    gzip_http_version   1.0;
    gzip_types          text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript image/png image/gif image/jpeg;

    set $cache_uri $request_uri;

    # POST requests and urls with a query string should always go to PHP
    if ($request_method = POST) {
        set $cache_uri 'null cache';
    }
    if ($query_string != "") {
        set $cache_uri 'null cache';
    }

    # Don't cache uris containing the following segments
    if ($request_uri ~* "(/wp-admin/|/xmlrpc.php|/wp-(app|cron|login|register|mail).php|wp-.*.php|/feed/|index.php|wp-comments-popup.php|wp-links-opml.php|wp-locations.php|sitemap(_index)?.xml|[a-z0-9_-]+-sitemap([0-9]+)?.xml)") {
        set $cache_uri 'null cache';
    }

    # Don't use the cache for logged in users or recent commenters
    if ($http_cookie ~* "comment_author|wordpress_[a-f0-9]+|wp-postpass|wordpress_logged_in") {
        set $cache_uri 'null cache';
    }

    # Use cached or actual file if they exists, otherwise pass request to WordPress
    location / {
        try_files /wp-content/cache/page_enhanced/${host}${cache_uri}_index.html $uri $uri/ /index.php?$args ;
    }

    location ~ ^/wp-content/cache/minify/[^/]+/(.*)$ {
        try_files $uri /wp-content/plugins/w3-total-cache/pub/minify.php?file=$1;
    }

    location = /favicon.ico { log_not_found off; access_log off; }
    location = /robots.txt  { log_not_found off; access_log off; }

    location ~ .php$ {
        fastcgi_index   index.php;
        fastcgi_pass    unix:/var/run/php5-fpm.sock;
        include         fastcgi_params;
        fastcgi_param   SCRIPT_FILENAME    $document_root$fastcgi_script_name;
        fastcgi_param   SCRIPT_NAME        $fastcgi_script_name;
    }

    # Cache static files for as long as possible
    location ~* .(ogg|ogv|svg|svgz|eot|otf|woff|mp4|ttf|css|rss|atom|js|jpg|jpeg|gif|png|ico|zip|tgz|gz|rar|bz2|doc|xls|exe|ppt|tar|mid|midi|wav|bmp|rtf)$ {
        expires max; log_not_found off; access_log off;
    }
}
</pre>

Don&#8217;t forget to create your log directory if you&#8217;re not using the defualt.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true "># mkdir /var/log/nginx/how2centos.com/
</pre>

Check your configuration for any errors.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true "># nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
</pre>

Enable php-fpm and Nginx at startup.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true "># chkconfig nginx on
# chkconfig php-fpm on
</pre>

Start php-fpm and Nginx.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true "># service php-fpm start
# service nginx start
</pre>