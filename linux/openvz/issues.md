<!-- TITLE: Issues -->
<!-- SUBTITLE: A quick summary of Issues -->

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
