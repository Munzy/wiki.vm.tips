<!-- TITLE: UFW -->
<!-- SUBTITLE: How to setup UFW Firewall -->

# UFW

## Install
```
apt update
apt isntall ufw
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

ufw enable --force
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
ufw status
ufw delete RULE|NUM
```