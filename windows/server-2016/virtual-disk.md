<!-- TITLE: Server 2016 Virtual Disk -->
<!-- SUBTITLE: A quick summary of Virtual Disk -->

# Virtual Hard Disk

## Limitations

### VHD
1. Support starts in 2008 and windows 7.
2. 2 TB max size.
3. Less Resiliant.
 
### VHDX
1. Support starts in windows 8 and 2012.
2. 64 TB max size.
3. Meta Data with added resiliancy.
4. Supports 4k logical sector sizes.


## Powershell

Requires Hyper-V commandlets. 

```

# install hyper-v powershell cmdlets
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools

# view VHD related cmdlets
Get-Command -Module Hyper-V -Name *vhd*

# create a new dynamic vhdx
New-VHD -Path V:\Disks\data2.vhdx -SizeBytes 10GB -Dynamic

# mount it
Mount-VHD -Path V:\Disks\data2.vhdx

# initialize, partition, and format it
Initialize-Disk -Number 4 -PartitionStyle GPT -PassThru | New-Partition -UseMaximumSize -AssignDriveLetter | Format-Volume -FileSystem NTFS

# generate many files
1..5000 | %{ ($_ * (Get-Random -Max (100))) > "d:\file$_.txt" }

# unmount it
Dismount-VHD -Path V:\Disks\data2.vhdx

# view disk information
Get-VHD -Path V:\Disks\data2.vhdx
```
