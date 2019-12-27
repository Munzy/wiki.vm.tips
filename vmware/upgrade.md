<!-- TITLE: VMware Upgrade -->
<!-- SUBTITLE: A quick summary of Upgrade -->

# VMware Upgrade


## Online

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
esxcli software profile update -p ESXi-6.5.0-20170702001-standard -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml
```
5.  Verify update.
```
Update Result
     Message: The update completed successfully, but...
```
6.  Reboot server.
7.  Remove Maitenance Mode and start VMs.
8.  Re-apply firewall ruleset.
```
esxcli network firewall ruleset set -e false -r httpClient
```

[video](https://www.youtube.com/watch?v=Xkh05Wv7D3U){.youtube}


Resouces: https://www.vladan.fr/how-to-upgrade-esxi-6-0-to-6-5-via-cli-on-line/