# Proxying and Pivoting

An essential tool often used to access things on different networks, or across security boundaries. It is rather important to understand the differences between the two, so we shall start there.

| Feature                | Network Pivot                          | Network Proxy                          |
|------------------------|----------------------------------------|----------------------------------------|
| **Primary Goal**       | Access internal networks               | Anonymize or bypass restrictions       |
| **Layer**              | Network/Transport (3/4)                | Application (5–7)                      |
| **Configuration**      | Routes on attacker’s machine           | Client-side proxy settings             |
| **Traffic Scope**      | Broad (any protocol)                   | Limited to proxy-supported protocols   |
| **Primary Use Case*    | Lateral movement, internal scanning    | Evasion, anonymity, tunneling          |

In penetration testing in particular, **pivoting** and **proxying** are both techniques used to route traffic through (compromised) systems, but they serve distinct purposes and operate differently. Generally, proxying is usually easier to setup and can be useful for things like getting 'general internet access' into an environment where it is often restricted, useful for updating tools and packages.

However, a full blown pivot or operating at a lower level in the OSI stack is critical to use tools like `nmap` for network / port scanning, especially for protocols such as `UDP` or `ICMP`.

### Purpose
- **Pivot**  
	Pivoting focuses on **extending access** into restricted or deeper parts of a network. Once a system is compromised, pivoting allows attackers to interact with internal resources that are not directly reachable from their own machine.
	Example: Using a compromised host as a "jump point" to scan or exploit other systems on an isolated subnet.

- **Proxy**  
	Proxying focuses on **tunneling traffic** through an intermediary for **anonymity**, **bypassing filters**, **machine in the middle** or **evading detection**. It masks the attacker's origin or circumvents network restrictions such as firewalls. Sometimes the proxy can have wider access than the target, allowing for access to other resources.
  Example: Routing web traffic through a compromised server to hide the attacker's origin IP address, or to mask network traffic.

### Technical Operation
- **Pivot**  
	- Operates at **lower network layers (Layer 3/4)**, enabling full TCP/IP connectivity to internal networks.
	- Involves configuring routing tables or tunnels (e.g., SSH tunnels, Meterpreter's `autoroute`, or `chisel`) to forward traffic to non-routable subnets.
	- Transparent to the attacker's tools; no client-side configuration is needed beyond routing.
	- Supports broad protocols (UDP, ICMP, SMB, etc.) and actions like port scanning or lateral movement.

- **Proxy**  
	- Operates at **higher layers (Layer 5–7)**, handling specific application-layer protocols (HTTP, SOCKS, etc.).  
	- Requires client-side configuration (e.g., setting proxy settings in browsers or tools like `ProxyChains`).  
	- Limited to protocols supported by the proxy (e.g., HTTP proxies may not handle DNS or ICMP).  
	- Common tools include SSH dynamic port forwarding (`-D`), SOCKS proxies, or reverse proxies.
	- Can still be used for basic things like [[Reverse Shells]] and `machine-in-the-middle` style attacks.
	- Burp Suite operates as a proxy, used for intercepting traffic!

### Use Cases
- **Pivoting**  
	- Post-exploitation phase to access internal networks (e.g. moving from a DMZ to an internal corporate network).  
	- Enables reconnaissance (e.g. Nmap scans) and lateral movement (e.g. exploiting vulnerabilities on internal hosts).  

- **Proxying**  
	- Bypassing egress filtering (e.g. tunneling HTTP traffic through an allowed SSH proxy).  
	- Anonymizing attacks (e.g. making requests appear to originate from the proxy host).  
	- Circumventing IP-based restrictions (e.g. accessing a service whitelisted for the proxy's IP).

### Example Scenarios
- **Pivot Examples**  
	- After exploiting a web server (`192.0.2.10/24`) with access to an internal subnet (`198.51.100.0/24`), the attacker adds a route to reach `198.51.100.5/24` (a database server) using Metasploit's `autoroute` or SSH tunnels. They can then directly connect to `198.51.100.5/24:3306` to attack MySQL.

- **Proxy Examples**  
	- An attacker sets up a SOCKS proxy via SSH on a compromised host (`192.0.2.10/24`). Using `ProxyChains`, they route a `curl` request through the proxy to access an internal web application on `198.51.100.2/24`, masking their IP as `192.0.2.10/24`.
	- An attacker has access to a KALI Virtual Machine within a target environment, but internet access is blocked. The attacker can connect from the outside world via `SSH`. They use this SSH connection to tunnel a `SOCKS` proxy, such as with `squid`, allowing them to access the internet (e.g. for package updates) on the KALI host.

### Overlap
Quite often, technical people often use the terms `pivot` and `proxy` interchangably. This is because of some overlap between the two:

- A pivot can act as a proxy (e.g. routing traffic through a compromised host to reach an internal proxy server).
	- The same is true in the reverse direction as well. A proxy could allow traffic to a different subnet, hence acting as a pivot.
- Tools like `chisel` or `meterpreter` combine both concepts, enabling pivoting and acting as proxies.  
- Proxies often rely on pivoting to reach restricted networks, but pivoting alone doesn’t require proxying.

# Proxy

Proxying on linux generally uses environment variables so that software knows where to send things. The main ones are these:

```shell
export http_proxy=http://10.203.0.1:5187/
export https_proxy=$http_proxy
export ftp_proxy=$http_proxy
export rsync_proxy=$http_proxy
export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"
```

A useful bash script for automating proxying (Credit to [the Arch Wiki!](https://wiki.archlinux.org/title/Proxy_server):

```bash
function proxy_on() {
    export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"

    if (( $# > 0 )); then
        valid=$(echo $@ | sed -n 's/\([0-9]\{1,3\}.\?\)\{4\}:\([0-9]\+\)/&/p')
        if [[ $valid != $@ ]]; then
            >&2 echo "Invalid address"
            return 1
        fi
        local proxy=$1
        export http_proxy="$proxy" \
               https_proxy=$proxy \
               ftp_proxy=$proxy \
               rsync_proxy=$proxy
        echo "Proxy environment variable set."
        return 0
    fi

    echo -n "username: "; read username
    if [[ $username != "" ]]; then
        echo -n "password: "
        read -es password
        local pre="$username:$password@"
    fi

    echo -n "server: "; read server
    echo -n "port: "; read port
    local proxy=$pre$server:$port
    export http_proxy="$proxy" \
           https_proxy=$proxy \
           ftp_proxy=$proxy \
           rsync_proxy=$proxy \
           HTTP_PROXY=$proxy \
           HTTPS_PROXY=$proxy \
           FTP_PROXY=$proxy \
           RSYNC_PROXY=$proxy
}

function proxy_off(){
    unset http_proxy https_proxy ftp_proxy rsync_proxy \
          HTTP_PROXY HTTPS_PROXY FTP_PROXY RSYNC_PROXY
    echo -e "Proxy environment variable removed."
}
```

## Fscking SUDO is dropping my goddarn proxy config!?

I have run into this issue so many times (skill issue), as I forget that `SUDO` drops the needed environment variables. This is especially painful if you try to `sudo apt update` on a box, only to lose your mind why the proxy no longer works.

The easiest fix is to tell `SUDO` to keep the environment variables with the following configuration dropin.

```shell
/etc/sudoers.d/05_proxy

Defaults env_keep += "*_proxy *_PROXY"
```

## Proxy Software - HTTP(S) Specific

- [burpsuite](https://portswigger.net/burp) - Proprietary and graphical, though a legend in the MITM proxy space.
- [charles](https://www.charlesproxy.com) — Graphical trialware written in Java.
- [fiddler](https://www.telerik.com/fiddler) — Proprietary and graphical, running on Mono.
- [microsocks](https://github.com/rofl0r/microsocks) — Plain simple SOCKS5 proxy server, written in C.
- [mitmproxy](https://mitmproxy.org) — Command-line and web interface, written in Python, also has an API.
- [sslsplit](https://www.roe.ch/SSLsplit) — Works with any TLS connections but cannot act as a HTTP proxy in a browser, written in C.

## Proxy Software - Web Specific

- [squid](https://www.squid-cache.org) - Squid is a very popular caching/optimizing proxy.
- [privoxy](https://www.privoxy.org) is an anonymizing and ad-blocking proxy.
- [tinyproxy](https://tinyproxy.github.io) is a small, efficient HTTP/SSL proxy daemon.
- For a very simple proxy, ssh with port forwarding (`ssh -D 1337`) can be used.

**TODO:** I often use `squid` for things like getting internet into environments without internet. Time to learn and write up using tinyproxy instead?

## Proxychains

An old classic. Speed, performance and functionality is limited as this is effectively a fancy SOCKS proxy. In particular, `proxychains` allows you to forward (proxy) traffic based on rules you define in `/etc/proxychains.conf`. For example, if you have dynamic port forwarding on a SSH connection you could add:

```shell
cat /etc/proxychains.conf

socks5 127.0.0.1 1337
```

Then you run a tool with `proxychains program` such as `proxychains curl`.

**NOTE:** It's useful to set the `all_proxy` environment variable to ensure that all tools (e.g. `apt`, `pacman`, `curl`, etc) use the proxy by default. An example is: `export all_proxy="socks5://localhost:1337"`.

## Proxy Settings on Gnome
Chromium and Firefox automatically use the proxy settings defined in Gnome. You can modify this in the GUI settings app `gnome-control-center`, or via the CLI with `gsettings`. Enjoy the pain and suffering of typing out the same commands multiple times.

```shell
gsettings set org.gnome.system.proxy mode 'manual' 
gsettings set org.gnome.system.proxy.http host 'localhost'
gsettings set org.gnome.system.proxy.http port 1337
gsettings set org.gnome.system.proxy.ftp host 'localhost'
gsettings set org.gnome.system.proxy.ftp port 1337
gsettings set org.gnome.system.proxy.https host 'localhost'
gsettings set org.gnome.system.proxy.https port 1337
gsettings set org.gnome.system.proxy.socks host 'localhost'
gsettings set org.gnome.system.proxy.socks port 1337
gsettings set org.gnome.system.proxy ignore-hosts "['localhost', '127.0.0.0/8', '10.0.0.0/8', '192.168.0.0/16', '172.16.0.0/12' ]"
```

Its easier with smart shells, though.

```shell
gsettings set org.gnome.system.proxy mode 'manual' 
gsettings set org.gnome.system.proxy.{https,http,ftp} host 'localhost'
gsettings set org.gnome.system.proxy.{https,http,ftp} port 1337
gsettings set org.gnome.system.proxy ignore-hosts "['localhost', '127.0.0.0/8', '10.0.0.0/8', '192.168.0.0/16', '172.16.0.0/12' ]"
```

- **TODO:** KDE options?
- **TODO:** Check shell expansion actually works.

# References
* [Arch Wiki - Proxy Server](https://wiki.archlinux.org/title/Proxy_server)
