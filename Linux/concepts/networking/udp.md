# User Datagram Protocol

- `UDP` is used when applications prefer speed and simplicity over built-in reliability.
  - its a connectionless transport protocol.
  - UDP sends packets without creating a connection first.

```bash
DNS
DHCP
NTP
VoIP
Video streaming
Online gaming
QUIC/HTTP/3
Syslog
SNMP
WireGuard VPN
Kubernetes DNS
```

## UDP in Linux

Show UDP listening sockets. [ss](../../commands/network-interfaces/ss.md)

```bash
ss -ulnp

UNCONN 0 0 127.0.0.53:53     0.0.0.0:* users:(("systemd-resolve",pid=700))
UNCONN 0 0 0.0.0.0:123       0.0.0.0:* users:(("chronyd",pid=820))

# systemd-resolved is listening on UDP port 53
# chronyd is listening on UDP port 123
```

`UNCONN` That means unconnected, because UDP has no real connection state like TCP.

## Important UDP Linux Commands

1. Show UDP listeners

```bash
ss -ulnp
```

2. Show all UDP sockets

```bash
ss -uan
```

3. Show TCP & UDP listeners together

```bash
ss -tulnp
```

4. Capture UDP traffic

```bash
sudo tcpdump -i any -nn 'udp'
```

5. Capture DNS UDP traffic

```bash
sudo tcpdump -i any -nn 'udp port 53
```

6. check UDP statistics

```bash
netstat -su

cat /proc/net/snmp | grep -A1 Udp
```

7. Check firewall rules

```bash
sudo nft list ruleset
```

## UDP Example

#### DNS

DNS usually uses UDP port 53

```bash
dig google.com
# you linux machine sends a udp packet to a DNS server

# Your machine: random UDP port 45000
# DNS server:   UDP port 53

Client 45000  ---- query ---->  DNS Server 53
Client 45000  <--- reply ----   DNS Server 53

# Capture DNS packets
sudo tcpdump -i any -nn 'udp port 53'

client-ip.random-port > dns-server.53
dns-server.53 > client-ip.random-port
```

> DNS can also user TCP `TCP/53`

```bash
# In situations like this DNS use TCP/53 over UDP/53

DNS response is large
DNSSEC is involved
Zone transfer is happening
UDP response is truncated

# To force DNS over TCP
dig +tcp google.com
```

#### DNCP

`DHCP (Dynamic Host Configuration Protocol)` in Linux automates the assignment of IP addresses and network parameters to devices, eliminating manual configuration.

DHCP uses UDP because the client may not have an IP address yet.

```bash
Client 0.0.0.0:68  ---- DHCP Discover ---->  Server/Broadcast:67
Server:67          ---- DHCP Offer ------->  Client:68
Client:68          ---- DHCP Request ----->  Server:67
Server:67          ---- DHCP ACK --------->  Client:68

## Check DHCP client logs
journalctl -u NetworkManager
journalctl | grep -i dhcp

# Capture DHCP
sudo tcpdump -i any -nn 'udp port 67 or udp port 68'
```

## UDP and Firewalls

UDP and TCP firewall rules are separate. **_Opening TCP port 53 does not open UDP port 53._**

```bash
sudo ufw allow 53/udp
sudo ufw allow 123/udp
```

> TCP and UDP use separate protocol numbers. Same port number does not mean same firewall rule.

## UDP Troubleshooting Flow

Suppose DNS is failing.

1. Check resolver config

```bash
cat /etc/resolv.conf
nameserver <ip-address>
```

2. Test DNS server directly

```bash
dig @8.8.8.8 example.com # external DNS
dig @10.0.0.10 example.com # Internal DNS
```

3. Check Local UDP Listener

```bash
# If this server should be running DNS:
ss -ulnp | grep ':53'
```

4. Check Firewall

```bash
sudo nft list ruleset
```

5. Capture packets

```bash
# On client side
sudo tcpdump -i any -nn 'udp port 53'

# run
dig example.com
```

| tcpdump result          | Meaning                                        |
| ----------------------- | ---------------------------------------------- |
| Query leaves, no reply  | DNS server/firewall/network issue              |
| No query leaves         | Client resolver/local issue                    |
| Query and reply visible | Network path works; check application/resolver |
| ICMP port unreachable   | Service likely not listening                   |

## Common UDP Problems

#### 1. No Response

Most common UDP issue.

```bash
Firewall dropped packet
Server not listening
Wrong destination IP
Wrong port
NAT issue
Packet loss
Application ignored request
```

Unlike TCP, UDP may not give a clear error.

#### 2. UDP blocked but TCP works

```bash
dig +tcp example.com # works
dig example.com # fails

UDP/53 blocked
Firewall allows TCP/53 only
Large DNS response issue
MTU/network issue
```

#### 3. UDP Receive buffer drops

High-volume UDP apps can drop packets if Linux receive buffers fill.

```bash
netstat -su

# packet receive errors
# receive buffer errors

DNS servers
Syslog servers
Telemetry collectors
VPN gateways
Monitoring agents
```

###### Why DNS uses UDP ?

_DNS usually uses UDP because DNS queries and responses are small, fast, and request-response based. UDP avoids TCP connection setup overhead. However, DNS can also use TCP on port 53 for large responses, `DNSSEC`, truncated responses, and zone transfers._

###### TCP vs UDP

- TCP is connection-oriented and reliable. It uses a three-way handshake, acknowledgments, sequence numbers, retransmissions, flow control, and congestion control. It is used by SSH, HTTP, HTTPS, and databases.
- UDP is connectionless and does not guarantee delivery or ordering. It has lower overhead and is used by DNS, DHCP, NTP, syslog, WireGuard, and QUIC. In Linux, TCP sockets show states like LISTEN and ESTABLISHED, while UDP sockets usually show UNCONN.
