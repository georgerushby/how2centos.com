---
id: 18
title: SVN Apache LDAP configuration
date: 2009-03-09T18:53:31+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=18
permalink: /svn-apache-ldap-configuration/
dsq_thread_id:
  - "44624383"
categories:
  - CentOS
tags:
  - Apache
  - Subversion
---
## Apache Configuration

Create a Apache configuration file for Subversion

```
vi /etc/httpd/conf.d/subversion.conf
```

```
LoadModule dav_svn_module     modules/mod_dav_svn.so
LoadModule authz_svn_module   modules/mod_authz_svn.so

<Location /repos>
  
  # Enable Subversion
    DAV svn

  # Directory containing all repository for this path
    SVNParentPath			/opt/svn/
    SVNAutoversioning			on

  # LDAP Authentication & Authorization is final; do not check other databases
    AuthBasicProvider			ldap
    AuthzLDAPAuthoritative		off

  # Do basic password authentication in the clear
    AuthType				Basic

  # The name of the protected area or "realm"
    AuthName				"Subversion Repository"

  # The LDAP query URL
    AuthLDAPURL				"ldap://<LDAPServer>:389/dc=example,dc=co.za

  # Require authentication for this Location
    Require valid-user

</Location>
```
