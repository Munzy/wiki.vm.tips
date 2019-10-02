<!-- TITLE: Cisco Router Upgrade -->
<!-- SUBTITLE: A quick summary of Cisco Router Upgrade -->

# Upgrade Cisco Router IOS

## Download

https://software.cisco.com/download/home

Search for the appropriate IOS software.

## Upgrade Steps

### Connect

Connect into the router using either serial, telnet, or ssh.

### Enable

```
enable
```

### Filesystem

```
show file systems
```

### Copy

Copy from the TFTP server.

```
copy tftp: flash:
Address or name of remote host [192.168.1.2]?
Source filename [c2951-universalk9-mz.SPA.157-3.M5.bin]?
Destination filename [c2951-universalk9-mz.SPA.157-3.M5.bin]?
Accessing tftp://192.168.1.2/c2951-universalk9-mz.SPA.157-3.M5.bin...
Loading c2951-universalk9-mz.SPA.157-3.M5.bin from 192.168.1.2 (via GigabitEthernet0/0): !!!!!![...]```

### Verify

Verify the IOS image.

```
Router#verify flash:?
flash:c2951-universalk9-mz.SPA.157-3.M4b.bin
flash:c2951-universalk9-mz.SPA.157-3.M5.bin
flash:pnp-tech-discovery-summary
flash:pnp-tech-time

Router#verify flash:c2951-universalk9-mz.SPA.157-3.M5.bin
Starting image verification
Hash Computation:    100% Done!
Computed Hash   SHA2: 386DBDCEF0738CE96F2706AABA5007BA
                      55A89919DB3DD368D72DA80B5F4ED85E
                      4D9AB9AAAECC6D7390BEBAB4189D0CF6
                      9CD024355366698CB358EDC20CC43B1D

Embedded Hash   SHA2: 386DBDCEF0738CE96F2706AABA5007BA
                      55A89919DB3DD368D72DA80B5F4ED85E
                      4D9AB9AAAECC6D7390BEBAB4189D0CF6
                      9CD024355366698CB358EDC20CC43B1D

CCO Hash        MD5 : 44632C628D7A70124CE28044A530C6FE
Digital signature successfully verified in file flash0:c2951-universalk9-mz.SPA.157-3.M5.bin
```


### Set Boot

Set the boot image.

```
Router#config t
Router(config)#no boot system
Router(config)#boot system  flash:c2951-universalk9-mz.SPA.157-3.M5.bin
```

Verify with:

```
Router#show run | include boot
boot-start-marker
boot system flash:c2951-universalk9-mz.SPA.157-3.M5.bin
boot-end-marker
```

### Reload

Reload the router.

```
Router#reload
Proceed with reload? [confirm]

*Oct  2 15:55:55.209: %SYS-5-RELOAD: Reload requested by console. Reload Reason: Reload Command.
System Bootstrap, Version 15.0(1r)M16, RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 2012 by cisco Systems, Inc.
```

### Links

https://www.cisco.com/c/en/us/support/docs/routers/3800-series-integrated-services-routers/49044-sw-upgrade-proc-ram.html


### Video

[video](https://www.youtube.com/watch?v=EX5zvzH0cSc){.youtube}



