# Phase 2 - Networking

This is Phase 3 (Networking) of the roadmap, kept as Phase 2 in this repo because the roadmap's Phase 2 (Shell Scripting & Automation) was skipped.

Primary roadmap sources:

- [linux-mastery-roadmap.md](../../resources/roadMap/linux-mastery-roadmap.md)
- [linux-mastery-roadmap-detailed.md](../../resources/roadMap/linux-mastery-roadmap-detailed.md)
- [networking-roadmap.md](../../resources/roadMap/networking-roadmap.md)

## Quick Index

| Order | Topic | Main Note |
|---:|:------|:----------|
| 1 | Introduction to networking | [1_intro_networking.md](./1_intro_networking.md) |
| 2 | Linux networking tools and fundamentals | [2._Linux_Networking_Fundmentals.md](./2._Linux_Networking_Fundmentals.md) |
| 3 | DNS and name resolution | [3_DNS_and_NameResolution.md](./3_DNS_and_NameResolution.md) |
| 4 | Firewalls | [4_Firewalls.md](./4_Firewalls.md) |
| 5 | SSH and remote access | [5_ssh_remote_access.md](./5_ssh_remote_access.md) |

## Reference Indexes

| Area | Index |
|:-----|:------|
| Commands | [Linux commands index](../commands/README.md) |
| Concepts | [Linux concepts index](../concepts/README.md) |

---

## 1. Introduction To Networking

Build the mental model: OSI layers, TCP/IP, IPv4 addressing and CIDR, ports, and the TCP three-way handshake.

Main note:

- [1_intro_networking.md](./1_intro_networking.md)

Concepts:

- [OSI model](../concepts/networking/osi_model.md)
- [CIDR and subnetting](../concepts/networking/cidr_subnetting.md)
- [TCP](../concepts/networking/tcp.md)
- [UDP](../concepts/networking/udp.md)

Commands:

- [`ip`](../commands/network-interfaces/ip.md)
- [`traceroute`](../commands/network-diagnostics/traceroute.md)
- [`mtr`](../commands/network-diagnostics/mtr.md)
- [`nc`](../commands/network-diagnostics/nc.md)

---

## 2. Linux Networking Tools And Fundamentals

Replace the deprecated tools (`ifconfig`, `netstat`, `route`) with the modern `ip`/`ss` stack, and configure interfaces and routes.

Main note:

- [2._Linux_Networking_Fundmentals.md](./2._Linux_Networking_Fundmentals.md)

Commands:

- [`ip`](../commands/network-interfaces/ip.md)
- [`ss`](../commands/network-interfaces/ss.md)
- [`ping`](../commands/network-diagnostics/ping.md)
- [`mtr`](../commands/network-diagnostics/mtr.md)
- [`tcpdump`](../commands/network-diagnostics/tcpdump.md)
- [`nmcli`, netplan](../commands/network-interfaces/nmcli_netplan.md)

---

## 3. DNS And Name Resolution

Understand how a hostname becomes an IP, the resolution order, record types, and how to diagnose DNS issues.

Main note:

- [3_DNS_and_NameResolution.md](./3_DNS_and_NameResolution.md)

Concepts:

- [UDP](../concepts/networking/udp.md)

Commands:

- [`dig`, `nslookup`, `host`](../commands/dns/dig_nslookup_host.md)
- [`nmap`](../commands/network-diagnostics/nmap.md)
- [`tcpdump`](../commands/network-diagnostics/tcpdump.md)

---

## 4. Firewalls

Learn stateful filtering, default-deny posture, and the NetFilter framework, then write firewall rules that survive reboots.

Main note:

- [4_Firewalls.md](./4_Firewalls.md)

Concepts:

- [TCP](../concepts/networking/tcp.md)
- [UDP](../concepts/networking/udp.md)

Commands:

- [`ufw`](../commands/firewall/ufw.md)
- [`nftables`](../commands/firewall/nftables.md)
- [`iptables`](../commands/firewall/iptables.md)

---

## 5. SSH And Remote Access

Use SSH properly: keys, agents, config, jump hosts, and tunnels, then harden it for production.

Main note:

- [5_ssh_remote_access.md](./5_ssh_remote_access.md)

Concepts:

- [SSH deep dive](../concepts/ssh/ssh_deepdive.md)

Commands:

- [`sshpass`](../commands/ssh/ssh_sshpass.md)
- [`ssh-keygen`, `ssh-agent`, `~/.ssh/config`](../commands/ssh/ssh_keys_agent_config.md)
- [`scp`, `rsync`](../commands/ssh/scp_rsync.md)
- [`nc`](../commands/network-diagnostics/nc.md)

---
