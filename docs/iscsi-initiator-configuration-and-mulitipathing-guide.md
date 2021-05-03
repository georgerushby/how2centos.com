---
id: 175
title: ISCSI Initiator Configuration and Mulitipathing Guide
date: 2009-05-12T18:37:17+02:00
author: George
layout: post
guid: http://www.how2centos.com/?p=175
permalink: /iscsi-initiator-configuration-and-mulitipathing-guide/
wiki_page:
  - "0"
wiki_page_toc:
  - "0"
dsq_thread_id:
  - "44472055"
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
categories:
  - CentOS 5.2 Tutorials
  - CentOS 5.3 Tutorials
  - CentOS 5.4 Tutorials
  - CentOS 5.5 Tutorials
tags:
  - centos 5.3
  - iscsi
  - multipathing
  - red hat 5.3
---
**Installation Instructions:**

The initiator is actually a kernel module that is already available with the appropriate CentOS Linux installation. To use and manage the initiator, you need to install the iSCSI utilities.

<!--more-->

**Notes:**

*Do not use the version that comes on the CentOS v5.0 install CDs. That version, iscsi-initiator-utils-6.2.0.742-0.5.el5 does not work with the EqualLogicarray. You can find targets but not connect to them. You need version iscsi-initiator-utils-6.2.0.742-0.6.el5 or greater.

*CentOS 5.0 requires at least one iSCSI HBA and one GbE NIC to do multipathing.  
*You will need CentOS 5.1 or greater to take advantage of multipathing with GbE NICs. This requires version iscsi-initiator-utils-6.2.0.865-0.8.el5 or greater.

<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true"><pre>
# yum install iscsi-initiator-utils
</pre>


<p>
  Once installed, run:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# service iscsi start
</pre>


<p>
  To verify that the iSCSI service will be started at boot time, the chkconfig command can be used as follows:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# chkconfig --list iscsi 
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
iscsi 0:off 1:off 2:off 3:off 4:off 5:off 6:off
</pre>


<p>
  By default, the newly added iscsi initiator is not enabled at boot, which is the reason for each of the run levels listed to have the service set to off. To enable this at boot, again use the chkconfig command as follows:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# chkconfig --add iscsi
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# chkconfig iscsi on
</pre>


<p>
  The above two commands first checks to be sure there are the necessary scripts to start and stop the service, and then it sets this service to be enabled for the appropriate runlevels. Then check to be sure the changes took effect:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# chkconfig --list iscsi 
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
iscsi 0:off 1:off 2:on 3:on 4:on 5:on 6:off
</pre>


<p>
  You also need to do the same for the Multipath daemon.
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# chkconfig --list multipathd 
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
multipathd 0:off 1:off 2:on 3:on 4:on 5:on 6:off
</pre>


<p>
  <strong>Discovering Targets:</strong>
</p>


<p>
  Once you have the iscsi service running you will use the &#8216;iscsiadm&#8217; utility to discover, login and logout of targets. To get a list of available targets type:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# iscsiadm -m discovery -t st -p :3260
</pre>


<p>
  <em>Example:</em>
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# iscsiadm -m discovery -t st -p 172.23.10.240:3260
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
172.23.10.240:3260,1 iqn.2001-05.com.equallogic:0-8a0906-83bcb3401-16e0002fd0a46f3d-centos-test
</pre>


<p>
  The example shows that the &#8216;centos-test&#8217; volume has been found but not yet logged in.
</p>


<p>
  <strong>Logging into Targets</strong>
</p>


<p>
  Here are two ways to connect to iSCSI targets : ( #man iscsiadm for other methods)
</p>


<p>
  Log into all targets.
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# iscsiadm -m node -l
</pre>


<p>
  Log into an individual target.
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# iscsiadm -m node -l -T  -l -p :3260
</pre>


<p>
  <em>Example:</em>
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# iscsiadm -m node -l -T iqn.2001-05.com.equallogic: 83bcb3401-16e0002fd0a46f3d-centos-test -p :3260
</pre>


<p>
  <strong>Checking Session Status:</strong>
</p>


<p>
  To see the connection status run:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# iscsiadm -m session -i
</pre>


<p>
  <em>Sample output:</em>
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
iscsiadm version 2.0-742
************************************
Session (sid 1) using module tcp:
************************************
TargetName: iqn.2001-05.com.equallogic:0-8a0906-83bcb3401-16e0002fd0a46f3d-centos-test
Portal Group Tag: 1
Network Portal: 172.23.10.240:3260
iSCSI Connection State: LOGGED IN
Internal iscsid Session State: NO CHANGE
************************
Negotiated iSCSI params:
************************
HeaderDigest: None
DataDigest: None
MaxRecvDataSegmentLength: 65536
MaxXmitDataSegmentLength: 65536
FirstBurstLength: 65536
MaxBurstLength: 262144
ImmediateData: Yes
InitialR2T: No
axOutstandingR2T: 1
************************
Attached SCSI devices:
************************
Host Number: 2 State: running
scsi2 Channel 00 Id 0 Lun: 0
Attached scsi disk sdb State: running
</pre>


<p>
  <strong>Logging off a target:</strong>
</p>


<p>
  It&#8217;s the same as logging into a target, except, that you use -u instead of -l
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# iscsiadm -m node -u -T iqn.2001-05.com.equallogic:0-8a0906-83bcb3401-16e0002fd0a46f3d-centos-test -p :3260
</pre>


<p>
  <strong>Persistent Device naming:</strong>
</p>


<p>
  Devices using the Red Hat software initiators do not have a persistent naming scheme, and do not guarantee that a device (i.e. /dev/sdc) will always have the same device node. Persistent Naming describes mechanisms where the system identifies devices without relying on the /dev node, and provides a reference point for it that does not change at reboot.
</p>


<p>
  First you have to comment out the &#8216;Blacklist all devices&#8217; section in /etc/multipath.conf file
</p>


<p>
  <em>Note:</em>
</p>


<p>
  If the example file, multipath.conf is not in /etc, copy it from /usr/share/doc/device-mapper- multipath-0.4.7/multipath.conf.synthetic and edit it as below
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# cp /usr/share/doc/device-mapper-multipath-0.4.7/multipath.conf.synthetic /etc/multipath.conf
</pre>


<pre class="brush: bash">
# Blacklist all devices by default. Remove this to enable multipathing
# on the default devices.
blacklist {
	devnode "*"
}
</pre>


<p>
  So it should look like this:
</p>


<pre class="brush: bash">
# Blacklist all devices by default. Remove this to enable multipathing
# on the default devices.
# blacklist {
#	devnode "*"
#}
</pre>


<p>
  Then restart the multipathd daemon
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# service multipathd restart
</pre>


<p>
  Now check that dev-mapper has configured the volume.
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# multipath -v2
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# multipath -ll
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
mpath0 (<span style="color: #ff0000;">36090a01840b3bc833d6fa4d02f00e016</span>) dm-2 EQLOGIC,100E-00
[size=8.0G][features=0][hwhandler=0]
\_ round-robin 0 [prio=1][active]
\_ 2:0:0:0 sdb 8:16 [active][ready]
</pre>


<p>
  The highlighted number is the UUID of the volume. That never changes. You can use that UUID to create a persistent, friendlier name. For example you can name it the same as you called the volume on the EQL array.
</p>


<p>
  Again edit the /etc/multipath.conf file.
</p>


<p>
  Uncomment the following section and change the defaults to match your UUID and set a friendly alias name.
</p>


<p>
  Edit the /etc/multipath.conf file and uncomment out the following:
</p>


<pre class="brush: bash">
#multipaths {
# 	multipath {
# 		wwid 3600508b4000156d700012000000b0000
# 		alias yellow
# 		path_grouping_policy multibus
# 		path_checker readsector0
# 		path_selector "round-robin 0"
# 		failback manual
# 		rr_weight priorities
# 		no_path_retry 5
# 		rr_min_io 100
# 	}
# 	multipath {
# 		wwid 1DEC_____321816758474
# 		alias red
# 	}
#}
</pre>


<p>
  Change the number after wwid to the UUID for your volume. Change the alias to something more friendly or use the volume name from the array. Change the rr_min_io to 10.
</p>


<p>
  Here&#8217;s an example showing how to do more than one volume.
</p>


<pre class="brush: bash">
multipaths {
	multipath {
		wwid 36090a02830f251891f74744263735281
		alias centos-test
		path_grouping_policy multibus
		path_checker readsector0
		path_selector "round-robin 0"
		failback manual
		rr_weight priorities
		no_path_retry 5 r
		rr_min_io 10
	}
#	multipath {
#		wwid 36090a01840b31c74e173a4873200a02f
#		alias svr-vol
# 	}
}
</pre>


<p>
  Save the file, then run:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# multipath -v2
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# multipath -ll
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
centos-test (36090a02830f251891f74744263735281) dm-1 EQLOGIC,100E-00
[size=100G][features=1 queue_if_no_path][hwhandler=0]
\_ round-robin 0 [prio=0][active]
 \_ 9:0:0:0 sdc 8:48 [active][ready]
 svr-vol (36090a01840b31c74e173a4873200a02f) dm-0 EQLOGIC,100E-00
[size=10G][features=0][hwhandler=0]
\_ round-robin 0 [prio=0][enabled]
 \_ 6:0:0:0 sdb 8:16 [active][ready]
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# ls -l /dev/mapper
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
total 0
crw-rw---- 1 root root 10, 63 2007-11-16 17:15 control
brw-rw---- 1 root disk 254, 1 2007-11-19 15:59 centos-test
brw-rw---- 1 root disk 254, 0 2007-11-19 15:58 svr-vol
</pre>


<p>
  You should now have a persistent name to access that volume.
</p>


<p>
  Now create a EXT3 filesystem on that device.
</p>


<p>
  <em>Example:</em>
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# mke2fs -j -v /dev/mapper/centos-test
</pre>


<p>
  <strong>Mounting iSCSI Filesystems at Boot:</strong>
</p>


<p>
  In order to mount a filesystem that exists on an iSCSI Volume connected through the Linux iSCSI Software initiator, you need to add a line to the /etc/fstab file. The format of this line is the same as any other device and filesystem with the exception being that you need to specify the _netdev mount option, and you want to have the last two numbers set to 0 (first is a dump parameter and the second is the fsck pass).
</p>


<p>
  The _netdev option delays the mounting of the filesystem on the device listed until after the network has been started and also ensures that the filesystem is unmounted before stopping the network subsystem at shutdown.
</p>


<p>
  An example of an /etc/fstab line for a filesystem to be mounted at boot that exists on an iSCSI Volume is as follows:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
/dev/mapper/centos-test /mnt/centos-test ext3 _netdev,defaults 0 0
</pre>


<p>
  <strong>Configuring Mulitpath Connections:</strong>
</p>


<p>
  To create the multiple logins needed for Linux dev-mapper to work you need to create an &#8216;interface&#8217; file for each GbE interface you wish to use to connect to the array.
</p>


<p>
  You will have to create the &#8216;iface.ethX&#8217; file in /var/lib/iscsi/ifaces.
</p>


<p>
  You then edit the file and add in the MAC address of the ETH0 adapter.
</p>


<p>
  You can get the MAC address with the following command:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
#ifconfig eth0 | grep HW 
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
eth0 Link encap:Ethernet HWaddr 00:50:56:97:70:70
</pre>


<p>
  The MAC address in this case is: 00:50:56:97:70:70
</p>


<p>
  Here&#8217;s an example of what the /var/lib/iscsi/ifaces/iface.eth0 looks like:
</p>


<p>
  To bind by hardware address set the NIC&#8217;s MAC address to iface.hwaddress
</p>


<p>
  <em>Example:</em>
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
iface.hwaddress = 00:50:56:97:70:70
</pre>


<p>
  Repeat for the other ports you wish to use. E.g. if you are going to use ETH1, then copy the /var/lib/iscsi/ifaces/iface.eth0 file to /var/lib/iscsi/ifaces/iface.eth1.
</p>


<p>
  Then edit the MAC address to reflect the MAC address of the ETH1 adapter.
</p>


<p>
  If you have already discovered your volumes, you now need to re-discover the target(s).
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
#iscsiadm -m discovery -t st -p :3260
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
172.23.10.90:3260,1 iqn.2001-05.com.equallogic: 0-8a0906-83bcb3401-16e0002fd0a46f3d-centos-test
172.23.10.90:3260,1 iqn.2001-05.com.equallogic: 0-8a0906-83bcb3401-16e0002fd0a46f3d-centos-test</pre>


<p>
  You should see an entry for each interface you specified.
</p>


<p>
  You&#8217;ll need to log into the volume.
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
#iscsiadm -m node -l -T iqn.2001-05.com.equallogic:0-8a0906-8951f2302-815273634274741f-centos-test -p :3260
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
#iscsiadm -m session
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
tcp: [3] 172.23.10.90:3260,1 iqn.2001-05.com.equallogic: 0-8a0906-83bcb3401-16e0002fd0a46f3d-centos-test
tcp: [4] 172.23.10.90:3260,1 iqn.2001-05.com.equallogic: 0-8a0906-83bcb3401-16e0002fd0a46f3d-centos-test</pre>


<p>
  This shows that both adapters have connected to the array.
</p>


<p>
  Verify that the multipathing is correctly configured.
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
#multipath -v2
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
#multipath -ll 
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
dw-test-vol (36090a02830f251891f74744263735281) dm-1 EQLOGIC,100E-00 [size=100G][features=1 queue_if_no_path][hwhandler=0]
\_ round-robin 0 [prio=0][active]
 \_ 9:0:0:0 sdd 8:48 [active][ready]
 \_ 8:0:0:0 sde 8:64 [active][ready]
dw-svr-vol (36090a01840b31c74e173a4873200a02f) dm-0 EQLOGIC,100E-00
[size=10G][features=0][hwhandler=0]
\_ round-robin 0 [prio=0][enabled]
 \_ 6:0:0:0 sdb 8:16 [active][ready]
 \_ 7:0:0:0 sdc 8:32 [active][ready]
</pre>


<p>
  In this example you see that there are two paths to each volume.
</p>


<p>
  <strong>Linux NIC Configuration:</strong>
</p>


<p>
  <em>Flowcontrol</em>
</p>


<p>
  Be sure that the network interfaces are configured to use Flow Control and Jumbo Frames if supported. To do this, use the ethtool utility on Red Hat.
</p>


<p>
  To check for Flow Control (RX/TX Pause) on your interface, use the -a option as follows:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# ethtool -a
</pre>


<p>
  <em>Example:</em>
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# ethtool -a eth0
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
Pause parameters for eth0:
Autonegotiate: on
RX: off
TX: off
</pre>


<p>
  Set Flow Control to on using the -A parameter with ethtool as follows:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# ethtool -A  [rx|tx] on
</pre>


<p>
  <em>Example:</em>
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# ethtool -A eth0 rx on tx on
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# ethtool -a eth0
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
Pause parameters for eth0:
Autonegotiate: on
RX: on
TX: on
</pre>


<p>
  For some NICs, using ethtool will not persistently set this variable. For these NICs, see the manufacturer of the NIC for steps to configure the Flow Control setting for the NIC, or simply add the ethtool command to the end of the /etc/rc.d/rc.local file to set Flow Control for rx and tx to on. The majority of GbE NICs will detect and enable flowcontrol properly
</p>


<p>
  For Jumbo Frames, you can use the ifconfig utility to make the change on a running interface. Unfortunately, this change will revert back to the default on a system reboot.
</p>


<p>
  First, show the current setting for the interface in question using the ifconfig command using the interface name as an argument:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# ifconfig eth0
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
eth0 	Link encap:Ethernet HWaddr 00:0E:0C:70:C9:63
	inet addr:172.19.51.160 Bcast:172.19.255.255 Mask:255.255.0.0
	UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1
	RX packets:236410 errors:0 dropped:0 overruns:0 frame:0
	TX packets:272 errors:0 dropped:0 overruns:0 carrier:0
	collisions:0 txqueuelen:1000
	RX bytes:40458683 (38.5 Mb)
	TX bytes:55551 (54.2 Kb)
	Base address:0xdca0 Memory:dfca0000-dfcc0000
</pre>


<p>
  The setting we are interested in is the MTU value. As we can see from the above output, this is currently set to 1500 which is not a Jumbo packet size. To change this, we need to set the mtu again using the ifconfig command as follows:
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# ifconfig eth0 mtu 9000
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# ifconfig eth0
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
eth0 	Link encap:Ethernet HWaddr 00:0E:0C:70:C9:63
	inet addr:172.19.51.160 Bcast:172.19.255.255 Mask:255.255.0.0
	UP BROADCAST RUNNING MULTICAST MTU:9000 Metric:1
	RX packets:3007 errors:0 dropped:0 overruns:0 frame:0
	TX packets:813 errors:0 dropped:0 overruns:0 carrier:0
	collisions:0 txqueuelen:1000
	RX bytes:415532 (405.7 Kb)
	TX bytes:123065 (120.1 Kb)
	Base address:0xdca0 Memory:dfca0000-dfcc0000
</pre>


<p>
  To make this setting persistent, you need to add the MTU=&#8221;&#8221; parameter to the end of the ifcfg startup scripts for your SAN interfaces. These are found in the /etc/sysconfig/network-scripts directory. The naming format of the cfg files for your interfaces is ifcfg-.
</p>


<p>
  A sample output of one of these files after adding the MTU line to the file should be similar to the following
</p>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
# cat /etc/sysconfig/network-scripts/ifcfg-eth0
</pre>


<pre class="toolbar:2 nums:false nums-toggle:false theme:github font:droid-sans-mono whitespace-before:1 whitespace-after:1 lang:default decode:true">
DEVICE=eth0
BOOTPROTO=none
BROADCAST=172.19.255.255
IPADDR=172.19.51.160
NETMASK=255.255.0.0
NETWORK=172.19.0.0
ONBOOT=yes
TYPE=Ethernet
USERCTL=no
PEERDNS=yes
GATEWAY=172.19.0.1
MTU="9000"
</pre>


<p>
  Once the above Network changes have been made, you should reboot the host to verify that they have been made correctly and that the settings are persistent.
</p>