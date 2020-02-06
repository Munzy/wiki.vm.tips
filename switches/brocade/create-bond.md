---
title: Create Bond on Brocade
description: A quick summary of Create Bond
published: true
date: 2020-02-06T23:18:42.446Z
tags: 
---

# Bonding

## Create

```
config t
lag <name> dynamic id <id#>
	ports ethernet 1/3/4 ethernet 2/3/4
	primary-port 1/3/4
	deploy
```

