<!-- TITLE: Create Bond on Brocade -->
<!-- SUBTITLE: A quick summary of Create Bond -->

# Bonding

## Create

```
config t
lag <name> dynamic id <id#>
	ports ethernet 1/3/4 ethernet 2/3/4
	primary-port 1/3/4
	deploy
```

