---
title: Dell Switch Upgrade
description: How to upgrade Dell Switches
published: true
date: 2020-02-06T23:18:54.605Z
tags: 
---

# Dell Switch Upgrade

Firstly make sure to setup a TFTP server https://wiki.vm.tips/apps/tftp to distribute your packages from as well as a telnet/ssh client.

## download

You can download a copy of the newest dell switch firmware from: https://www.dell.com/support/

## enable

Firstly, login to your switch and get to an enabled prompt.

You can do this via:
```enable```

It should ask for a password.

## boot

Secondly, we need to update the boot rom. Boot roms always end in ```.rfb```.

To do this we run:
```
copy tftp://10.239.245.118/dell/5324.rfb boot
```


## image

Thirdly, we need to update the actual image. To do this step is a bit more complex. 

We first need to verify which "slot" is active, and upload to the "not active" slot.

```
console# show bootvar
Images currently available on the FLASH
image-1 active (selected for next boot)
image-2 not active
console#
```

Upload the firmware:
```
console# copy tftp://10.239.245.118/dell/5324.ros image
```

## reload

Now to fully install the dell switch run. Where X is the non active boot image.

```
console# boot system image-x
console# reload
```

