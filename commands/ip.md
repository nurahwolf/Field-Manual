# IP

NOTE: This guide follows RFC-5737:

- 192.0.2.0/24		(TEST-NET-1)
- 198.51.100.0/24	(TEST-NET-2)
- 203.0.113.0/24	(TEST-NET-3)

## Commands

### Show IP addresses

```bash
ip a # Detailed Information
ip -br a # Show just the IPs assigned to an interface

# Is IPv6 disabled?
ip -br a | grep fe80
sysctl -a 2>/dev/null | grep -i disable_ipv6

ifconfig # Used by legacy systems
networkctl # systemd-networkd
nmcli -p device show # network-manager
```

### Dynamically change route

```bash
ip route replace [destination] via [gateway] dev [interface] metric [value]
```

For example:

```bash
ip route replace default via 192.0.2.20 dev enp0s3 metric 200
```

### Add/Remove Static IP

```bash
ip addr add 192.0.2.0/24 dev enp5s0
ip addr delete 192.0.2.0/24 dev enp5s0
```

### Add/Remove Default Gateway

```bash
ip route add default via 192.0.2.0/24 dev enp5s0
ip route add default gw 192.0.2.254
```

### Add/Remove Static Route

```bash
ip route add 198.51.100.0/24 via 192.0.2.254 dev enp5s0
ip route delete 198.51.100.0/24
```
