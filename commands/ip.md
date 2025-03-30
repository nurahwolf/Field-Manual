# IP

#### Dynamically change route

```shell
ip route replace [destination] via [gateway] dev [interface] metric [value]
```

For example:

```
ip route replace default via 10.69.69.69 dev enp0s3 proto dhcp src 10.69.69.254 metric 200
```
