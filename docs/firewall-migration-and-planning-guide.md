---
id: 689
title: Firewall Migration and Planning Guide
date: 2010-01-31T13:56:29+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=689
permalink: /firewall-migration-and-planning-guide/
dsq_thread_id:
  - "62953653"
categories:
  - MISC
tags:
  - Firewall
  - Planning Guide
---
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/01/firewall.jpeg" alt="firewall" title="firewall" width="60" height="60" class="alignleft size-full wp-image-696" />](http://www.how2centos.com/wp-content/uploads/2010/01/firewall.jpeg)**Plan and document existing firewall**  
Planning and documenting the migration of the firewall is critical, without it you&#8217;re doomed. Over time configurations grow as rules and networks are added for specific purposes are often superseded or simply forgotten.

You need to document these as well as any other additional services or features (i.e. DHCP, DNS or VPN). This will assist you while you recreate the policies behind the firewall rules.  
[<img loading="lazy" src="http://www.how2centos.com/wp-content/uploads/2010/01/Firewall-Planning-Guide-300x187.png" alt="" title="Firewall Planning Guide" width="300" height="187" class="aligncenter size-medium wp-image-702" srcset="https://www.how2centos.com/wp-content/uploads/2010/01/Firewall-Planning-Guide-300x187.png 300w, https://www.how2centos.com/wp-content/uploads/2010/01/Firewall-Planning-Guide.png 553w" sizes="(max-width: 300px) 100vw, 300px" />](http://www.how2centos.com/wp-content/uploads/2010/01/Firewall-Planning-Guide.png)  
You can download a sample Firewall Planning Guide here:

[download id=&#8221;4&#8243;]  
<!--more-->

  
**Review policy changes**  
Before you begin review your Planning guides and decide if they&#8217;re 1) Still valid? or 2) Is this what the customer needs?

The replacement of a key firewall is the perfect time to consider whether the current policy is the current best practice and/or whether it still fits the security needs of the company.

**Determine any rule changes**  
Once you’re agreed on policy, check the existing firewall rules and functions to see what is in conformance, what is superfluous, and what needs to be added.

**Test the new configuration on the firewall**  
This is an important step. Change your gateway to the new firewall which is running concurrently with the old and test. Firstly, this will let you determine whether or not the new rules are complete and working. Secondly, it won&#8217;t interrupt day to day operations.

**Move to the new firewall**  
If all is well migrate to the new firewall either by switching IP&#8217;s or changing the default gateway in DHCP. Once the firewall is in place, acceptance testing will of course need to be carried out.

In case of emergency, the fact that you have the old firewall and new firewall set up allows you to quickly roll back to the old firewall and pull the new one out of service for checking and reconfiguration. Having this option available should reduce network downtime if there&#8217;s a show stopper.

**Adding new services**  
Wait until the new firewall is stable before adding new functions as this simplifies testing. Finally complete the migration by adding any new services that were requested, and since the configuration is known to be good and stable before adding the new services, any introduced instability should be easy to isolate and fix.