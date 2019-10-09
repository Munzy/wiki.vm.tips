<!-- TITLE: Windows Server 2016 Core Basics -->
<!-- SUBTITLE: A quick summary of Core -->

# Core Basics

## SConfig

Sconfig provides a text-based menu utility for configure server core. 

![Chrome Altnqq 5 Net](/uploads/chrome-altnqq-5-net.png "Chrome Altnqq 5 Net")


## Powershell

### Help Docs

To update the help docs to the latest and greatest run.
```
Update-Help
```

## Task Manager

Enter CTRL+ALT+DEL.

Select "Task Manager".

![Chrome Usunrfw 3 T 0](/uploads/chrome-usunrfw-3-t-0.png "Chrome Usunrfw 3 T 0")

## Network Settings

### The easiest way to set this is via sconfig.

![Ipsettings](/uploads/ipsettings.png "Ipsettings")


### IP Address

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