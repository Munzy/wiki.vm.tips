<!-- TITLE: Brocade Switch Upgrade -->
<!-- SUBTITLE: A quick summary of Upgrade -->

# Brocade Upgrade



## Current Version 

To get the version of the current running switch:

```
show flash
```

The output will look like:

```
SSH@ICX6610-24 Switch#show flash
Stack unit 1:
  Compressed Pri Code size = 7762230, Version:08.0.30nT7f1 (FCXS08030n.bin)
  Compressed Sec Code size = 7762230, Version:08.0.30nT7f1 (FCXS08030n.bin)
  Compressed Boot-Monitor Image size = 370695, Version:10.1.00T7f5
  Code Flash Free Space = 48889856
Stack unit 2:
  Compressed Pri Code size = 7762230, Version:08.0.30nT7f1 (FCXS08030n.bin)
  Compressed Sec Code size = 7762230, Version:08.0.30nT7f1 (FCXS08030n.bin)
  Compressed Boot-Monitor Image size = 370695, Version:10.1.00T7f5
  Code Flash Free Space = 48627712
SSH@ICX6610-24 Switch#
```

We also can get details from:

```
show version
```

The output will look like.

```
SSH@ICX6610-24 Switch#show version
  Copyright (c) 1996-2016 Brocade Communications Systems, Inc. All rights reserv                                                                                                                                                             ed.
    UNIT 1: compiled on May 06 2017 at 08:15:28 labeled as FCXS08030n
                (7762230 bytes) from Secondary FCXS08030n.bin
        SW: Version 08.0.30nT7f1
    UNIT 2: compiled on May 06 2017 at 08:15:28 labeled as FCXS08030n
                (7762230 bytes) from Secondary FCXS08030n.bin
        SW: Version 08.0.30nT7f1
  Boot-Monitor Image size = 370695, Version:10.1.00T7f5 (grz10100)
  HW: Stackable ICX6610-24
==========================================================================
UNIT 1: SL 1: ICX6610-24 24-port Management Module
         Serial  #: BXP2530J2P6
         License: BASE_SOFT_PACKAGE   (LID: dzrHKIFlHrL)
         P-ENGINE  0: type E02B, rev 01
==========================================================================
UNIT 1: SL 2: ICX6610-QSFP 10-port 160G Module
==========================================================================
UNIT 1: SL 3: ICX6610-8-port Dual Mode(SFP/SFP+) Module
==========================================================================
UNIT 2: SL 1: ICX6610-24 24-port Management Module
         Serial  #: BXP2530J2P1
         License: BASE_SOFT_PACKAGE   (LID: dzrHKIFlHrG)
         P-ENGINE  0: type E02B, rev 01
==========================================================================
UNIT 2: SL 2: ICX6610-QSFP 10-port 160G Module
==========================================================================
UNIT 2: SL 3: ICX6610-8-port Dual Mode(SFP/SFP+) Module
==========================================================================
  800 MHz Power PC processor 8544E (version 0021/0023) 400 MHz bus
65536 KB flash memory
  512 MB DRAM
STACKID 1  system uptime is 1 hour(s) 35 minute(s) 58 second(s)
STACKID 2  system uptime is 1 hour(s) 35 minute(s) 59 second(s)
The system started at 20:52:51 GMT+00 Thu Aug 29 2019

 The system : started=warm start         reloaded=by "reload"
My stack unit ID = 1, bootup role = active

SSH@ICX6610-24 Switch#

```

## Upgrade

To upgrade you need a running tftp server with the most current flash files.

You can access the downloads from here: https://support.ruckuswireless.com/software

### Upgrade Boot Code

This will upgrade the bootrom version. Make sure you do this step first.

```
copy tftp flash 10.239.2xx.xxx Boot/grz10123.bin bootrom
```

### Upgrade Firmware

This will download the newest firmware into the secondary slot. Usually, brocade is shipped with two versions. The red letter differs the image for either [R]outer or [S]witch.

FCX<color #ed1c24>S</color>08030n.bin
FCX<color #ed1c24>R</color>08030n.bin

In this case we want the [S]witch image.

```
copy tftp flash 10.239.2xx.xxx Firmware/FCXS08030n.bin sec
```

### Reload into new Flash

Now that we have upgraded the image and boot code. We need to reboot into the new image. To do this we boot into the secondary.

```
boot system flash sec
```

If it fails it will go back to primary. 

### Copy primary to secondary

After we have successfully verified that we have booted into the correct image. We need to copy the secondary to the primary this is done by:

```
copy flash flash primary
```


### Video Tutorial

https://www.youtube.com/watch?v=7SpZq51v8GE
