<!-- TITLE: Unattended Upgrades -->
<!-- SUBTITLE: A quick summary of Unattended Upgrades -->

# Unattended-Upgrades

## Install 
Scripted:
```
wget -O- https://git.enjen.net/munzy/recipes/raw/master/recipes/debian/unattended-upgrades.sh | bash
```

Install:
```
apt install unattended-upgrades
```

## Enable all updates

```
sed '/-updates\|-backports\|Remove-/s#^//##; /Remove-/s#false#true#; /\/\//d; /^$/d; /Black/,/};/d' "/etc/apt/apt.conf.d/50unattended-upgrades" | tee "/etc/apt/apt.conf.d/51unattended-upgrades_on"
```

## Links

https://www.richud.com/wiki/Ubuntu_Enable_Automatic_Updates_Unattended_Upgrades
