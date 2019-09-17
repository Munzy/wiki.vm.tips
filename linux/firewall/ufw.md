<!-- TITLE: UFW -->
<!-- SUBTITLE: How to setup UFW Firewall -->

# UFW

## Install
```
apt update
apt install ufw
```

## Add Allowed Ports/Services
```
ufw allow ssh
ufw allow 80
ufw allow https
ufw allow 6000:6007/tcp
```

## Enable
```
ufw enable

# or

ufw --force enable
```

## Disable
```
ufw disable
```

## Status
```
ufw status
```

## Delete Rule
```
ufw status numbered
ufw delete <number>
```

## Find Currently Open Ports

```
apt install net-tools -y
netstat -tulpn
```

```
root@docker-1:~# netstat -tulpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:61209           0.0.0.0:*               LISTEN      532/python3
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      575/sshd
tcp6       0      0 :::443                  :::*                    LISTEN      17568/docker-proxy
tcp6       0      0 :::32770                :::*                    LISTEN      4531/docker-proxy
tcp6       0      0 :::32771                :::*                    LISTEN      4545/docker-proxy
tcp6       0      0 :::3310                 :::*                    LISTEN      11552/docker-proxy
tcp6       0      0 :::80                   :::*                    LISTEN      17582/docker-proxy
tcp6       0      0 :::22                   :::*                    LISTEN      575/sshd
udp        0      0 0.0.0.0:161             0.0.0.0:*                           536/snmpd
udp6       0      0 :::161                  :::*                                536/snmpd
```
