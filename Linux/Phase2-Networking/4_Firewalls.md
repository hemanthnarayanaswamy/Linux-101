# Firewalls in Linux

A firewall acts as a gatekeeper, determining which traffic is allowed into or out of your system. This is managed through a set of rules known as firewall rules. When a request or packet of data attempts to enter or leave your system, the firewall compares it against these rules.

![netfilter](https://tekneed.com/wp-content/uploads/2024/09/Netfilter.-Firewall-configuration-in-Linux-678x381.png)

Before diving into firewalls, it’s important to understand `NetFilter`, the underlying framework that manages all network-related operations in Linux. Since firewalls deal with network traffic, they are inherently tied to `NetFilter`.

- On Linux, the actual packet-filtering system lives in the kernel and is called `netfilter`.

```bash
source IP        where it came from
destination IP   where it is going
protocol         TCP, UDP, ICMP, etc.
port             22, 80, 443, 53, etc.
direction        inbound, outbound, forwarded
state            new connection, existing connection, related traffic
```

The rules can be applied to this type of traffics:

1. **INPUT** traffic coming into this Linux machine
2. **OUTPUT** traffic leaving this Linux machine
3. **FORWARD** traffic passing through this Linux machine. The packet is not meant for the Linux machine itself. The Linux machine is just forwarding it.

You usually care about _FORWARD_ when doing things like:

- routing
- NAT
- VPN gateways
- containers
- virtual machines
- home router/firewall setups

**Tools like these are used to manage firewall rules:**

```bash
nftables    modern low-level firewall tool
ufw         beginner-friendly frontend, common on Ubuntu

iptables    older low-level firewall tool
firewalld   zone-based frontend, common on Fedora/RHEL
```

### Stateful Filtering

A Linux firewall can remember connections/connection state. This is called connection tracking or conntrack.

- Without this, the firewall would look at every packet separately and ask: _Should I allow this packet?_
- With stateful firewalling, it can ask a smarter question: _Is this packet part of a connection I already allowed?_

**The Main States are**

- `NEW` starting a new connection
- `ESTABLISHED` part of an already accepted connection
- `RELATED` connected to an existing connection somehow
- `INVALID` broken/weird packet

```txt
Example: you run this on your Linux machine: curl https://example.com

Your machine sends an outbound request
The website replies back
Even though the reply is technically incoming, it should be allowed because your machine started the connection. So firewalls commonly have a rule like: Allow ESTABLISHED,RELATED traffic

NEW = someone is knocking
ESTABLISHED = part of an existing conversation
RELATED = side conversation connected to an allowed one
INVALID = suspicious or malformed
```

Most basic secure firewall setups include:

- Allow Loopback
- Allow established/related
- Allow specific new inbound services, like SSH or HTTP
- Deny everything else inbound

### FILTERING ORDER

Firewall rules are usually checked from top to bottom. A packet enters a firewall chain like _INPUT_, then Linux checks rules in order:

```bash
Rule 1: does this packet match?
Rule 2: does this packet match?
Rule 3: does this packet match?
...
Default policy if nothing matched

Once a packet matches a rule that says ALLOW, DROP, or REJECT, the firewall usually stops checking further rules for that packet.
```

**_SO ORDER MATTERS ALOT_**
First allow the safe/needed traffic. Then block the rest.

- `ALLOW / ACCEPT`: Let the packet through.

```bash
Allow incoming TCP traffic to port 22
```

- `DROP`: Silently discard the packet. Do not tell the sender anything.

```bash
Client tries to connect to blocked port 3306
Firewall drops it
Client waits...
Eventually: connection timed out
```

- `REJECT`: Block the packet and send a response saying no.

```bash
Client tries to connect to blocked port 3306
Firewall rejects it
Client quickly gets: connection refused / unreachable
```

1. _ACCEPT/ALLOW_ = let it pass
2. _DROP_ = silently discard/Ignore
3. _REJECT_ = block and notify sender

- Ports identify services.
- Protocols matter: _TCP_ and _UDP_ are different.
- For incoming server traffic, rules usually match destination ports.

### Interfaces and IP Address

A firewall rule can be more specific:

```bash
Allow SSH only from 192.168.1.50 Block SSH from everyone else
```

An interface is a network device/name, like:

```bash
eth0      common Ethernet name
ens33     common modern Linux VM name
wlan0     Wi-Fi
lo        loopback
docker0   Docker bridge
```

A firewall rule can say

- Allow traffic only if it enters though `eth0`
- Block traffic if it enters through `wlan0`

```bash
eth0 = internal private network
eth1 = public internet

Allow tcp/5432 on eth0
Block tcp/5432 on eth1

So your database is reachable inside your private network, but not from the public internet.
```

### Loopback Interface `lo`

The loopback interface is the machine talking to itself.

```bash
It usually uses this IP: 127.0.0.1
Hostname: localhost

lo interface
```

Many programs on Linux communicate internally using localhost.

- A web app connects to a local database
- A monitoring tool checks a local service
- A browser connects to a local development server
- system services talk to each other

That traffic does not go out to the internet. It stays inside your own machine through the `lo` interface.

> Most firewall setups include a rule like: `Allow all traffic on lo` Because blocking loopback can break local programs in confusing ways.

- App server runs on `localhost:8000`
- Database runs on `localhost:5432`
- If _Firewall blocks lo_, Result: _app cannot talk to database, even though both are on the same machine_

## Firewall Tools

1. [UFW](../commands/firewall/ufw.md) `(uncomplicated firewall)` is a command-line tool designed to simplify firewall management on Linux systems, particularly those based on Ubuntu. Built on top of `iptables`, it provides a user-friendly way to define rules for controlling network traffic, such as allowing or blocking specific ports, IP addresses, or services.

2. [nftables](https://medium.com/@ihouelecaurcy/the-complete-nftables-guide-modern-linux-firewall-mastery-79fb86894d5c) -> TODO

### Common DevOps Mistake

1. Enabling Firewal before allowing SSH

```bash
sudo ufw allow 22/tcp
sudo ufw enable
```

2. Opening database ports publicly

```bash
sudo ufw allow from 10.0.1.0/24 to any port 5432 proto tcp
```

3. Forgetting UDP
   TCP and UDP are separate.

```bash
sudo ufw allow 53/tcp
sudo ufw allow 53/udp
```

### Troubleshooting Firewall Issues

1. Check listening service. `ss -tulnp`
2. Check UFW `sudo ufw status verbose`
3. Check nftables `sudo nft list ruleset`
4. Test TCP ports `nc -vz <host> <port>`
5. Capture Packets `nc -vz <host> <port>`
