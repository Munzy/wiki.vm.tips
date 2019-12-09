<!-- TITLE: Server 2016 Hyper V -->
<!-- SUBTITLE: A quick summary of Hyper V -->

# Hyper-V

## Changes

|     Resource     	|       2012     	|  2016 |
|:-----------:	|:----------------:	|:----------------:	|
|   Processors  	|    320 | 512 | 
|   vCPU  	|    2048 | 2048 | 
|   Memory 	|    4TB | 24TB | 
|   Active VMs   	|   1024  | 1024 | 

## Requirements

### Host System

1. SLAT-Capable Processor
2. VM Monitor Mode extensions
3. Hardware-Assisted Virtualization
4. Hardware-Enforced DEP

This can be determined with:

`systeminfo`
`Get-ComputerInfo`


### Upgrade to 2016
* Export or migrate VMS off host.
* Clean install of Server 2016
* Import or migrate VMS onto host.
* Upgrade VM Configuration Version.



## Powershell

```

### HV1-NUG (Desktop) ###

# install Hyper-V remotely
Install-WindowsFeature -ComputerName HV1-NUG -Name Hyper-V -IncludeManagementTools -Restart


### HV2-NUG (Nano) ###

# enter remote session
Enter-PSSession -ComputerName HV2-NUG

# import provider, view installed and available packages
Import-PackageProvider -Name NanoServerPackage
Get-Package -ProviderName NanoServerPackage
Find-Package -ProviderName NanoServerPackage

# install the hyper-v role
Install-NanoServerPackage -Name Microsoft-NanoServer-Compute-Package

# reboot
Restart-Computer


### ADMIN-NUG (Local) ###

# install GUI and PS tools
Get-WindowsOptionalFeature -Online -FeatureName *Hyper-V*
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Tools-All -All
```