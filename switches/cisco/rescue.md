---
title: Cisco Switch / Router Rescue
description: How to rescue a broken cisco router
published: true
date: 2020-02-07T00:43:53.925Z
tags: cisco, router, switch, rescue
---

# Rescue

In certain cases a switch may brick itself and fail to boot. This is a guide on how to rescue these systems.

## Telnet + XMODEM

This process will transfer the fimrware over serial and telnet to the device that is not working.

### Requirements

ExtraPutty comes with XModem support which we will need to transfer from your local device to the switch over telnet/serial.

Download: http://www.extraputty.com/

Download the firmware for the device from: https://software.cisco.com/

### Process
1. Connect device with telnet cable.
2. Launch putty session with a baud of 9600.
3. Power on switch/router and hold in mode button. Keep holding until device stop at booting prompt.
4. Release the mode button. Switch should finish booting into recovery mode.
5. Run the following command: ` format flash:` This will delete the contents of the flash.
6. (Optionally) Upgrade the baud with: `set BAUD 57600` This will increase transfer speeds.
7. Initiate the copy with: `copy xmodem: flash:(firmware name)` and then select Transfer > Xmodem > send. Selecting the correct file you wish to transfer.
8. Wait for this to finish. It will state: `File “xmodem:” successfully copied to “flash:c3550-ipservicesk9-mz.122-44.SE6.bin”`
9. Boot into the new uploaded firmware with: `boot flash:( firmware name)` You will need to reconfigure the device from scratch at this point.
10. After booting into the system run: 
```
enable
config t
no boot system
boot system flash:( firmware name)
exit
write
```
11. Done, please very your switch/router is operating properly.

### Resources

1. https://community.cisco.com/t5/networking-blogs/loading-an-ios-on-a-switch-via-xmodem/ba-p/3103557