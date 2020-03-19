---
title: Urbackup
description: Apply urbackup settings via powershell
published: true
date: 2020-03-19T22:05:28.842Z
tags: urbackup, powershell, firewall, apply, service
---

# Urbackup

## Remote into Computer
```
Enter-PSSession -ComputerName <computername>
```

## Firewall

### Current Firewall Settings
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
### Set Firewall Settings

#### Any

```
Set-NetFirewallRule -DisplayName UrBackupClientBackend -Profile Any
```

## Service

### Service Status
```
Get-Service -name UrBackupClientBackend
```
Response:
```

Status   Name               DisplayName
------   ----               -----------
Running  UrBackupClientB... UrBackup Client Service for Backups

```

### Set Status
```
Set-Service -name UrBackupClientBackend -StartupType Automatic
```

### Start Service
```
start-Service  -Name UrBackupClientBackend
```


