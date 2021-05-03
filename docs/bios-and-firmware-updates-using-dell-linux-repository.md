---
id: 2243
title: BIOS and Firmware Updates Using Dell Linux Repository
date: 2011-10-28T15:08:51+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=2243
permalink: /bios-and-firmware-updates-using-dell-linux-repository/
dsq_thread_id:
  - "455369556"
image: /wp-content/uploads/2009/05/centos.gif
categories:
  - CentOS 5.6 Tutorials
  - CentOS 5.7 Tutorials
  - CentOS 6.0 Tutorials
---
You can update your CentOS 6 system to the latest version or to a specific version of the BIOS and firmware available in the Dell Linux online repository. You can inventory your CentOS 6 system, scan the repository for matching firmware with newer version using repository management software such as yum.

Firmware-tools are used to update BIOS and firmware on your system. Using a repository management software, you can easily update your BIOS and firmware to the latest or specific versions on your system.

### Setting Up/Bootstrapping the Repository

To setup/bootstrap the Dell Linux online repository on your CentOS 6 sever, run the following command at the command prompt:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget &#45;q &#45;O &#45; http://linux.dell.com/repo/hardware/latest/bootstrap.cgi | bash
</pre>

The system is configured to access the Dell Linux online repository using supported repository management software. The Dell GPG keys and libsmbios (BIOS library) are also installed.

### Installing Firmware Tools

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install dell_ft_install
</pre>

### Downloading Applicable Firmware

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># yum install $(bootstrap_firmware)
</pre>

You can also inventory your system for the list of existing versions of BIOS and firmware using the following command:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># inventory_firmware
</pre>

### Updating BIOS and Firmware Using CLI

Run the following command to inventory the system and scan the repository for new versions of components:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># update_firmware
</pre>

This command provides information about the existing versions of components on your system and the list of component versions that are available to be installed.

To install all applicable BIOS and firmware updates on your system, run the following command:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># update_firmware &#45;&#45;yes
</pre>

Once the devices are updated, the &#8220;Execution Success&#8221; message is displayed.

### Automatically Update Firmware

By default, installing a BIOS or firmware RPM does not apply the update to the hardware. The update is manually applied using the update_firmware command. However, you can automatically update the hardware during RPM installation by configuring the /etc/firmware/firmware.conf file.

To automatically install BIOS and firmware updates, ensure that rpm_mode is set to auto in the firmware.conf file as shown:

<pre class="theme:github font:droid-sans-mono lang:vim decode:true" >[main]

# Automatically install BIOS updates when an RPM BIOS Update file is installed
# values: 'auto', 'manual'
# default: 'manual'
rpm_mode=auto
</pre>

### Viewing Log Information

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># cat /var/log/firmware-updates.log
</pre>