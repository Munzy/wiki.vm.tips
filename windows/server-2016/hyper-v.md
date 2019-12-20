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


## Install

### Powershell

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


## Configure

### Powershell

```

# hyper-v cmdlets
Get-Command -Module Hyper-V


### Remote Hyper-V Host Management ##

# single-use remote sessions
Invoke-Command -ComputerName HV1-NUG,HV2-NUG -ScriptBlock { Get-VMHost }
Invoke-Command -ComputerName HV1-NUG,HV2-NUG -ScriptBlock { Set-VMHost -VirtualHardDiskPath V:\VMs -VirtualMachinePath V:\VMs }

# interactive remote session
Enter-PSSession -ComputerName HV1-NUG
Get-VM
Start-VM -Name GUI-NUG
Exit-PSSession

# persistent remote session
$hv2 = New-PSSession -ComputerName HV2-NUG
Enter-PSSession -Session $hv2
Get-VM
Start-VM -Name CORE-NUG
Exit-PSSession


### PowerShell Direct Virtual Machine Management ###

# single-use powershell direct session 
Invoke-Command -VMName CORE-NUG -Credential (Get-Credential) -ScriptBlock { Get-ComputerInfo }

# interactive powershell direct session
Enter-PSSession -VMName CORE-NUG -Credential (Get-Credential)
Get-WindowsFeature
Exit-PSSession

# persistent powershell direct session
$core = New-PSSession -VMName CORE-NUG -Credential (Get-Credential)
Enter-PSSession -Session $core
"this file was created on core-nug" > c:\core-nug.txt
Exit-PSSession

# copy files to/from VM
"this file was created on hv2-nug" > c:\hv2-nug.txt
Copy-Item -Path c:\hv2-nug.txt -Destination C:\ -ToSession $core
Copy-Item -Path c:\core-nug.txt -Destination C:\ -FromSession $core

# clean up sessions
Exit-PSSession
Remove-PSSession $core
Exit-PSSession
Remove-PSSession $hv2


### Nested Virtualization ###

# interactive session on HV1-NUG
Enter-PSSession -ComputerName HV1-NUG

# attempt to install Hyper-V role on GUI-NUG (fail...)
Invoke-Command -VMName GUI-NUG -ScriptBlock { Install-WindowsFeature Hyper-V -IncludeManagementTools }

# stop the vm
Stop-VM GUI-NUG

# enable nested virt on HV1-NUG for GUI-NUG VM
Set-VMProcessor -VMName GUI-NUG -ExposeVirtualizationExtensions $true

# start the vm
Start-VM GUI-NUG

# interactive session on HV1-NUG
Enter-PSSession -ComputerName HV1-NUG

# attempt to install Hyper-V role on VM (success!)
Invoke-Command -VMName GUI-NUG -ScriptBlock { Install-WindowsFeature Hyper-V -IncludeManagementTools }
```


## Storage 

### Powershell

```
# remote over to HV1-NUG
Enter-PSSession -ComputerName HV1-NUG

# install roles and features into a VHDX
Install-WindowsFeature -Name Web-Server -Vhd V:\VMs\CORE-NUG.vhdx

# compact a VHDX
Optimize-VHD -Path V:\VMs\CORE-NUG_data1.vhdx

# resize a VHDX
Resize-VHD -Path V:\VMs\CORE-NUG_data1.vhdx -SizeBytes 10GB

# modify a VHDX
Convert-VHD -Path V:\VMs\CORE-NUG_data1.vhdx -DestinationPath V:\VMs\CORE-NUG_data.vhdx -VHDType Fixed -DeleteSource

# merge diff disks
Merge-VHD -Path V:\VMs\CORE-NUG_diff2.vhdx -DestinationPath V:\VMs\CORE-NUG_diff1.vhdx


# checkpoints #
Set-VM -Name CORE-NUG -CheckpointType Production
Start-VM -VMName CORE-NUG

# create a base checkpoint
Checkpoint-VM -Name CORE-NUG -SnapshotName CORE-NUG_base

# install container feature and checkpoint
Invoke-Command -VMName CORE-NUG -ScriptBlock { Install-WindowsFeature FS-FileServer -Restart }
Checkpoint-VM -Name CORE-NUG -SnapshotName CORE-NUG_fileserver

# install data dedup and checkpoint
Invoke-Command -VMName CORE-NUG -ScriptBlock { Install-WindowsFeature FS-Data-Deduplication }
Checkpoint-VM -Name CORE-NUG -SnapshotName CORE-NUG_dedup

# view checkpoints
Get-VMSnapshot -VMName CORE-NUG

# revert back to container checkpoint
Restore-VMSnapshot -VMName CORE-NUG -Name CORE-NUG_fileserver

# revert back to base checkpoint
Restore-VMSnapshot -VMName CORE-NUG -Name CORE-NUG_base

# remove all checkpoints
Get-VMSnapshot -VMName CORE-NUG | Remove-VMSnapshot


# exit remote session
Exit-PSSession
```