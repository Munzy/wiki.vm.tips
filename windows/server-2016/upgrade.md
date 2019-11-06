<!-- TITLE: Windows Server 2016 Upgrade -->
<!-- SUBTITLE: A quick summary of Upgrade -->

# Server 2016 Upgrade

## Clean Install + Migration

This is the suggested route for upgrades. You can go from 2008 directly to 2016.


### Install Migration Tools

Powershell:

```
Get-WindowsFeature -Name migration
Install-WindowsFeature -Name migration
```

### Create Migration Profile

```
.\smigdeploy.exe /package architecture amd64 /os WS12R2 /path C:\migration

# Copy to your source machine
Copy-Item -Path C:\migration\SMT_ws12r2_amd64 -Destination \\old_server\c$\migration -Recurse

```

### Source Machine Requirements

Source machine requires .NET 3.5 Framework

```
Get-WindowsFeature net-framework-core
Install-WindowsFeature net-framework-core -source d:\sources\sxs
```

### Source Machine Migration

```
# Deploy our commandlets
c:\migration\SMT_win12R2_amd64\smigdeploy.exe
Add-PSSapin Microsoft.Windows.ServerManager.Migration

# Services needs to be stopped for migration.
Stop-Service -Name DHCPServer

# Find the services we can migrate.
Get-SmigServerFeature

# Migrate the data
Export-SmigServerSetting -FeatureID DHCP -Path C:\migration\data -verbose

# Copy data to new server
Copy-Item -Path C:\migration\data -Destination \\new_server\c$\migration -Recurse
```

### Import Migration

```
Add-PSSapin Microsoft.Windows.ServerManager.Migration

Import-SmigServerSetting -FeatureID DHCP -Path c:\migration\data -Verbose -Force
```

## In-Place

Can only hop one version at a time. So you can't go from 2008 to 2016, you would need to do 2008, to 2012, to 2016.

This is fairly simple though. Simply go through the upgrade process via the Windows Server 2016 ISO. 
