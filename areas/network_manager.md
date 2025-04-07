# Network Manager

### Add macvtap

```shell
nmcli connection add type macvlan dev eth0 mode bridge tap yes ifname macvtap0
```
