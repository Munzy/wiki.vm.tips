---
title: Server 2016 Virtualization
description: A quick summary of Virtualization
published: true
date: 2020-02-06T23:19:30.914Z
tags: 
---

# Virtualization

## MAP Toolkit

A toolkit for how and if we are ready to roll out virtualization. 


## What Not To Virtualize

* Extreme Performance Requirements.
* High Security Requirements.
* Physical Hardware Dependencies.
* Applcations that Don't Permit it.

## Create VM

### Generations

|      Generation 1     	|       Generation 2      	| Generation 2 (improvements) 	|
|:---------------------:	|:-----------------------:	|:---------------------------:	|
|    Emulation Based    	|      Software Based     	|            Faster           	|
|       32/64 Bit       	|          64 Bit         	|  Remove of 32 bit support.  	|
|     IDE Controller    	| Virtual SCSI Controller 	|    Boot from VHDX (64TB)    	|
|       IDE CD-ROM      	|   Virtual SCSI DC-ROM   	|        Hot Add/Remove       	|
|      Legacy BIOS      	|      UEFI Firmware      	|         Secure Boot         	|
|       Legacy NIC      	|      Synthetic NIC      	| Network Boot with IPv4/IPv6 	|
|   Floppy Controller   	|    No floppy support    	|      Removal of Floppy!     	|
|   UART for COM ports  	|         Optional        	|   Faster and more reliable  	|
| PS/2 Keyboard & Mouse 	|      Software-bsed      	| No emulation, less overhead 	|
|        S3 Video       	|      Software-based     	| No emulation, less overhead 	|
|        PCI Bus        	|          VM Bus         	|             N/A             	|


Generation 1 is emulation based.
Generation 1 supports 32 bit, where generation 2 doesn't.

Generation 2 is software based.



### PowerShell

```

# start powershell session on HV1-NUG
Enter-PSSession HV1-NUG

# create generation 2 VM
New-VM -Name GUI-NUG -MemoryStartupBytes 2GB -NewVHDPath V:\VMs\GUI-NUG.vhdx -NewVHDSizeBytes 40GB -Generation 2

# add SCSI controller for DVD drive
Add-VMScsiController -VMName GUI-NUG

# add DVD drive to SCSI controller with Server 2016 installation media mounted
Add-VMDvdDrive -VMName GUI-NUG -ControllerNumber 1 -ControllerLocation 0 -Path V:\ISOs\WindowsServer2016.ISO

# create another virtual hard disk for data drive
New-VHD -Path V:\VMs\GUI-NUG_data1.vhdx -SizeBytes 100GB -Dynamic

# attach data disk
Add-VMHardDiskDrive -VMName GUI-NUG -Path V:\VMs\GUI-NUG_data1.vhdx

# start the VM
Start-VM -Name GUI-NUG

# exit powershell session
Exit-PSSession

# launch virtual machine connection
vmconnect
```

## Configure VM



### Powershell
```

# start powershell session on HV1-NUG
Enter-PSSession HV1-NUG


### Dynamic Memory ###

# configure dynamic memory
Get-VM | Set-VM -MemoryStartupBytes 2GB -DynamicMemory -MemoryMinimumBytes 1GB -MemoryMaximumBytes 3GB

# configure buffer and weight
Set-VMMemory -VMName CORE-NUG -Buffer 10 -Priority 100


### NUMA ###

# view hosts NUMA topology
Get-VMHostNumaNode

# disable NUMA spanning
Set-VMHost -NumaSpanningEnabled $false


### Smart Paging ###

Get-VM | Set-VM -SmartPagingFilePath V:\Paging


### Resource Metering ###

# enable resource metering
Get-VM | Enable-VMResourceMetering

# view usage stats
Get-VM | Measure-VM

# configure collection interval
Set-VMHost -ResourceMeteringSaveInterval 00:01:00

# create a resource pool to measure memory
New-VMResourcePool -Name HV1RP -ResourcePoolType Memory

# add VMs memory to pool
Set-VMMemory -ResourcePoolName HV1RP -VMName CORE-NUG,GUI-NUG

# view pool usage stats
Measure-VMResourcePool -Name HV1RP | ft *mem*

# disable resource metering
Get-VM | Disable-VMResourceMetering


# exit powershell session
Exit-PSSession
```

## Manage



### Power Shell
```

# start powershell session on HV1-NUG
Enter-PSSession HV1-NUG


### Hot/ Add/Remove Memory ###

# add more memory to both machines
Get-VM | Set-VMMemory -StartupBytes 3072
Get-VM


### Enhanced Session Mode ##

# enable enhanced session mode support on the host
Set-VMHost -EnableEnhancedSessionMode $true


### Secure Boot ###

# enable secure boot
Get-VM | Set-VMFirmware -EnableSecureBoot On -SecureBootTemplate MicrosoftWindows # <- MicrosoftUEFICertificateAuthority (linux)
Get-VM | Get-VMFirmware


### Direct Device Assignment (DDA) ##

# DDA cmdlets
Get-Command *device* -Module Hyper-V


### Integration Services ###

# view integration services
Get-VMIntegrationService -VMName GUI-NUG

# enable guest services
Enable-VMIntegrationService -VMName GUI-NUG -Name "Guest Service Interface"


### Import & Export VMs ###

# export a vm
Export-VM -Name GUI-NUG -Path \\hv2-nug\v$\vms\exports

# import a vm
Invoke-Command -ComputerName HV2-NUG -ScriptBlock { Import-VM -Path V:\vms\exports\GUI-NUG }


### Convert VMs from Previous Versions ###

# upgrade vm version to server 2016 hyper-v (8.0)
Get-VM | Update-VMVersion


# exit powershell session on HV1-NUG
Exit-PSSession HV1-NUG
```

## Storage
### Power Shell
```

# remote over to HV1-NUG
Enter-PSSession -ComputerName HV1-NUG


# create a VHDX
New-VHD -Path v:\VMs\CORE-NUG_data2.vhdx -SizeBytes 100GB -Dynamic
Add-VMHardDiskDrive -VMName CORE-NUG -Path v:\VMs\CORE-NUG_data2.vhdx -ControllerType SCSI -ControllerNumber 0 -ControllerLocation 2

# configure storage QoS
Set-VMHardDiskDrive -VMName CORE-NUG -ControllerType SCSI -ControllerNumber 0 -ControllerLocation 2 -MinimumIOPS 0 -MaximumIOPS 100

# create a pass-through disk
Add-VMScsiController -VMName CORE-NUG
Add-VMHardDiskDrive -VMName CORE-NUG -DiskNumber 1 -ControllerType SCSI -ControllerNumber 1 -ControllerLocation 0

# create a vhd set
New-VHD -Path v:\shared.vhds -SizeBytes 100GB -Dynamic

# create a diff disk
New-VHD -Path V:\VMs\GUI-NUG_diff2.vhdx -ParentPath V:\VMs\GUI-NUG_diff1.vhdx -Differencing


# exit remote session
Exit-PSSession
```