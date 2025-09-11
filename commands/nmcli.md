# Network Manager CLI (nmcli)

https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_networking/using-networkmanager-to-disable-ipv6-for-a-specific-connection_configuring-and-managing-networking#disabling-ipv6-on-a-connection-using-nmcli_using-networkmanager-to-disable-ipv6-for-a-specific-connection

## Commands

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

### Show Wireless Access Points

```h
nmcli device wifi list

*  SSID               			MODE    CHAN  RATE       SIGNAL  BARS  SECURITY
   NASA							Infra   6     54 Mbit/s  37      ▂▄__  WEP
*  NASA_CORP					Infra   11    54 Mbit/s  98      ▂▄▆█  WPA1
   NASA_GUEST					Infra   1     54 Mbit/s  62      ▂▄▆_  WPA2 802.1X
   ncsc							Infra   6     54 Mbit/s  29      ▂___  WPA1
   ncsc-but-cool				Ad-Hoc  6     54 Mbit/s  22      ▂___  --
   GCHQ DATA COLLECTOR			Infra   1     54 Mbit/s  19      ▂___  WEP
   FREE WIFI (NOT A SCAM)		Infra   1     54 Mbit/s  20      ▂___  WEP
   PREMIUM WIFI (20$ an hour)	Infra   4     54 Mbit/s  32      ▂▄__  WPA2
```
