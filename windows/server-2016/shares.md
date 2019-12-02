<!-- TITLE: Server 2016 Shares -->
<!-- SUBTITLE: A quick summary of Shares -->

# Shares
## Share Types

### Share

* Only accessible if you are the owner or creator of the folder.
* Less features.
* Basic functionality.
* Read - Read/Write permissions.

### Advanced Share

The windows feature "File Server" will be auto installed by Windows if you Select "Share this Folder", and select ok.

* Advanced.
* Suggested for File Sharing in a corperate enviornment.
* Can set name of share.
* Setup Caching
* Setup max number of connections.
* Advanced Permissions.
* Access Based Enumeration (Hide files that the user doesn't have access to.)
* Encryption during transfer.


## Powershell

### Configure Shares

```
# generate test files
.\Generate-Files.ps1 -Path I:\Software -NumFiles 10 -TotalSize 2GB -FileType Binary
.\Generate-Files.ps1 -Path I:\Training -NumFiles 100 -TotalSize 5GB -FileType Media
.\Generate-Files.ps1 -Path U:\Company\Docs -NumFiles 200 -TotalSize 500MB -FileType Documents
.\Generate-Files.ps1 -Path U:\Stuff -NumFiles 100 -TotalSize 1GB -FileType All


# install the file server role service
Install-WindowsFeature fs-fileserver

# install the server for nfs role service (unix/linux)
Install-WindowsFeature fs-nfs-service

# view smb and nfs share related cmdlets
Get-Command -Module smbshare,nfs

# create and view smb share
New-SmbShare -Path U:\Stuff -Name UserStuff -FolderEnumerationMode AccessBased -FullAccess nuggetlab\human -ChangeAccess nuggetlab\ewok -ReadAccess nuggetlab\droid
Get-SmbShare -Name UserStuff

# view, add or remove smb access rights
Get-SmbShareAccess -Name UserStuff
Grant-SmbShareAccess -Name UserStuff -AccessRight Full -AccountName nuggetlab\wookiee -Force
Revoke-SmbShareAccess -Name UserStuff -AccountName nuggetlab\droid -Force

# block or unblock smb access
Block-SmbShareAccess -Name UserStuff -AccountName 'nuggetlab\darth.vader' -Force
Unblock-SmbShareAccess -Name UserStuff -AccountName 'nuggetlab\darth.vader' -Force

# monitor and manage share sessions
Get-SmbSession
Close-SmbSession -ClientComputerName 192.168.1.120

# configure smb server side
Get-SmbServerConfiguration
Set-SmbServerConfiguration -AuditSmb1Access $true -EnableSMB1Protocol $false -Force

# configure smb client side
Get-SmbClientConfiguration
Set-SmbClientConfiguration -SessionTimeout 120 -Force
```

### Generate Random Fun

```


[cmdletbinding()]
param(
	[Parameter(mandatory=$true)]$NumFiles,
    [Parameter(mandatory=$true)][ValidateSet("Media","Documents","Binary","All")]$FileType,
	[Parameter(mandatory=$true)]$Path,
	[Parameter(mandatory=$true)]$TotalSize
)
 
begin{
	Write-verbose "Generating files..."
	$GeneratedFiles = @()
 
function Generate-FileName{
[CmdletBinding(SupportsShouldProcess=$true)]
	param()
	begin {
		$Extensions = @()
        switch ($FileType) {
          "Media" { $Extensions = ".avi",".midi",".mov",".mp3",".mp4",".mpeg",".mpeg2",".mpeg3",".mpg",".ogg",".ram",".rm",".wma",".wmv" }
          "Documents" { $Extensions = ".docx",".doc",".xls",".docx",".doc",".pdf",".ppt",".pptx",".dot" }
          "Binary" { $Extensions = ".exe",".msi",".msu",".iso" }
          "All" { $Extensions = ".exe",".msi",".msu",".iso",".avi",".midi",".mov",".mp3",".mp4",".mpeg",".mpeg2",".mpeg3",".mpg",".ogg",".ram",".rm",".wma",".wmv",".docx",".doc",".xls",".docx",".doc",".pdf",".ppt",".pptx",".dot" }
        }
        $extension = $null
	}
	process{
		Write-Verbose "Generating filename..."
 
        $extension = $Extensions | Get-Random
		Get-Verb | Select-Object verb | Get-Random -Count 2 | %{ $Name+= $_.verb }
		$FullName = $name + $extension

		Write-Verbose "Filename : $FullName"
	}
	end{
 
	return $FullName
	}
 
}
 
}
 
process{
	$FileSize = $TotalSize / $NumFiles
	$FileSize = [Math]::Round($FileSize, 0)
 
	while ($TotalFileSize -lt $TotalSize) {
		$TotalFileSize = $TotalFileSize + $FileSize
 
		$FileName = Generate-FileName
 
		Write-verbose "Filename: $filename"
 
		Write-Verbose "Filesize: $filesize"
 
		$FullPath = Join-Path $path -ChildPath $fileName
		Write-Verbose "Generating file : $FullPath of $Filesize"
		try{
			fsutil.exe file createnew $FullPath $FileSize | Out-Null
			}
		catch{
			$_
		}
 
		$FileCreated = ""
		$Properties = @{'FullPath'=$FullPath;'Size'=$FileSize}
 
		$FileCreated = New-Object -TypeName psobject -Property $properties
		$GeneratedFiles += $FileCreated
		Write-verbose "$($AllCreatedFilles) created $($FileCreated)"
	}
 
}
end{
	Write-Output $GeneratedFiles
}
```