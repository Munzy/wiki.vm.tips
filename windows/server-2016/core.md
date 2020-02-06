---
title: Server 2016 Core Basics
description: A quick summary of Core
published: true
date: 2020-02-06T23:19:00.304Z
tags: 
---

# Core Basics

## Server Manager

https://www.microsoft.com/en-us/Download/confirmation.aspx?id=8053&mstLocPickShow=True

Download will allow you to remotely manager your server core instances from a GUI.

## SConfig

Sconfig provides a text-based menu utility for configure server core. 

![Chrome Altnqq 5 Net](/uploads/chrome-altnqq-5-net.png "Chrome Altnqq 5 Net")


## Powershell

### Help Docs

To update the help docs to the latest and greatest run.
```
Update-Help
```


### Remote Session

Enter Session:
```
Enter-PSSession -ComputerName <name>
```
Exit Session:
```
Exit-PSSession
```

## Task Manager

Enter CTRL+ALT+DEL.

Select "Task Manager".

![Chrome Usunrfw 3 T 0](/uploads/chrome-usunrfw-3-t-0.png "Chrome Usunrfw 3 T 0")

## Network Settings

### The easiest way to set this is via sconfig.

![Ipsettings](/uploads/ipsettings.png "Ipsettings")


### IP Address

#### Set IP Address

From cmd.prompt you can use:

```
netsh interface ipv4 set address name="Ethernet" static <ip> <subnet> <gateway>
```

From powershell you can use:

```
Get-NetAdapter
```
Then:
```
New-NetIPAddress -InterfaceIndex 2 -IPAddress <ip> -PrefixLength 24 -DefaultGateway <gateway>
```

#### Verify IP Address

cmd.prompt:

```
ipconfig
```

powershell:
```
Get-NetIPConfiguration
```


### DNS 

From cmd.prompt you can use:

```
netsh interface ipv4 set dns name="Ethernet" static <ip>
netsh interface ipv4 set dns name="Ethernet" static <ip> index=2
```

From powershell you can use:

```
Set-DnsClientServerAddress -InterfaceIndex 2 -ServerAddresses ("<dns-pri>","<dns-sec">)
```

### Firewall

From cmd.prompt use:

```
netsh
netsh> advfirewall
netsh advfirewall> set allprfiles state on/off
```

From powershell you can use:

```
Set-NetFirewallProfile -Profile domain,public,private -Enabled True/False
```

### Rename Computer

From cmd.prompt:

```
netdom renamecomputer %computername$ /newname:<name>
```

From powershell:
```
Rename-Computer -NewName <name> -Restart
```

### Verify ComputerName

From cmd.prompt:

```
hostname
```

From powershell:
```
$env:COMPUTERNAME
```

### Join Domain

Make sure the DNS servers are set to the Domain Controllers IP Address.


cmd.prompt:
```
netdom join %computername% /domain:<domain> /userd:<admin> /passwordd:<pass>
```

powershell:
```
Add-Computer -DomainName <domain>
```

Make sure to restart the computer.

## Power Operations

### Restart

cmd.prompt
```
shutdown -r -t 0 
```

powershell:
```
Restart-Computer
```

## Install Roles
powershell:
```
# View all available features
Get-WindowsFeature

# Install web server (iis)
Install-WindowsFeature -Name Web-Server, Web-Mgmt-Service

# View installed features
Get-WindowsFeature | Where-Object Installed -eq True


# Configure Remote Management
Set-ItemPropertey -Path "HKLM:\Software\Microsoft\WebManagement\Server" -Name "EnableRemoteManagement" -Value 1

# Configure Remote Management Service to autostart
Set-Service WMSVC -StartupType Automatic

# Rename Computer
Rename-Computer -NewName Web-Server -DomainCredential "<domain>\<user>" -Force -Restart

# Send a Command
Invoke-Command -ComputerName <name> -ScriptBlock { Get-Service W3SVC, WMSVC }

# Use the built in computername paramater (( Same as above ))
Get-Service -ComputerName <name>
```