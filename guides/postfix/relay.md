<!-- TITLE: Relay -->
<!-- SUBTITLE: Setting up a simple postfix relay -->

# Relay

Postfix is a powerful email MTA, mail transfer agent. It is also a great way to collect your emails in a local, or remote location and dump them to a email server. Especially when you don't want your email credentials hanging around!

## Prerequisits
Please run one of the following operating systems.
* Debian 9/10 +
* Ubuntu 16.04 LTS / 18.04 LTS +

``` 
apt update 
apt dist-upgrade -y
apt install postfix -y
```

Follow the setup guide, and select Internet with Smart Host.
