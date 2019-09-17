<!-- TITLE: Linux Migrations -->
<!-- SUBTITLE: A few different options for migrating linux servers to another platform. -->

# Migrations
Sometimes we need to move a server from one provider to the next. Some methods however will not work with your provider. This is because of how they setup there servers or vms during deployment.


## VM to VM 

### VMWare Converter

This method uses the proprietary VMWare Converter Software. This method is great when going to a VMWare product such as VMWare ESXi. This method doesn't always work. Especially if the source VM/Server has a non standard boot provider.

If you wish to get the VMWare converter. Head to the VMWare website and login. https://my.vmware.com/web/vmware/login.
Afterwards you can download the VMWare Converter client from: https://www.vmware.com/go/getconverter.

You will need a Microsoft Windows machine to run it off of. Run the installer, and follow the default prompts. Make sure to pick Local installation. 

#### VMWare to VMWare
1. Turn off the source VM. You can only use VMWare converter to do offline migrations.
2. Click Convert Machine in VMWare Converter.
3. Select the Source Type as Powered Off.
4. Then select the appropriate type. Vmware Infastructure Client is what you will generally use.
5. Input the source host server details, username, and password and click next.
6. Select the VM you wish to move. 
7. Input the destination host server details, username, and password and click next. 
8. Place the VM into the appropriate locations, and select the destination host and storage. 
9. Select Data to Copy and change from Thick to Thin. Assuming you want thin provisioning.
10. On the Networks section change the Controller Type to the current controller type. Also set the networks that you wish the VM to connect to.
11. Select Advanced and determine if you want the VM to power on after conversation.
12. Click next, and verify your settings. Then simply start the conversion.
***13. Make sure to leave your old VM in an offline state for a few days, and to have a backup just in case the new one has issues.***


#### VM to VMware
1. Select the source type as Powered On.
2. Determine the most appropriate conversion type and input the IP Address and credentials of the device. 
3. Input the destination host server details, username, and password and click next. 
4. Place the VM into the appropriate locations, and select the destination host and storage. 
5.  Select Data to Copy > Destinaton Layout and change from Thick to Thin. Assuming you want thin provisioning.
6.  On the Networks section change the Controller Type to the current controller type. Also set the networks that you wish the VM to connect to.
7.  Click next, and verify your settings. Then simply start the conversion.
8. Once the migration has finished. It is best to turn off the source host. Turn on the destination VM, and verify IP connectivity. Make sure everything is working as intended.
9. Finally as some final polishing make sure to install vmware-tools.
***10. Make sure to leave your old source host in an offline state for a few days, and to have a backup just in case the new one has issues.***



### Live CD DD

One of the best methods I have found is the Live CD DD migration. You will need a live CD from the likes of Ubuntu, Debian, or some other linux distro. 

You can find some Live CD options here: https://ubuntu.com/download/desktop

For this method to properly work, there must be a root drive with partitions on it. For example if we have a partition of /dev/sda1 there has to be a backing device of /dev/sda. Further a partition of /dev/sda with no backing device will not work. 

1. Boot the live CD to your destination VM. Get into the live interface, and do not install the OS.
2. Verify IP connectivity to your source VM.
3. Find the root drive on your source VM. Should be something like /dev/sda.
4. Determine the root drive on your destination vm. Should be something like /dev/vda.
5. Now using your destination VM, launch a bash prompt and sudo into root.
6. Run ```ssh root@source-vm-ip "dd if=/dev/sda" | dd of=/dev/vda```
7. Wait for the job to finish. It is sometimes recommended to do a fsck while you are it to verify everything is ok.
8. Once done, reboot your VM and verify that the destination is working as intended. 
9. Set the new IP address if you need to, and turn off the old VM once you feel everything is good.
***10. Make sure to leave your old VM in an offline state for a few days, and to have a backup just in case the new one has issues.***



