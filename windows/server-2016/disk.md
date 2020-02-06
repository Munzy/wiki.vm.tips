---
title: Server 2016 Disks
description: A quick summary of Disk
published: true
date: 2020-02-06T23:19:06.722Z
tags: 
---

# Disks

* Track == One Circular track.
* Sector == Smallest block of data (4kb). 
	* Determined by the hardware manufacturer.
* Allocation Unit Size == Cluster 
	* Configured for your work load.
	* The smallest allocatable file size. For example, if you had three sectors at 4kb. You would allocate 12kb for each file.
	* Large files will preform better with larger allocation unit.


## Partition Table

### MBR - Master Boot Record
* Older
* Parititon type stored in boot sector.
* 4 Primary partions max.
* up to 2TBs in size.
* Bootable.


### GPT - GUID Partition Table
* Partition type stored with every partition.
* 128 Primary partitions.
* up to 18EB in size.
* Bootable, but with UEFI.
* Newer. 

## Partition Format

### ReFS

Came out with Server 2012.

* Data Integrity
	* Checksums.
* High Availability
* Optimized for VMs.
* Preferred File System

Limitations:
* File system compression
* File System Encryptions
* Data Deduplicatiion
* Disk Quotas.
* Removable media.
* not bootable.

### NTFS

Oldie but Goodie.

## Powershell Goodies
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