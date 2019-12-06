<!-- TITLE: Server 2016 Deduplication -->
<!-- SUBTITLE: A quick summary of Deduplication -->

# Deduplication

Introduced in Server 2012.
Removes redundant data.

1. Dramatically reduces storage costs. (maybe)
2. Redundant data stored only once.
3. Jobs run in the background.
4. New Performance and Scalability features.


Post Deduplication, not a deduplicaiton on write like ZFS.



## Tasks

|     Job     	|       Operations     	|  Run Time |
|:-----------:	|:----------------:	|:----------------:	|
|   Optimzation  	|    Deduplicates Data | Hourly | 
|   Garbage Collector  	|    Reclaims hard drive space| Once a week | 
|   Integrity Scrubbing  	|    Identifies corruption | Once a week | 
|   Unoptimization   	|   Reverts and disables dedup | Manual | 


## Usage Types

|     Usage Types     	|       Description     	|  Process  |
|:-----------:	|:----------------:	|:----------------:	|
|   Default  	|    General Purpose File Servers | background | 
|   Hyper-V 	|    VDI Server  | Background | 
|   IBackup  	|    Birtualized backup app | Priority  | 


# Updates from 2012

1. Multi-threaded
2. 64 TB Volumes.
3. 1 TB files.
4. Supported on Nano server.


## Powershell

```

# install data deduplication
Install-WindowsFeature -Name FS-Data-Deduplication

# dedup evaluation tool
ddpeval U:
ddpeval I:
ddpeval V:

# enable data deduplication
Enable-DedupVolume -Volume V: -UsageType HyperV

# view and set volume-wide settings
Get-DedupVolume -Volume V: | Format-List *
Set-DedupVolume -Volume V: -MinimumFileAge 0

# manually run optimization job
Start-DedupJob -Type Optimization -Volume V: -Priority High -Memory 100 -Cores 100

# monitor running jobs
Get-DedupJob

# view overall status
Get-DedupStatus | Format-List *

# view and set schedule
Get-DedupSchedule
Set-DedupSchedule -Name ThroughputOptimization -Enabled $false

# disable deduplication
Start-DedupJob -Type Unoptimization -Volume V:
```