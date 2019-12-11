<!-- TITLE: Server 2016 Virtualization -->
<!-- SUBTITLE: A quick summary of Virtualization -->

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
|:---------------------:	|:-----------------------:	|-----------------------------	|
|     IDE Controller    	| Virtual SCSI Controller 	| Boot from VHDX (64TB)       	|
|       IDE CD-ROM      	|   Virtual SCSI DC-ROM   	| Hot Add/Remove              	|
|      Legacy BIOS      	|      UEFI Firmware      	| Secure Boot                 	|
|       Legacy NIC      	|      Synthetic NIC      	| Network Boot with IPv4/IPv6 	|
|   Floppy Controller   	|    No floppy support    	| Removal of Floppy!          	|
| UART for COM ports    	| Optional                	| Faster and more reliable    	|
| PS/2 Keyboard & Mouse 	| Software-bsed           	| No emulation, less overhead 	|
| S3 Video              	| Software-based          	| No emulation, less overhead 	|
| PCI Bus               	| VM Bus                  	| N/A                         	|



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