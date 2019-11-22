	<!-- TITLE: Server 2016 DISM -->
<!-- SUBTITLE: A quick summary of Dism -->

# DISM

**D**eployment **I**maging **S**ervice and **M**anagement.

Works with .vhd, .vhdx, .wim.


## CMD.exe

```

### DISM - Deployment Image Servicing and Management Command-line Utility ###

## Desktop Experience & Server Core ##

# display help
dism /?

# view images within a .wim
dism /get-imageinfo /imagefile:.\images\install.wim

# mount server core datacenter image
dism /mount-image /imagefile:.\images\install.wim /index:3 /mountdir:.\mount

# add drivers (.inf)
dism /image:.\mount /get-drivers
dism /image:.\mount /add-driver /driver:.\drivers
dism /image:.\mount /remove-driver /driver:.\drivers\iaStorAC.inf

# add patches, hotfixes, and updates (.msu or .cab)
dism /image:.\mount /get-packages
dism /image:.\mount /add-package /packagepath:.\updates
dism /image:.\mount /remove-package /packagepath:.\updates\windows10.0-kb3150513.msu

# install roles and features
dism /image:.\mount /get-features
dism /image:.\mount /get-featureinfo /featurename:web-server
dism /image:.\mount /enable-feature /featurename:web-server /all
dism /image:.\mount /disable-feature /featurename:web-server

# dismount and commit all changes
dism /unmount-image /mountdir:.\mount /commit


## Nano Server ##

# view nano server images
dism /get-imageinfo /imagefile:.\images\nanoserver.wim

# mount nano server datacenter image
dism /mount-image /imagefile:.\images\nanoserver.wim /index:2 /mountdir:.\mount

# add nano server package
dism /image:.\mount /add-package /package:d:\nanoserver\packages\microsoft-nanoserver-containers-package.cab

# dismount and commit all changes
dism /unmount-image /mountdir:.\mount /discard

```

## Powershell

```

### DISM - Deployment Image Servicing and Management PowerShell Cmdlets ###

## .wim ##

# view cmdlets in DISM module
Get-Command -Module DISM

# view images within a .wim
Get-WindowsImage -ImagePath .\images\install.wim
Get-WindowsImage -ImagePath .\images\nanoserver.wim

# mount server core datacenter image
Mount-WindowsImage -ImagePath .\images\install.wim -Path .\mount -Index 3

# add drivers (.inf)
Get-WindowsDriver -Path .\mount
Add-WindowsDriver -Path .\mount -Driver .\drivers\iaStorAC.inf

# add packages (.msu or .cab)
Get-WindowsPackage -Path .\mount
Add-WindowsPackage -Path .\mount -PackagePath .\updates\windows10.0-kb3150513.msu

# install roles and features
Get-WindowsOptionalFeature -Path .\mount -FeatureName dhcpserver
Enable-WindowsOptionalFeature -Path .\mount -FeatureName dhcpserver
Disable-WindowsOptionalFeature -Path .\mount -FeatureName dhcpserver

# dismount and commit changes
Dismount-WindowsImage -Path .\mount -Save


## .vhd/vhdx ##

# mount nano server datacenter vhd
Mount-WindowsImage -ImagePath .\image\nanoserver.wim -Path .\mount -Index 2

# add iis package
Add-WindowsPackage -Path .\mount -PackagePath d:\nanoserver\packages\microsoft-nanoserver-iis-package.cab

# dismount and commit changes
Dismount-WindowsImage -Path .\mount -Save
```