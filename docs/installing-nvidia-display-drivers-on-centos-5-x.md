---
id: 887
title: Installing NVIDIA display drivers on CentOS 5.5
date: 2010-06-01T12:36:49+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=887
permalink: /installing-nvidia-display-drivers-on-centos-5-x/
dsq_thread_id:
  - "103098482"
categories:
  - CentOS 5.5 Tutorials
tags:
  - CentOS
  - nvidia
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif" alt="" title="centos" width="42" height="40" class="alignleft size-full wp-image-225" />](http://www.how2centos.com/wp-content/uploads/2009/05/centos.gif)[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/06/nvidia.jpg" alt="" title="nvidia" width="53" height="40" class="alignleft size-full wp-image-918" />](http://www.how2centos.com/wp-content/uploads/2010/06/nvidia.jpg)After you have installed CentOS, you see that your Desktop is running quite a bit slower than you expected, everything is jerky and your pretty ticked off at this point in time. What you need is not a psychiatrist, but to install the drivers for your graphics card. This will solve the jerky issues you are having and save you some frustration. This post will show you how to install the NVIDIA drivers on CentOS.

CentOS does not have these drivers available in its default yum repository, so you will first need to add a repository to make the drivers available for installation.  
<!--more-->

  
Firstly you will need to update your CentOS distribution, to do this simply type the following into your console/terminal:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># su -c 'yum update'
</pre>

Enter your root password and follow the prompts that follow to complete the update.

Next we need to install the RPMForge repository so that we can download and install the NVIDIA display drivers. The repository comes in two flavors, namely 64-bit and 32-bit. Make sure you select the correct repository to install. When in doubt, select 32-bit.

For 32-bit installations (i.e. CentOS 5.x 32-bit installed):

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.1-1.el5.rf.i386.rpm
# su -c 'rpm -Uvh rpmforge-release-0.5.1-1.el5.rf.i386.rpm'
</pre>

Enter your root password to complete the installation.

For 64-bit installations (i.e. CentOS 5.x 64-bit installed):

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># wget http://packages.sw.be/rpmforge-release/rpmforge-release-0.5.1-1.el5.rf.x86_64.rpm
# su -c 'rpm -Uvh rpmforge-release-0.5.1-1.el5.rf.x86_64.rpm'
</pre>

Enter your root password to complete the installation.

Once the repository has been installed, we can finally install the NVIDIA display driver. To do this simply enter the following into your terminal/console:

For 32-bit installations (i.e. CentOS 5.x 32-bit installed):

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># su -c 'yum install dkms-nvidia-x11-drv.i386'
</pre>

Enter your root password and follow the prompts that follow.

For 64-bit installations (i.e. CentOS 5.x 64-bit installed):

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># su -c 'yum install dkms-nvidia-x11-drv.x86_64'
</pre>

Enter your root password and follow the prompts that follow.

Once the drivers have been installed, you can reboot your machine and experience CentOS without the jerky after effects. If you would like to customize your settings after you have rebooted, you can do so by using the NVIDIA settings applications. To open the application you can simply type the following into a console/terminal:

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"># su -c 'nvidia-settings'
</pre>

Enter your root password, a dialog will appear where you can customize your display settings. If you change anything, please remember to click the SAVE Configuration button.

If you get the following error &#8220;You do not appear to be using the NVIDIA X driver&#8230;..&#8221;, then you have not rebooted your machine, so do so now to enjoy the benefits of your NVIDIA display. If you are experiencing any problems, then please post a reply and I will help you sort it out.

Thanks for taking the time to read this post. Please share it with your friends.