---
id: 844
title: 'Installing Redmine &#038; Subversion on CentOS 5.5'
date: 2010-07-25T13:24:55+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=844
permalink: /installing-redmine-subversion-on-centos-5-5/
sidebar_layout:
  - ""
the_sidebar:
  - ""
lower_sidebar:
  - ""
content_sidebar:
  - ""
colorscheme:
  - ""
full_width_widget:
  - ""
hide_bottom_sidebars:
  - ""
featureboxes:
  - ""
carousel_items:
  - ""
carousel_mode:
  - ""
carousel_ngen_gallery:
  - ""
featuretitle:
  - ""
featuretext:
  - ""
featuremedia:
  - ""
dsq_thread_id:
  - "124716880"
categories:
  - CentOS 5.5 Tutorials
tags:
  - CentOS 5.5
  - Redmine
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif) In this CentOS 5.5 tutorial we will be installing Redmine and Subversion with LDAP authentication on a CentOS 5.5 i386 server. The assumption is that you have a basic to medium understanding of Apache and MySQL but if you follow this tutorial you should be able to complete the task successfully. A bit on the software that we&#8217;ll be using:

**Redmine**  
Redmine is a flexible project management web application. Written using Ruby on Rails framework, it is cross-platform and cross-database. An online demo can be found here:  
<http://demo.redmine.org/>

**Subversion**  
Subversion is a free/open-source version control system. That is, Subversion manages files and directories, and the changes made to them, over time. This allows you to recover older versions of your data, or examine the history of how your data changed.  
<http://subversion.apache.org/> 

**Preliminary Note:**  
I am using a CentOS 5.5 i386 base installation in this tutorial.

* svn.how2centos.com (IP 10.0.0.100): CentOS 5.5 i386 base installation  
* ldap.how2centos.com (IP 10.0.0.100): CentOS 5.5 i386 base installation  
* redmine.how2centos.com (IP 10.0.0.100): CentOS 5.5 i386 base installation  
<!--more-->

Lets begin by installing the framework required by the Redmine, Subversion and LDAP platform.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install yum-priorities
# rpm -Uhv http://apt.sw.be/redhat/el5/en/i386/rpmforge/RPMS/rpmforge-release-0.3.6-1.el5.rf.i386.rpm
# rpm -Uvh http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-4.noarch.rpm
# yum update
# yum install mysql mysql-server
# yum install httpd
# yum install gcc-c++
# yum install ImageMagick ImageMagick-devel
# yum install subversion mod_dav_svn
# yum install perl-HTML-Parser perl-SVN-Notify
# yum install ruby rubygems rubygem-rails rubygem-sqlite3-ruby ruby-devel ruby-mysql
</pre>

Next a couple of Ruby Gems

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># gem install rack -v 1.0.1
# gem install cgi_multipart_eof_fix
# gem install coderay
# gem install thin
</pre>

Now lets add the software to startup and start MySQL and Apache.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># thin install
# chkconfig thin on
# chkconfig mysqld on
# chkconfig httpd on
# service mysqld start
# service httpd start
</pre>

Configure Thin 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># thin config -C /etc/thin/(config-name).yml -c (rails-app-root-path) &#45;&#45;servers (number-of-threads) -e (environment)
</pre>

(application-name) = redmine  
(rails-app-root-path) = /opt/redmine  
(number-of-threads) = 3  
(environment) = production

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># thin config -C /etc/thin/redmine.yml -c /opt/redmine &#45;&#45;servers 3 -e production
</pre>

Download, install and configure the Redmine framework.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /opt/
# http://rubyforge.org/frs/download.php/73457/redmine-1.0.4.tar.gz
# tar zxvf redmine-1.0.4.tar.gz
# mv redmine-1.0.4 redmine
# chmod -R a+rx /opt/redmine/public/
# cd /opt/redmine
# chmod -R 755 files log tmp
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /opt/redmine/config/
# cp database.yml.example database.yml
# vi /opt/redmine/config/database.yml
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >production:
  adapter: mysql
  database: redmine
  host: redmine
  username: redmine
  password: redmine
  encoding: utf8
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># mysql

mysql> create database  redmine default  character set  utf8;
mysql> grant all  on redmine.* to redmine@localhost identified by 'redmine';
mysql> flush privileges;
mysql> quit
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cd /opt/redmine
# RAILS_ENV=production rake config/initializers/session_store.rb
# RAILS_ENV=production rake db:migrate
</pre>

Configure Apache and add a Redmine config file

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/httpd/conf/httpd.conf 
</pre>

Uncomment  
NameVirtualHost *:80

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/httpd/conf.d/redmine.conf
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >&lt;VirtualHost *:80>
        DocumentRoot /opt/redmine
        ServerName redmine.how2centos.com
        &lt;Proxy balancer://redminecluster>
                 BalancerMember http://127.0.0.1:3000
                 BalancerMember http://127.0.0.1:3001
                 BalancerMember http://127.0.0.1:3002
        &lt;/Proxy>
        ProxyPass / balancer://redminecluster/
        ProxyPassReverse / balancer://redminecluster/
        ErrorLog /var/log/httpd/redmine_error.log
        CustomLog /var/log/httpd/redmine_access.log combined
&lt;/VirtualHost>
</pre>

Setup Redmine to email 

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cp /opt/redmine/config/email.yml.example /opt/redmine/config/email.yml
# vi /opt/redmine/config/email.yml
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >production:
  delivery_method: :smtp
  smtp_settings:
    address: smtp.how2centos.com
    port: 25
    domain: how2centos.com
#    authentication: :login
#    user_name: "redmine@example.net"
#    password: "redmine"

development:
  delivery_method: :smtp
  smtp_settings:
    address: 127.0.0.1
    port: 25
    domain: how2centos.com
#    authentication: :login
#    user_name: "redmine@example.net"
#    password: "redmine"
</pre>

Start thin and Redmine

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># service thin start
</pre>

Create a Subversion repository and start the SVN deamon

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># svnadmin create /opt/svn/repo
# svnserve -d
</pre>

Add a Subversion Apache configuration file

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># vi /etc/httpd/conf.d/svn.conf
</pre>

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >LoadModule dav_svn_module     modules/mod_dav_svn.so
LoadModule authz_svn_module   modules/mod_authz_svn.so

LDAPSharedCacheFile /root/LDAPSharedCacheFile
LDAPSharedCacheSize 200000
LDAPCacheEntries 1024
LDAPCacheTTL 600
LDAPOpCacheEntries 1024
LDAPOpCacheTTL 600

&lt;VirtualHost *:80>

        DocumentRoot /opt/svn
        ServerName svn.how2centos.com

        ErrorLog /var/log/httpd/svn_error.log
        LogLevel warn
        CustomLog /var/log/httpd/svn_access.log combined
        ServerSignature On

        &lt;Location "/">
                AuthBasicProvider ldap
                AuthType Basic
                AuthzLDAPAuthoritative off
                AuthName "How2CentOS SVN server"
                AuthLDAPURL "ldap://ldap.how2centos.com/CN=Users,DC=how2centos,DC=com?sAMAccountName"
                AuthLDAPBindDN "CN=ldap,CN=Users,DC=how2centos,DC=com"
                AuthLDAPBindPassword LDAPpassword

                require valid-user

        &lt;/Location>

        &lt;Location "/svn">
                DAV svn
                SVNParentPath           /opt/svn
                SVNListParentPath       On
                SVNReposName            "How2CentOS SVN Repo"
        &lt;/Location>

        &lt;Location /cache-info>
                SetHandler ldap-status
        &lt;/Location>

&lt;/VirtualHost>
</pre>

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># chown -R apache:apache /opt/svn/*
# chmod -R 770 /opt/svn/*
</pre>

Restart Apache for changes to take effect

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># Service httpd restart
</pre>

Browse to newly setup Redmine and Subversion setup

http://redmine.how2centos.com  
http://svn.how2centos.com