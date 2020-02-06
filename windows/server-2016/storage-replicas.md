---
title: Server 2016 Storage Replicas
description: A quick summary of Storage Replicas
published: true
date: 2020-02-06T23:19:24.915Z
tags: 
---

# Storage Replicas
Built for Disaster Recovery.

* Volume-Based Block-Level Replication
* Async and Synchronous 
	* Sync garunteed... but needs high bandwidth
	* Async low bandwidth needs.
* Storage Hardware Agnostic.
* Supports three configurations
	* Server to Server
		* Between two servers.
		* Supports only Manual Failover.
		* Fail over location is inaccessible.
	* Cluster to Cluster
		* Adds disaster recovery between HA clusters.
		* Manual Failover.
		* Fail over location is inaccessible.
	* Strech Cluster
		* Single Cluster
		* Spans geographical boundry.
		* Supports automatic failover.
			* Site Awareness.


Requires two volumes:
1. Replica
2. Log


## Powershell

```

# install storage replica feature on replication partners
Invoke-Command -ComputerName FS1-NUG,FS2-NUG -ScriptBlock { Install-WindowsFeature -Name Storage-Replica,FS-FileServer -IncludeManagementTools -Restart }

# test out a potential partnership
Test-SRTopology -SourceComputerName FS1-NUG -SourceVolumeName R: -SourceLogVolumeName L: `
                -DestinationComputerName FS2-NUG -DestinationVolumeName R: -DestinationLogVolumeName L: `
                -DurationInMinutes 1 -ResultPath c:\nuggetlab -IgnorePerfTests

# create a storage replica partnership
New-SRPartnership -SourceComputerName FS1-NUG -SourceRGName FS1RG -SourceVolumeName R: -SourceLogVolumeName L: `
                  -DestinationComputerName FS2-NUG -DestinationRGName FS2RG -DestinationVolumeName R: -DestinationLogVolumeName L:

# view replication status
(Get-SRGroup).Replicas
Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica

# add some files to the source volume
"Hello!" > r:\file.txt
"Was it me," > r:\file2.txt
"you were looking for?" > r:\file3.txt
fsutil file createnew r:\file4.dat 1000000000

# reverse replication
Set-SRPartnership -NewSourceComputerName FS2-NUG -SourceRGName FS2RG -DestinationComputerName FS1-NUG -DestinationRGName FS1RG

# remove replication and clean up
Get-SRPartnership | Remove-SRPartnership

Invoke-Command -ComputerName FS1-NUG,FS2-NUG -ScriptBlock {
    Get-SRGroup | Remove-SRGroup
}
```