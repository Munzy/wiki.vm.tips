---
title: Server 2016 Storage Pools
description: A quick summary of Storage Pools
published: true
date: 2020-02-06T23:19:22.848Z
tags: 
---

# Storage Pools

Adds two layers of abstraction for storage. 

### Normal Setup
1. Volumes
2. Physical Layer

### Storage Pool

1. Volumes
2. Storage Spaces (Virtual Disks)
3. Sorage Pools
4. Physical Layer


### Benefits

* Allows for diffferents sizes, types, and interfaces in regards to physical storage.
* Enclosure Awareness
	* Spread across multiple multiple JBODs.
* Storage can be Local or Direct.
* Resiliancy Settings
	* Simple
		* Raid 0
		* FAST!!!!
		* YOLO!~
	* Mirror
		* Raid 1
		* Redundant and Fast!
	* Parity
		* Raid 5/6
		* Slow
* Storage Tiers
	* Move Data Between Fast and Slow drives.
		* Infrequently accessed data migrated to slow drives.
		* High accessed files migrated to fast drives.
* Write back cache.
	* Copy data to fast tier.
	* Once done, slowly trickle into slow drives.



### Powershell

```

# create a new storage pool
New-StoragePool -FriendlyName AllTheDisks -StorageSubSystemUniqueId (Get-StorageSubSystem).UniqueId -PhysicalDisks (Get-PhysicalDisk -CanPool $true)

# create virtual disk
New-VirtualDisk -FriendlyName CompanyData -StoragePoolFriendlyName AllTheDisks -Size 2TB -ProvisioningType Thin -ResiliencySettingName Mirror

# initialize underlying physical disks
Initialize-Disk -VirtualDisk (Get-VirtualDisk -FriendlyName CompanyData)

# partition and format volumes from virtual disk
$vd = Get-VirtualDisk -FriendlyName CompanyData | Get-Disk

$vd | 
    New-Partition -Size 100GB -Driveletter U |
            Format-Volume -FileSystem NTFS -AllocationUnitSize 4096 -NewFileSystemLabel "User Data"

$vd |     
    New-Partition -Size 500GB -Driveletter I |
        Format-Volume -FileSystem ReFS -AllocationUnitSize 4096 -NewFileSystemLabel "IT Data"

$vd | 
    New-Partition -Size 1TB -Driveletter V |
        Format-Volume -FileSystem ReFS -AllocationUnitSize 65536 -NewFileSystemLabel "VM Data"


```


## iSCSI

Exposes Network Storage over an IP network

### Additional Features

#### ISNS

Acts like DNS for iSCSI and allows for communication between Initiators, and targets.

#### DCB

Datacenter Bridging.
Allows SAN & LAN traffic to coexist.

#### MPIO

Multiple Path IO.
Redundancy.




### Powershell

```

# view storage related cmdlets
Get-Command -Module Storage

# view disks
Get-Disk

# initialize disks as GPT
Initialize-Disk -Number 2 -PartitionStyle GPT

# view partitions
Get-Partition

# partition an entire disk
New-Partition -DiskNumber 2 -UseMaximumSize -Driveletter I

# view volumes
Get-Volume

# format with a file system
Format-Volume -DriveLetter I -FileSystem NTFS -AllocationUnitSize 4096 -NewFileSystemLabel "IT Data"


# format remaining disk with a single command
Get-Disk | 
    Where-Object PartitionStyle -eq "RAW" | 
        Initialize-Disk -PartitionStyle GPT -PassThru | 
            New-Partition -UseMaximumSize -Driveletter V | 
                Format-Volume -FileSystem ReFS -AllocationUnitSize 65536 -NewFileSystemLabel "VM Data"
								
```