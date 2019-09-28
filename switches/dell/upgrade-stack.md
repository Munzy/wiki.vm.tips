<!-- TITLE: Dell Switch Upgrade -->
<!-- SUBTITLE: How to upgrade Dell Switches -->

# Dell Switch Stack Upgrade

Firstly make sure to setup a TFTP server https://wiki.vm.tips/apps/tftp to distribute your packages from as well as a telnet/ssh client.

## download

You can download a copy of the newest dell switch firmware from: https://www.dell.com/support/

## enable

Firstly, login to your switch and get to an enabled prompt.

You can do this via:
```enable```

It should ask for a password.

## boot

Secondly, we need to update the boot rom. Boot roms always end in ```.rfb```.

To do this we run:
```
copy tftp://10.239.245.118/dell/5324.rfb unit://*/boot
```


## image

Thirdly, we need to update the actual image. To do this step is a bit more complex. 

We first need to verify which "slot" is active, and upload to the "not active" slot.

```
console# show bootvar
Unit  Image  Filename   Version    Date                    Status
----  -----  ---------  ---------  ---------------------   -----------
1     1      image-1    4.1.0.12   22-Jul-2013  16:32:43   Active*
1     2      image-2    4.1.0.22   17-May-2018  19:17:44   Not active
2     1      image-1    4.1.0.12   22-Jul-2013  16:32:43   Active*
2     2      image-2    4.1.0.22   17-May-2018  19:17:44   Not active
3     1      image-1    4.1.0.12   22-Jul-2013  16:32:43   Active*
3     2      image-2    4.1.0.22   17-May-2018  19:17:44   Not active
4     1      image-1    4.1.0.12   22-Jul-2013  16:32:43   Active*
4     2      image-2    4.1.0.22   17-May-2018  19:17:44   Not active
```

Upload the firmware:
```
console# copy tftp://10.239.245.118/dell/5324.ros unit://*/image
```

## reload

Now to fully install the dell switch run. Where X is the non active boot image.

```
console# boot system image-x switch-x
console# boot system image-x all
console# reload
```

## Links
https://www.dell.com/community/Networking-General/Upgrading-a-Dell-Sw-Stack-of-4-units-5548/td-p/5828464

