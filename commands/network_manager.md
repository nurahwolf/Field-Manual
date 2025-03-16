# Network Manager

https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/using-networkmanager-to-disable-ipv6-for-a-specific-connection_configuring-and-managing-networking#disabling-ipv6-on-a-connection-using-nmcli_using-networkmanager-to-disable-ipv6-for-a-specific-connection

#### Disabling IPv4 and IPv6 per connection

```shell
nmcli connection show
nmcli connection modify $TARGET ipv6.method "disabled"
nmcli connection up $TARGET
```
