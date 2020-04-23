---
title: Wipe Hard Drive
description: 
published: true
date: 2020-04-23T04:44:23.786Z
tags: wipe, hard, drive
---

# Wipe Hard Drive

```
#!/bin/bash 
for n in `seq 7`; do dd if=/dev/urandom of=/dev/sda bs=8b conv=notrunc; done

```