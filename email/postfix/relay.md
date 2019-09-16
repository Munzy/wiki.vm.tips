<!-- TITLE: Postfix Relay -->
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

## Information we will need.

* Server we are wanting to relay our messages to. A google search can usually find this.
* Server port.
* Username.
* Password.
* Limits.

## main.cf

Append the following to your main.cf in /etc/postfix/main.cf. You can do this via the command ```nano /etc/postfix/main.cf```. If it complains that it isn't installed... run ```apt install nano -y```.

```
# Relay, and login settings.
relayhost = secure.emailsrvr.com
smtp_sasl_auth_enable=yes
smtp_sasl_password_maps=hash:/etc/postfix/sasl_password
smtp_sasl_mechanism_filter = AUTH LOGIN
smtp_sasl_security_options =

# Sending Delays.
smtp_destination_concurrency_limit = 1
smtp_destination_rate_delay = 5s
smtp_extra_recipient_limit = 1

# Header, and Sender Remapping.
sender_canonical_classes = envelope_sender, header_sender
sender_canonical_maps =  regexp:/etc/postfix/generic
smtp_header_checks = regexp:/etc/postfix/header_check

# Defer emails, and reload postfix.
#defer_transports = hold
#default_transport = hold

# Max Message Size that postfix will handle. 50MB.
message_size_limit = 52428800
```


After pasting this in, verify that no other relayhost is set, and peruse the config file for any issues.

## sasl_password
The sasl_password file is what allows us to authenticate to our relay of choice. Assuming we don't have it to automatically accept any email from our ip address. In general it is best practice to have authentication done anyways, as IPs do change.

Edit the file via the command ```nano /etc/postfix/sasl_password```.

Add the following with the email address you wish to send as.

```
secure.emailsrvr.com noreply@example.com:<your password>
```

After which, and during any edits run: 
```
chmod 600 /etc/postfix/sasl_password
postmap /etc/postfix/sasl_password
```
## generic

Many Email providers will only accept your email address. So any email from another address will be dropped. This generic file is one of those steps to create that.

Edit the file via the command ```nano /etc/postfix/generic```.

Add the following with the email address you wish to send as.

```
/.+/    noreply@example.com
```

After which, and during any edits run: 
```
postmap /etc/postfix/generic
```

## header_check

Many Email providers will only accept your email address. So any email from another address will be dropped. This header_check file is another of those steps to create that. Both shold be done.

Edit the file via the command ```nano /etc/postfix/header_check```.

Add the following with the email address you wish to send as.

```
/From:.*/ REPLACE From: noreply@example.com
```

After which, and during any edits run: 
```
postmap /etc/postfix/header_check
```


## allowed_networks

Let's say that you wish to allow certain IPs to be able to be forwarded by your relay to the destination. To do this... we need to allow them first.

Let's start by editing ```nano /etc/postfix/main.cf``` and finding the line ```mynetworks = ```. Edit this line to show ```mynetworks = cidr:/etc/postfix/allowed_networks```

Next create a new file by ```nano /etc/postfix/allowed_networks```.

Edit the file accordingly and add notes for each IP you wish to add like below:
```
# Local Address
127.0.0.0/8             OK
::1                     OK

#################################################
#               Monitors                        #
#################################################

# Watchdog.
10.0.0.1               OK


#################################################
#               Dracs                           #
#################################################

```
 
 
 Make sure to have the local addresses if you wish to send from your local machine.
 
 
 After which, and during any edits run: 
```
postmap /etc/postfix/allowed_networks
```
 
 ## restart/reload postfix
 
 After you make any change make sure to run ```systemctl restart postfix``` -or- ```systemctl reload postfix```
 
 
