<!-- TITLE: Windows 2016 Nano Server -->
<!-- SUBTITLE: A quick summary of Nano Server -->

# 2016 Nano Server
A very small version of Windows Server. Meant for Containers, and the cloud. Very little maitenance.

## Size
.41 GB Disk
11 Ports Opens

## Nano Recovery Console

Has a very limited number of functions that can be run from its recovery console. It has no GUI/CLI.

1. Networking
2. Firewall (Inbound / outbound)
3. winRM (Reset to default state)

## Create Nano Server

```
Import-Module D:\NanoServer\NanoServerImageGenerator -Verbose

New-NanoServerImage -MediaPath D: - BasePath .\Base -TargetPath .\<name>.<vhd/vhdx/wim> -DeploymentType <Guest/Host> -Edition <Datacenter/Standard> -ComputerName <name> -AdministratorPassword (ConvertTo-SecureString 'Password' -AsPlainText -Force)

```

With IP Settings and such:
```
New-NanoServerImage -MediaPath D: - BasePath .\Base -TargetPath .\<name>.<vhd/vhdx/wim> -DeploymentType <Guest/Host> -Edition <Datacenter/Standard> -ComputerName <name> -InterfaceNameOrIndex Ethernet -IPV4Address 192.168.1.200 -IPv4Subnetmask 255.255.255.0 -IPv4Gateway 192.168.1.1 -IPv4DNS ("192.168.1.100,8.8.8.8") -Package Microsoft-NanoServer-IIS-Package -AdministratorPassword (ConvertTo-SecureString 'Password' -AsPlainText -Force)
```

## Start Nano VM

```
new-VM <name> -VHDPath .\<name>.<vhd/vhdx> _MemeoryStartupBytes 1GB -Generation 2 | Start-VM
```


See in HyperVM:
```
virtmgmt.msc
```

## Connect

```
Enter-PSSession -VMName <name>
```

## Commands

```
Get-Process
Get-Service
Get-Childitem C:
Get-WinEvent
```




