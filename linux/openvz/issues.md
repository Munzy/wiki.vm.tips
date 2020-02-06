---
title: Issues
description: A quick summary of Issues
published: true
date: 2020-02-06T23:18:38.522Z
tags: 
---

# Issues

## OpenVZ 7 Factory Repo
The factory repo is a devel repo for OpenVZ.

If the repo is installed it can cause issues.

To see if you have the repo run:
```
yum repolist | grep factory
```

If you have factory return disable it via:
```
 yum-config-manager --disable factory
 ```
 
 https://forum.openvz.org/index.php?t=rview&th=13492&goto=53506
