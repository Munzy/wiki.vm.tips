<!-- TITLE: VMWare Tools -->
<!-- SUBTITLE: A quick summary of Vmware Tools -->

# VMWare Tools
## Official


## Open Source
The open source vm tools allows for easier installation and upgrades than the official source. For linux operating systems this is recommended by VMWare.

### Debian / Ubuntu 

```
apt install open-vm-tools -y
systemctl enable open-vm-tools
systemctl start open-vm-tools
```

### Centos / Fedora 

```
yum install open-vm-tools 
systemctl enable open-vm-tools
systemctl start open-vm-tools
```
