---
id: 233
title: 'Fedora 11: Leonidas – Released'
date: 2009-06-10T07:01:13+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=233
permalink: /fedora-11-reign-released/
dsq_thread_id:
  - "44453326"
categories:
  - Fedora
tags:
  - fedora 11
---
**_Daxos:_** I see I was wrong to expect Sparta&#8217;s Linux commitment to at least match our own.  
**_King Leonidas:_** Doesn&#8217;t it?  
[points to Arcadian linux user behind Daxos]  
**_King Leonidas:_** You there, what is your Distro?  
**_Free Greek-Slackware user:_** I am a Slackware user&#8230; sir.  
**_King Leonidas:_** [points to another linux user] And you, Arcadian, what is your Distro?  
**_Free Greek-Debian user:_** Debian, sir.  
**_King Leonidas:_** Debian.  
[turns to a third linux user]  
**_King Leonidas:_** You?  
_**Free Greek-Ubuntu user**:_ Ubuntu.  
**_King Leonidas:_** [turns back shouting] Spartans! What is your Distro?  
**_Spartans:_** FE-DOOR-RA! FE-DOOR-RA! FE-DOOR-RA!  
**_King Leonidas:_** You see, old friend? I brought a better Distro than you did.

<img loading="lazy" class="size-full wp-image-235 alignleft" title="fedora" src="http://www.how2centos.com/wp-content/uploads/2009/06/fedora.gif" alt="fedora" width="40" height="40" /> Fedora is a Linux-based operating system that showcases the latest in free and open source software. Fedora is always free for anyone to use, modify, and distribute. It is built by people across the globe who work together as a community.

Their latest release Fedora 11: Leonidas is available for download

[Download Fedora 11: Leonidas](http://fedoraproject.org/en/get-fedora)

<!--more-->

**The following are major features for Fedora 11:**

**Automatic font and mime-type installation** &#8211; PackageKit was introduced in Fedora 9 as a cross-distro software management application for users. The capabilities it offers thanks to integration with the desktop became more visible in Fedora 10, where it provided automatic codec installation. Now in Fedora 11, PackageKit extends this functionality with the ability to automatically install fonts where needed for viewing and editing documents. It also includes the capability to install handlers for specific content types as needed. Some work is still being completed to provide automatic installation of applications.

**Volume Control** &#8211; Currently, people using Fedora have to go through many levels of mixers in different applications to properly set up sound sources. These are all exposed in the volume control on the desktop, making for a very confusing user experience. PulseAudio allows us to unify the volume controls in one interface that makes setting up sound easier and more pain-free.

**Intel, ATI and Nvidia kernel modsetting** &#8211; Fedora 10 provided the first steps by a major distribution into using the kernel modesetting (KMS) feature to speed up graphical boot. We indicated at the time that we would be adding greater support for additional video cards as time went on. KMS originally was featured only on some ATI cards. In Fedora 11, this work is extended to include many more video cards, including Intel and Nvidia, and additional ATI as well. Although not fully complete, we have increased enormously the video card coverage of the KMS feature, with more to come.

**Fingerprint** &#8211; Extensive work has been done to make fingerprint readers easy to use as an authentication mechanism. Currently, using fingerprint readers is a bit of a pain, and installing/using fprint and its pam module take more time than should ever be necessary. The goal of this feature is to make it painless by providing all the required pieces in Fedora, together with nicely integrated configuration. To enable this functionality the user will register their fingerprints on the system as part of user account creation. After doing so, they will easily be able to log in and authenticate seamlessly using a simple finger swipe. This greatly simplifies one element of identity management and is a great step in the evolution of the linux desktop.

**IBus input method system** &#8211; ibus has been rewritten in C and is the new default input method for Asian languages. It allows input methods to be added and removed dynamically during a desktop session. It supports Chinese (pinyin, libchewing, tables), Indic (m17n), Japanese (anthy), Korean (libhangul), and more. There are still some features missing compared to scim so testing is strongly encouraged and reports of problems and suggestions for improvements welcome.

**Presto** &#8211; Normally when you update a package in Fedora, you download an entire replacement package. Most of the time (especially for the larger packages), most of the actual data in the updated package is the same as the original package, but you still end up downloading the full package. Presto allows you to download the difference (called the delta) between the package you have installed and the one you want to update to. This can reduce the download size of updates by 60% â€“ 80%. It is not enabled by default for this release. To make use of this feature you must install the yum-presto plugin with yum install yum-presto.

**Some other features in this release include:**

**Ext4 filesystem** &#8211; The ext3 file system has remained the mature standard in Linux for a long time. The ext4 file system is a major update that has an improved design, even better performance and reliability, support for much larger storage, and very fast file system checks and file deletions. It is now the default filesystem for new installations.

**Virt Improved Console** &#8211; In Fedora 10 and earlier the virtual guest console is limited to a screen resolution of 800&#215;600. In Fedora 11 the goal is to have the screen default to at least 1024&#215;768 resolution out of the box. New installations of F11 provide the ability to use other interface devices in the virtual guest, such as a USB tablet, which the guest will automatically detect and configure. Among the results is a mouse pointer that tracks the local client pointer one-for-one, and providing expanded capabilities.

**MinGW (Windows cross compiler)** &#8211; Fedora 11 provides MinGW, a development environment for Fedora users who wish to cross-compile their programs to run on Windows without having to use Windows. In the past developers have had to port and compile all of the libraries and tools they have needed, and this huge effort has happened independently many times over. MinGW eliminates duplication of work for application developers by providing a range of libraries and development tools already ported to the cross-compiler environment. Developers don&#8217;t have to recompile the application stack themselves, but can concentrate just on the changes needed to their own application.

Read more on the [Fedora Project Website](http://docs.fedoraproject.org/release-notes/f11/en-US/index.html#sect-Release_Notes-Fedora_11_Overview)