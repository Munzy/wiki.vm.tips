<!-- TITLE: Windows Server 2016 Core -->
<!-- SUBTITLE: A quick summary of Core -->

# Core

## SConfig

Sconfig provides a text-based menu utility for configure server core. 

![Chrome Altnqq 5 Net](/uploads/chrome-altnqq-5-net.png "Chrome Altnqq 5 Net")


## Configure IP/DNS

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
