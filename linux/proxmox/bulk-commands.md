---
title: Proxmox Bulk Commands
description: 
published: true
date: 2020-04-23T04:47:22.662Z
tags: proxmox, bulk, commands
---

# Proxmox Bulk Commands

## Set CPU to Host
```
for i in $(qm list | awk '{if (NR>1) { print $1 }}'); do qm set $i -cpu "host"; done
```