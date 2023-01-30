---
title: Asterisk LibPRI outbound MOH fix
categories:
  - MISC
tags:
  - Asterisk
---
[:octicons-file-code-24: Source](https://github.com/georgerushby/how2centos.com/blob/main/docs/asterisk-libpri-outbound-moh-fix.md)

This is a very simple and effective 'work around' for the popular Asterisk outbound Music on Hold (MOH) issue. This is a default Asterisk setting that will play your own MOH on an outbound call, which has proved to be problematic. More often than not, and all the time with certain dialed numbers (depending on the dialed company's PBX system), the caller (you) is put on hold, and your Asterisk system plays your `[default]` MOH music. When the call is unholded by the remote party, the call on your side is not retrieved, resulting in your MOH being played indefinitely, loosing the call - very unprofessional, and you are guaranteed this will happen to your CEO, to his most frequent dialed business partners !

**Reason:** Within the Asterisk Kernel, it is coded to play the `[default]` Message on Hold (MOH) class, which is the same for all calls made internally to another extension. You can create many MOH classes, but only the `[default]` is used for external calls. It is also a bug in the way LibPRI processes ISDN on-hold frames, and won’t be fixed in current Asterisk 1.4, but will be an option to turn this off in DAHDI Asterisk 1.6 (so I have read). We have tried and tested 1.6, and was introduced to a whole new range of problems (other advice - stick to 1.4!). Asterisk also has issues interpreting certain DTMF tones, so does not know when to unhold a call, because never receives or interprets the unhold tone sent by remote PBX system. On the 'Outbound Routes' option in FreePBX, you can set the MOH to none, but does not work, still uses default.

**Relates to:**

* AsteriskNOW
* Trixbox
* Elastix - My best choice of a complete Asterisk PBX Solution!
* Other Asterisk installed by source, possibly on Ubuntu

All the above are available for ISO download, and based on CentOS 5 Core.

## Create a new Music on Hold class, and configure the music

Firstly browser to your FreePBX GUI, navigate to `Music on Hold`, select `Add Music Category`, and give it a name. For now I will name it CompanyA.

In your Linux console, this will create a folder in `/var/lib/asterisk/mohmp3/CompanyA/`

```
ll /var/lib/asterisk/mohmp3/
```

Copy your music from `/var/lib/asterisk/mohmp3/` to this new directory:

```
cp /var/lib/asterisk/mohmp3/*.mp3 /var/lib/asterisk/mohmp3/CompanyA
```

## Change ALL your Inbound Lines and Queues

Next Change ALL your Inbound Lines and Queues within FreePBX GUI to use this new MOH class 'CompanyA'. You dont have to do this for inbound Fax lines if you are making use of Fax to Email, like Hylafax. I left mine as Default.

You can script this, but its a much longer process to create a script, rather than simply doing it via the GUI. If you backup your asterisk folder, or rsync it, then change a field or two in FreePBX, then run rsync again or diff on the files, it changes more than one config file, and just becomes confusing. Get your IT desktop guy to do the manual work, otherwise please post the script!

## Edit config file and restart Asterisk

Finally all you need to do is edit a config file, reload Asterisk, and you're done!

```
vim /etc/asterisk/musiconhold_additional.conf
```

Comment out the `[default]` MOH class to point to /dev/null (this deactivates the MOH feature completely for this class). Comment out and add for easy roll-back, always think ahead!

```
[CompanyA]
mode=files
directory=/var/lib/asterisk/mohmp3/CompanyA/
random=yes

[default]
mode=files
directory=/dev/null

;[default]
;mode=files
;directory=/var/lib/asterisk/mohmp3/
```

FINALLY - in FreePBX, `Reload Asterisk` and you're done!

From here, when you make an outbound call, and you are put on hold, you will hear the dialed party's Music on Hold (or whatever they have) instead of yours. This reassures you that the call has not dropped, and you are still on hold. There is no DTMF tones or frames that your Asterisk box has to interpret, taking the fault away from your side.