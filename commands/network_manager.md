# Network Manager

https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/using-networkmanager-to-disable-ipv6-for-a-specific-connection_configuring-and-managing-networking#disabling-ipv6-on-a-connection-using-nmcli_using-networkmanager-to-disable-ipv6-for-a-specific-connection

#### Change Route Metric

```shell
nmcli connection show
nmcli connection modify $TARGET ipv4.route-metric 69
nmcli connection modify $TARGET ipv6.route-metric 69
```

#### Create macvtap

```shell
nmcli connection show
nmcli connection add type macvlan dev $TARGET mode bridge tap yes ifname macvtap
```

#### Disabling IPv4 and IPv6 per connection

```shell
nmcli connection show
nmcli connection modify $TARGET ipv4.method "disabled"
nmcli connection modify $TARGET ipv6.method "disabled"
nmcli connection up $TARGET
```
