# Linux `ip` Command

The [ip](https://linuxize.com/post/linux-ip-command/) command is a powerful tool for configuring network interfaces that any Linux system administrator should know. It is used to bring interfaces up or down, assign and remove addresses and routes, manage ARP cache, and much more.

```bash
ip [OPTIONS] OBJECT { COMMAND | help }

# OBJECT is the object type you want to manage. The m;ost frequently used objects are:

ip OBJECT help
```

- **link (l)** — Display and modify network interfaces.
- **address (a)** — Display and modify IP addresses.
- **route (r)** — Display and alter the routing table.
- **neigh (n)** — Display and manipulate neighbor objects (ARP table).

**_The configurations set with the ip command are not persistent. After a system restart, all changes are lost. To make changes permanent, you need to edit the distribution-specific network configuration files or add the commands to a startup script._**

## Displaying and Modifying IP Addresses

```bash
ip addr [COMMAND] ADDRESS dev IFNAME
```

The most frequently used commands of the addr object are `show`, `add`, and `del`

#### 1. Display Information about ALL IP Addresses

```bash
ip addr show

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:8c:62:44 brd ff:ff:ff:ff:ff:ff
    inet 192.168.121.241/24 brd 192.168.121.255 scope global dynamic eth0
       valid_lft 2900sec preferred_lft 2900sec
    inet6 fe80::5054:ff:fe8c:6244/64 scope link
       valid_lft forever preferred_lft forever

# Display INformation about a specific network interface
ip addr show eth0
```

- `eth0`: Interface Name
- `UP`: Interface is enabled
- `LOWER_UP`: Physical/network link is detected
- `mtu 1500`: maximum packet size
- `inet 192.168.1.10/24`: IPv4 address with CIDR
- `brd 192.168.1.255`: Broadcast address
- `scope global`: Usable beyond this machine.
- `inet6`: IPv6 address

- We can temporary add or remove the ip address

```bash
sudo ip address add 192.168.121.241/24 dev eth0
sudo ip address add 192.168.121.45/24 dev eth0

2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    inet 192.168.121.241/24 brd 192.168.121.255 scope global dynamic eth0
       valid_lft 3515sec preferred_lft 3515sec
    inet 192.168.121.45/24 scope global secondary eth0
       valid_lft forever preferred_lft forever

sudo ip addr del 192.168.1.50/24 dev eth0
```

> These changes are temporary and disappear after reboot unless configured in Netplan, NetworkManager, systemd-networkd, etc.

#### 2. Displaying and Modifying Network Interfaces

To manage and view the state of network interfaces, use the link object. The most commonly used commands are `show`, `set`, `add`, and `del`.

In Linux, a **_network interface_** is a software representation of a physical or virtual network device that enables the system to send and receive data over a network. These interfaces are assigned IP addresses and managed through configuration files or command-line tools depending on the distribution.

```bash
ip link show

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 52:54:00:8c:62:44 brd ff:ff:ff:ff:ff:ff

ip link show dev eth0
```

- `eth0`: interface name
- `UP`: interface administratively enabled
- `LOWER_UP`: carrier/link is detected or actual link/carrier is detected
- `mtu 1500`: maximum transmission unit
- `state UP`: interface state
- `link/ether`: MAC address
- `08:00:27:aa:bb:cc`: interface MAC

###### Bring an Interface UP or DOWN

```bash
ip link set dev DEVICE { up | down }

sudo ip link set eth0 up
sudo ip link set eth0 down
```

###### Change MTU temporarily

```bash
sudo ip link set eth0 mtu 1400
```

_If you see UP but not LOWER_UP, the interface may be enabled but not physically/network connected._

To view packet and error statistics for an interface, use the `-s` flag:

```bash
ip -s link show dev eth0

2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 52:54:00:8c:62:44 brd ff:ff:ff:ff:ff:ff
    RX:  bytes packets errors dropped  missed   mcast
    1923498    8438      0       0       0     204
    TX:  bytes packets errors dropped carrier collsns
     842103    5565      0       0       0       0
```

#### 3. Displaying and Altering the Routing Table

To _assign, remove, and display_ kernel routing table entries, use the `route` object. Used to view or manage routing table.

> Routing decides where packets go.

```bash
# To list all kernel route entries
ip route list

default via 192.168.1.1 dev eth0 proto dhcp src 192.168.121.241 metric 100
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.10
192.168.121.1 dev eth0 proto dhcp scope link src 192.168.121.241 metric 100
```

- `default via 192.168.1.1 dev eth0`: Packets to unknown networks go through gateway `192.168.1.1` using interface _eth0_
- `192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.10` The local machine is directly connected to network `192.168.1.0/24` through _eth0_.

###### Check Route to Specific IP

To display routing for a specific networks:

```bash
ip route get 8.8.8.8

8.8.8.8 via 192.168.1.1 dev eth0 src 192.168.1.10
# Destination: 8.8.8.8
# Gateway: 192.168.1.1
# Interface: eth0
# Source IP: 192.168.1.10
```

###### Add a New Temporary Route

To add a route to

```bash
sudo ip route add 10.10.0.0/16 via 192.168.1.1 dev eth0

# Delete route temporarily
sudo ip route del 10.10.0.0/16

# Add default gateway temporarily
sudo ip route add default via 192.168.1.1
```

- use `ip route` when debugging:

```bash
Can this server reach the internet?
Which gateway is used?
Which interface sends traffic?
Is the default route missing?
Is traffic going through the wrong NIC?

ip route get <destination ip>
ip route get 10.0.5.20
```

#### 4. Displaying and Manipulating the ARP Table

The neigh object manages the neighbor **(ARP)** table, which _maps IP addresses to MAC addresses on the local network_.

`Address Resolution Protocol (ARP)` in Linux is a communication protocol used to map IPv4 addresses to physical Media Access Control (MAC) addresses on local networks or local area network _LAN_, operating at the link layer of the OSI model. It functions by maintaining a neighbor cache (or ARP table) that stores these mappings to reduce network latency and avoid redundant broadcast requests. \* When a device needs to send data to a known IP address but lacks the corresponding MAC address, it broadcasts an ARP request to all devices on the local network. The device holding that IP address responds with a unicast ARP reply containing its MAC address. This mapping is stored in the device's ARP cache to avoid repeated broadcasts for subsequent communications.

To display the current ARP table:

```bash
ip neigh show

192.168.1.1 dev eth0 lladdr aa:bb:cc:dd:ee:ff REACHABLE
192.168.1.25 dev eth0 lladdr 08:00:27:11:22:33 STALE

# 192.168.1.1              neighbor IP
# dev eth0                 interface
# lladdr aa:bb:cc:dd:ee:ff MAC address
# REACHABLE                recently confirmed reachable
# STALE                    known, but not recently confirmed
```

Some common states include:

| State      | Meaning                                |
| ---------- | -------------------------------------- |
| REACHABLE  | Recently reachable                     |
| STALE      | Entry exists but not recently verified |
| DELAY      | Waiting before probing                 |
| PROBE      | Actively checking reachability         |
| FAILED     | Neighbor resolution failed             |
| INCOMPLETE | ARP request sent, no reply yet         |

To add a static ARP entry:
`sudo ip neigh add 192.168.121.50 lladdr 00:11:22:33:44:55 dev eth0`

To delete an ARP entry:
`sudo ip neigh del 192.168.121.50 dev eth0`

To flush all ARP entries for an interface:
`sudo ip neigh flush dev eth0`
