---
title: Exchange User Phtoto Set
description: Set the user photo in exchange
published: true
date: 2020-03-05T23:36:26.412Z
tags: user, photo, exchange, set
---

# Set User Photo

This is a guide on how to change a users photo in exchange. This is currently a tough process due to issues with microsoft.

## Powershell Module

Install the below module, and select A for accepting all licenses etc.

```
Install-Module -Name ExchangeOnlineManagement
```

## Powershell Script

Connect to Exchange Online.

```
Connect-ExchangeOnline -Showprogress $true
```

This will bring up a login prompt where you need to input your admin username and password.

Next set the picture for the user as follows.
```
Set-UserPhoto -Identity "FirstName LastName" -PictureData ([System.IO.File]::ReadAllBytes("Downloads\download.jfif"))
```

It will prompt you to accept which users photo you are changing, accept with y.

Now you are finished.