<!-- TITLE: OpenVZ Bulk Commands -->
<!-- SUBTITLE: A quick summary of Bulk Commands -->

# Commands
## start
```
for VE in $(vzlist -Ha -o veid); do vzctl start $VE; done
```

## stop
```
for VE in $(vzlist -Ha -o veid); do vzctl stop $VE; done
```

## restart
```
for VE in $(vzlist -Ha -o veid); do vzctl restart $VE; done
```


## set autoboot
```
for VE in $(vzlist -Ha -o veid); do vzctl set $VE --onboot yes --save; done
```


## disable
```
for VE in $(vzlist -Ha -o veid); do vzctl restart $VE; done
```
