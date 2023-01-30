---
title: CentOS 5 Puppet Install
tags:
  - centos 5.2
  - puppet
---
Puppet is a system for automating system administration tasks. Its automation saves you countless hours of frustration, monotony and reinventing the wheel. It lets you perform administrative task from a central systems to any number of systems running any variant of operating system.

For a more complete description visit [Puppet Labs.](http://reductivelabs.com/trac/puppet/wiki/AboutPuppet)

## Installing the Puppet CentOS 5 packages

### Install the Puppet Repository

```bash
rpm -ivh http://yum.puppetlabs.com/el/6/products/i386/puppetlabs-release-6-7.noarch.rpm
```

### Install the Puppet Master packages

```bash
yum install puppet-server
```

### Install the Puppet Client packages

```bash
yum install puppet
```

## A Simple Manifest: Managing Ownership of a File

### Step 1: Create a minimal manifest file called site.pp in /etc/puppet/manifests with the following content

```bash
vi /etc/puppet/manifests/site.pp
```

```bash
/etc/puppet/manifests/site.pp

import "classes/*"

node default {
    include sudo
 }
```

### Step 2: Next create the sudo.pp class in `/etc/puppet/manifests/classes/` with the following content

```bash
vi /etc/puppet/manifests/classes/sudo.pp
```

```kbd
class sudo {
    file { "/etc/sudoers":
        owner => "root",
        group => "root",
        mode  => 440,
    }
}
```

This class which will ensure that the owner, group, and mode of the `/etc/sudoers` file will be set consistently across all systems that belong to that class.

### Step 3: Start the Puppet Master service and enable startup on boot

```bash
service puppetmaster start
```

```bash
chkconfig puppetmaster on
```

### Configuring Puppet

Configure the puppet client to connect to the server and enable logging. Edit the file `/etc/sysconfig/puppet` and uncomment the `PUPPET\_LOG` and `PUPPET\_SERVER` line specifying the servers address.

```bash
vi /etc/sysconfig/puppet
```

```kdb
# The puppetmaster server
PUPPET_SERVER=PuppetMaster

# If you wish to specify the port to connect to do so here
#PUPPET_PORT=8140

# Where to log to. Specify syslog to send log messages to the system log.
PUPPET_LOG=/var/log/puppet/puppet.log

# You may specify other parameters to the puppet client here
#PUPPET_EXTRA_OPTS=--waitforcert=500
```

The client will automatically pull configuration from the server every 30 minutes, start it as a service and enable startup on boot

```bash
service puppet start
```

```bash
chkconfig puppet on
```

### Sign the SSL key request from the Puppet Client

In order for the two systems to communicate securely we need to create signed SSL certificates. You should be logged into both the _Puppet Master_ and _Puppet_ machines for this next step.

```bash
puppetca --list
```

```kbd
server2.example.co.za
```

```bash
puppetca --sign server2.example.co.za
```

```kbd
Signed server2.example.co.za
```
