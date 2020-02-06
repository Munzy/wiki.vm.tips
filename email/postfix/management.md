---
title: Postfix Management
description: How to manage postfix email servers.
published: true
date: 2020-02-06T23:18:26.822Z
tags: 
---

# Postfix Management 101

## manage service

Basic service management:
start: ```systemctl start postfix``` -or- ```service postfix start```.
stop: ```systemctl stop postfix``` -or- ```service postfix stop```.
restart: ```systemctl restart postfix``` -or- ```service postfix restart```.
reload: ```systemctl reload postfix``` -or- ```service postfix reload```.

Auto start at boot:
enable: ```systemctl enable postfix```
disable:  ```systemctl disable postfix```



## logging

To access logging, and watch it live... an easy method is: ``` tail -f /var/log/mail.*```.

This will watch all mail related transports.

## mailq

Sometimes you need to see what is held up in the queue. To do this simply run ```mailq```.

## flush

### delete
```
postsuper -d ALL
```

### remove deferred

```
postsuper -d ALL deferred
```

### rerun

```
postfix -f
# or
postfix flush
```
