---
title: VMware Upgrade
description: A quick summary of Upgrade
published: true
date: 2020-02-06T23:18:21.271Z
tags: 
---

# VMware Upgrade


## Online
 ### Guide
1. Enable the firewall rule.
```
esxcli network firewall ruleset set -e true -r httpClient
```
2.  Get a list of avilable packages. You can get more info on the right build number by looking at the vmware update sheet on their website. 
https://docs.vmware.com/en/VMware-vSphere/6.5/rn/esxi650-201912002.html 
You can find under the image profiles the correct build to use.
```
esxcli software sources profile list -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml | grep -i ESXi-6
```
3.  Turn off VMs and put the instance into Maitenance Mode.
4.  Apply updates.
```
esxcli software profile update -p ESXi-6.5.0-20191204001-standard -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml
```
5.  Verify update.
```
Update Result
     Message: The update completed successfully, but...
```
6.  Remove Maitenance Mode.
7.  Re-apply firewall ruleset.
```
esxcli network firewall ruleset set -e false -r httpClient
```
8.  Reboot server.
9.  Start remaining VMs.


### Problem Resolution

If you have issues applying the updates, things to try:

* Use the -no-tools version if it says it has run out of space.
* Enable swap in VMware, and point it at a datastore.

### Resources
[video](https://www.youtube.com/watch?v=Xkh05Wv7D3U){.youtube}
https://www.vladan.fr/how-to-upgrade-esxi-6-0-to-6-5-via-cli-on-line/