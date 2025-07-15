# Pivoting with SSH

Source: https://www.adamcouch.co.uk/networking-pivoting-via-ssh-scanning-with-nessus-behind-a-firewall-or-nat

Create the virtual tap interface with the following command:

# ip tuntap add dev tap0 mod tap

Once this is setup assign it an ip address and raise the interface, assign a different address for each end of the tunnel:

So on the scanner:

# ip a a 10.100.100.100/24 dev tap0 && ip link set tap0 up

On the pivot server:

# ip a a 10.100.100.101/24 dev tap0 && ip link set tap0 up

On each end of the tunnel we will also need to make sure our SSH config will allow us to tunnel. Lets modify our /etc/ssh/sshd_config file by adding ‘ PermitTunnel=yes ‘ to the end and restart the service. More about this option can be found in SSH man page here.

Now for the magic, lets bring the tunnel up by establishing an SSH session, this will need to be done with a privileged account:

ssh -o Tunnel=ethernet -w 0:0 root@11.1.1.11

Lets cover off these options:

    -o = allows us to specify options
    Tunnel=ethernet = is our option for the tunnel
    -w 0:0 = specifies the next available interface for the tunnel, and corresponds to each side of the tunnel.

Next lets take a moment to verify our tunnel is up with a couple of quick checks:

First verify the link is up with ethtool:

# ethtool tap0

You will notice the link is up, try this without the connection you will find the link is down.

Second verify you can ping the other end of the tunnel:

# ping 10.100.100.101

Again disconnect your SSH connection and watch icmp response drop.

Next in order to get our traffic to our destination servers/subnet we are going to need some routes adding to Kali to tell the system where to send the traffic, ie the other end of the tunnel, so, something similar to this where 192.168.1.0/24 being the network you are targeting:

# ip route add 192.168.1.0/24 via 10.100.100.101

# ip route add 192.168.2.0/24 via 10.100.100.101

Finally we need to setup some iptables rules and turn our pivot point into a router by enabling IPv4 forwarding:

# echo 1 > /proc/sys/net/ipv4/ip_forward

# iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# iptables -t nat -A POSTROUTING -o tap0 -j MASQUERADE

# iptables -A INPUT -i eth0 -m state –state RELATED,ESTABLISHED -j ACCEPT

# iptables -A INPUT -i tap0 -m state –state RELATED,ESTABLISHED -j ACCEPT

# iptables -A FORWARD -j ACCEPT

At this point the pivot should be up and running, test this by doing some basic checks with a known host on your target network.

Happy pivoting testers!
