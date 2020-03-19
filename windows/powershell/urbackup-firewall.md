---
title: Urbackup Firewall Apply
description: Apply urbackup firewall policy via powershell
published: true
date: 2020-03-19T21:11:12.380Z
tags: urbackup, powershell, firewall, apply
---

# Urbackup Firewall

## Remote into Computer
```
Enter-PSSession -ComputerName <computername>
```

## Current Firewall Settings
```
Get-NetFirewallRule -DisplayName UrBackupClientBackend
```

Response:
```
Name                  : {5F4A8595-6E8E-44D8-ADA4-C291CA70900B}
DisplayName           : UrBackupClientBackend
Description           :
DisplayGroup          :
Group                 :
Enabled               : True
Profile               : Domain
Platform              : {}
Direction             : Inbound
Action                : Allow
EdgeTraversalPolicy   : Block
LooseSourceMapping    : False
LocalOnlyMapping      : False
Owner                 :
PrimaryStatus         : OK
Status                : The rule was parsed successfully from the store. (65536)
EnforcementStatus     : NotApplicable
PolicyStoreSource     : PersistentStore
PolicyStoreSourceType : Local
```
## Set Firewall Settings

### Any

```
Set-NetFirewallRule -DisplayName UrBackupClientBackend -Profile Any
```
