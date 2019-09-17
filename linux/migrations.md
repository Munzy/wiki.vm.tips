<!-- TITLE: Linux Migrations -->
<!-- SUBTITLE: A few different options for migrating linux servers to another platform. -->

# Migrations
Sometimes we need to move a server from one provider to the next. Some methods however will not work with your provider. This is because of how they setup there servers or vms during deployment.


## VM to VM 

### Live CD DD

One of the best methods I have found is the Live CD DD migration. You will need a live CD from the likes of Ubuntu, Debian, or some other linux distro. 

You can find some Live CD options here: https://ubuntu.com/download/desktop

For this method to properly work, there must be a root drive with partitions on it. For example if we have a partition of /dev/sda1 there has to be a backing device of /dev/sda. Further a partition of /dev/sda with no backing device will not work. 

1. Boot the live CD to your destination VM. Get into the live interface, and do not install the OS.
2. Verify IP connectivity to your source VM.
3. Find the root drive on your source VM. Should be something like /dev/sda.
4. Determine the root drive on your destination vm. Should be something like /dev/vda.
4. Now using your destination VM, launch a bash prompt and sudo into root.
5. Run ```ssh root@source-vm-ip "dd if=/dev/sda" | dd of=/dev/vda```
6. Wait for the job to finish. It is sometimes recommended to do a fsck while you are it to verify everything is ok.
7. Once done, reboot your VM and verify that the destination is working as intended. 
8. Set the new IP address if you need to, and turn off the old VM once you feel everything is good.
***9. Make sure to leave your old VM in an offline state for a few days, and to have a backup just in case the new one has issues.***



