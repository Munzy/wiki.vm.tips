---
title: Commands
description: A quick summary of Commands
published: true
date: 2020-04-20T17:11:37.076Z
tags: 
---

# Common Cisco Commands

### ASA

## Restart HTTP Server

```
config t
no http server enable
http server enable
```

## Show AnyConnect Users

```
sh vpn-sessiondb anyconnect
```

Preview:
```
Username     : <user>              Index        : 63630
Assigned IP  : <internal ip>         Public IP    : <remote ip>
Protocol     : IKEv2 IPsecOverNatT AnyConnect-Parent
License      : AnyConnect Essentials
Encryption   : IKEv2: (1)AES256  IPsecOverNatT: (1)AES256  AnyConnect-Parent: (1)none
Hashing      : IKEv2: (1)SHA512  IPsecOverNatT: (1)SHA1  AnyConnect-Parent: (1)none
Bytes Tx     : 87221951               Bytes Rx     : 11679720
Group Policy : <group policy>
Tunnel Group : <tunnel group>
Login Time   : 06:46:18 PDT Wed Apr 1 2020
Duration     : 2h:37m:44s
Inactivity   : 0h:00m:00s
VLAN Mapping : N/A                    VLAN         : none
Audt Sess ID : 0aefdfc80f88e0005e849b2a
Security Grp : none

```


## Switch 
### Find err-disabled

```
show interfaces status err-disabled
```