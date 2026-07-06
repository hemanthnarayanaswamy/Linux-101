# Introduction to Networking

1. Refer to the [OSI Model](https://github.com/hemanthnarayanaswamy/System-Design-Bootcamp/blob/main/notes/Networking/osiModel.md)
2. [Introduction to IP Address](https://github.com/hemanthnarayanaswamy/System-Design-Bootcamp/blob/main/notes/Networking/ipAddress.md)

## Networking basics: Important terms and concepts

Before we get into more complex networking details, we need to take a second and learn some basic networking terms and concepts:

1. **Node**: A node is the term used to describe any device that can send, receive, or forward information on a network. This could be a computer, a mobile phone, a printer, a switch, or a router

2. **Network Interface Card (NIC)**: Each node has a NIC, which creates a physical connection to the network. It also has a MAC address, which is a unique identifier

3. **MAC Address**: This 'Media Access Control' address is a unique identifier assigned to a NIC by its manufacturer. It's like your device's postal address on the network

4. **IP Address**: This is another unique identifier, but assigned by the network according to its own rules. Think of it as a temporary P.O. Box number that can change

5. **Router**: This hardware device routes data from one network to another. Picture it as a traffic officer, directing packets of data along the network to prevent congestion and ensure data gets to the right place

6. **Switch**: Connects devices within the same network and manages internal data communication. Connects computers, printers, and servers

7. **Packet**: Information sent over a network is broken into smaller pieces called packets. These are like the individual letters that make up a word or the words that make up a page

8. **Bandwidth**: This reflects the maximum amount of data that can be sent over a network connection in a given time. It can be likened to the width of a highway: a wider highway can accommodate more cars (But cars still need to be the same width and size)

9. **Protocol**: These are the set of rules that dictate how data is transferred on a network. Picture it as conversational etiquette that all devices on the network must adhere to, much like traffic on a highway

10. **Ethernet**: This is the most common protocol for wired Local Area Networks (LANs). If you've seen a cable connecting a computer to the internet, you've seen Ethernet at work

11. **Wi-Fi**: This is a protocol for wireless networking, where devices connect to a network through a Wi-Fi router

12. **TCP/IP**: The Transmission Control Protocol/Internet Protocol is the fundamental protocol that governs data transfer over the Internet

13. **Firewall**: This is a network security system that monitors and controls incoming and outgoing network traffic, akin to a security guard checking who enters and leaves a building

14. **VPN**: A Virtual Private Network extends a private network over a public one, like the Internet. This allows users to send and receive data as if their devices were directly connected to the private network

15. **Network Topology**: This refers to how various elements (nodes, links, etc.) are arranged in a network. This structure determines how information is transferred across the network

16. **ISP**: Your Internet Service Provider is the company that provides your Internet access

17. **Gateway**: Connects two different networks that user different protocols. Translates data between different systems. Enables communication between dissimilar networks. Commonly used to connect private networks to external networks.

18. **Access Point [AP]**: Provides wireless connectivity to devices in a network. Extends a wired network into Wi-Fi.

![basic](https://media.geeksforgeeks.org/wp-content/uploads/20250726184452563578/frame_25.webp)

19. **Network Interfaces**: A network interface is a point of interaction between a device (like a computer) and a network. In Linux, network interfaces can be physical (e.g., Ethernet cards) or virtual (e.g., loopback interface).

## Types of Computer Network Architecture

Computer Network falls under these broad Categories:

1. **Client-Server Architecture**: Represents a type of computer network architecture in which nodes function as servers or clients, where the server manages client behavior, known as Client-Server Architecture.
2. **Peer-to-Peer Architecture**: Operates without any central server, allowing each device to act as either a client or a server, known as P2P (Peer-to-Peer) Architecture.

In Linux:

```ini
Application Layer     HTTP, SSH, DNS, NTP, DHCP, Kubernetes API
Transport Layer       TCP, UDP
Network Layer         IPv4, IPv6
Data Link Layer       Ethernet, Wi-Fi, VLAN
Physical Layer        Cable, radio, fiber
```

## TCP(Transmission Control Protocol) vs UDP(User Datagram Protocol)

TCP and UDP are both transport layer protocols. They sit above IP and below application protocols.

- Both TCP and UDP use: `IP Address + Port Number`

```
192.168.1.10:22
10.0.0.5:53
172.16.1.20:443
```

The **IP address** identifies the machine or interface. The **port number** identifies the application or service on that machine.

- `TCP` is used when applications need reliable, ordered, connection-based communication.

```bash
SSH
HTTP/HTTPS
MySQL/PostgreSQL
SMTP
FTP
Kubernetes API server
Docker registry
Git over HTTPS/SSH
```

> Refer for detailed [TCP](../concepts/networking/tcp.md) details and analysis.

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

> Refer for detailed [UDP](../concepts/networking/udp.md) refer here.

## IP Addresses

[Introduction to IP Address](https://github.com/hemanthnarayanaswamy/System-Design-Bootcamp/blob/main/notes/Networking/ipAddress.md)

**_Private IPs_** are used inside internal networks. They are not directly routable on the public internet.

| Range                         | CIDR           | Common Use                            |
| ----------------------------- | -------------- | ------------------------------------- |
| 10.0.0.0 - 10.255.255.255     | 10.0.0.0/8     | Cloud/VPC, enterprise networks        |
| 172.16.0.0 - 172.31.255.255   | 172.16.0.0/12  | Docker, Kubernetes, internal networks |
| 192.168.0.0 - 192.168.255.255 | 192.168.0.0/16 | Home/lab networks                     |

**_LOOPBACK ADDRESS_**, Loopback means the machine talks to itself.

```bash
127.0.0.1
localhost #hostname

127.0.0.1/8 # loopback range
```

## Ports & Ephemeral in Linux

A _port_ identifies a specific application/service on a machine.

In Linux, the term _ephemeral_ primarily refers to **ephemeral ports**, which are temporary, dynamic ports (typically 32768–60999) assigned by the operating system to client applications for outbound connections.

In essence an ephemeral port is a random high port used to communicate with a known server port. For example, if I ssh from my machine to a server the connection would look like:

```bash
192.168.1.102:37852 ---> 192.168.1.105:22

# 22 is the standard SSH port I'm connecting to on the remote machine; 37852 is the ephemeral port used on my local machine

Client: 192.168.1.10:51544  --->  Server: 10.0.0.5:443
# 51544 = ephemeral port
# 443   = HTTPS server port
```

> The server listens on a fixed port like 443. The client uses a temporary random port.

| Range         | Name                    | Usage                            |
| ------------- | ----------------------- | -------------------------------- |
| 0 - 1023      | Well-known ports        | Standard system services         |
| 1024 - 49151  | Registered ports        | Applications and vendor services |
| 49152 - 65535 | Ephemeral/dynamic ports | Temporary client-side ports      |

- Ports below `1024` are called **well-known** ports. On Linux, binding to ports below 1024 usually requires root privileges or special capability.

## Check Port Mapping File

Linux stores common service-to-port mappings in: `/etc/services`

```bash
grep -E '^(ssh|domain|http|https|ntp|smtp)' /etc/services

ssh       22/tcp
domain    53/tcp
domain    53/udp
http      80/tcp
https     443/tcp
ntp       123/udp
```

- Ports identify applications/services running on a host. Well-known ports are from 0 to 1023 and are used by standard services like SSH 22, DNS 53, HTTP 80, and HTTPS 443.

```bash
0 - 1023        well-known ports
1024 - 49151    registered ports
49152 - 65535   ephemeral/dynamic ports

22              SSH
53              DNS
80              HTTP
123             NTP
443             HTTPS
514             Syslog
```
