# Phase 2 - Networking

This directory is "Phase 3 — Networking (Weeks 15–20)" in the detailed roadmap. It's numbered Phase 2 in this repo because Phase 2 of the roadmap (Shell Scripting & Automation) was skipped.

Primary roadmap sources:

- [linux-mastery-roadmap.md](../../resources/roadMap/linux-mastery-roadmap.md)
- [linux-mastery-roadmap-detailed.md](../../resources/roadMap/linux-mastery-roadmap-detailed.md) — see "Phase 3 — Networking (Weeks 15–20)"
- [networking-roadmap.md](../../resources/roadMap/networking-roadmap.md)

> Note: the note files in this folder are numbered 1–5 in the order they were written, not in roadmap week order — file 4 (`Firewalls`, Week 19) was written before file 5 (`SSH`, Week 18). The table below is ordered by **roadmap week**, so use it (not the filenames) to navigate/revise in the intended sequence.

## Quick Index

| Week | Topic | Main Note | Status |
|---:|:------|:----------|:-------|
| 15 | Networking fundamentals (OSI, TCP/IP, ports) | [1_intro_networking.md](./1_intro_networking.md) | Solid |
| 16 | Linux networking tools (`ip`, `ss`, interfaces) | [2._Linux_Networking_Fundmentals.md](./2._Linux_Networking_Fundmentals.md) | Gaps: nmcli/netplan |
| 17 | DNS and name resolution | [3_DNS_and_NameResolution.md](./3_DNS_and_NameResolution.md) | Solid |
| 18 | SSH deep dive | [5_ssh_remote_access.md](./5_ssh_remote_access.md) | Stub only — biggest gap |
| 19 | Firewalls (`ufw`, nftables, iptables) | [4_Firewalls.md](./4_Firewalls.md) | Solid concepts; commands are TODO |
| 20 | Capstone: three-tier home lab network | *(not started)* | Not started |

## Reference Indexes

| Area | Index |
|:-----|:------|
| Commands | [Linux commands index](../commands/README.md) |
| Concepts | [Linux concepts index](../concepts/README.md) |

---

## 15. Networking Fundamentals

OSI layers, TCP/IP, IPv4/CIDR, ports, three-way handshake.

Main note:

- [1_intro_networking.md](./1_intro_networking.md)

Concepts:

- [OSI model](../concepts/networking/osi_model.md) — TODO
- [CIDR and subnetting](../concepts/networking/cidr_subnetting.md) — TODO
- [TCP](../concepts/networking/tcp.md)
- [UDP](../concepts/networking/udp.md)

Commands:

- [`ip`](../commands/network-interfaces/ip.md)
- [`traceroute`](../commands/network-diagnostics/traceroute.md)
- [`mtr`](../commands/network-diagnostics/mtr.md)
- [`nc`](../commands/network-diagnostics/nc.md)

Still to do (from the roadmap exercises/mini-project):

- [ ] Draw the OSI model by hand, one protocol per layer.
- [ ] Work through `/24`, `/16`, `/8` host-count math.
- [ ] "My network diagram" mini-project (home network, draw.io/excalidraw).

---

## 16. Linux Networking Tools

Replacing `ifconfig`/`netstat`/`route` with `ip`/`ss`; interface and route configuration.

Main note:

- [2._Linux_Networking_Fundmentals.md](./2._Linux_Networking_Fundmentals.md)

Commands:

- [`ip`](../commands/network-interfaces/ip.md)
- [`ss`](../commands/network-interfaces/ss.md)
- [`ping`](../commands/network-diagnostics/ping.md)
- [`mtr`](../commands/network-diagnostics/mtr.md)
- [`tcpdump`](../commands/network-diagnostics/tcpdump.md)
- [`nmcli`, netplan](../commands/network-interfaces/nmcli_netplan.md) — TODO

Still to do:

- [ ] `nmcli` basics (list/activate/deactivate profiles, assign static IP).
- [ ] Netplan static IP config with `netplan try` in a VM.
- [ ] "ss/ip cheat sheet" mini-project — 15 networking commands, modern tool + deprecated equivalent side by side.

---

## 17. DNS and Name Resolution

Resolution order, record types, `dig`, `systemd-resolved`, TTL/caching.

Main note:

- [3_DNS_and_NameResolution.md](./3_DNS_and_NameResolution.md)

Commands:

- [`dig`, `nslookup`, `host`](../commands/dns/dig_nslookup_host.md)
- [`nmap`](../commands/network-diagnostics/nmap.md) — TODO (bonus, referenced from the DNS note but not roadmap-required)

Still to do:

- [ ] "Tiny local DNS resolver" mini-project — `dnsmasq` for `*.lab` domains, forwarding to `1.1.1.1`, with caching (no note or command file yet — consider a `dnsmasq.md` command file if you go through this).

---

## 18. SSH Deep Dive

**Biggest gap in this phase.** [5_ssh_remote_access.md](./5_ssh_remote_access.md) only covers the client/server model and confirming TCP/22 is reachable — none of the actual roadmap topics (keys, agent, config, jump hosts, tunnels, hardening) are written up yet.

Main note:

- [5_ssh_remote_access.md](./5_ssh_remote_access.md)

Concepts:

- [SSH deep dive](../concepts/ssh/ssh_deepdive.md) — TODO

Commands:

- [`sshpass`](../commands/ssh/ssh_sshpass.md)
- [SSH keys, agent, config](../commands/ssh/ssh_keys_agent_config.md) — TODO
- [`scp`, `rsync`](../commands/ssh/scp_rsync.md) — TODO

Still to do (this is most of Week 18):

- [ ] Generate an ed25519 key, `ssh-copy-id` to a second VM.
- [ ] `~/.ssh/config` host alias (`prod-web`).
- [ ] Jump host: `ssh -J` on the command line, then convert to `ProxyJump` in config.
- [ ] Local port forward (`-L`) — tunnel a remote service to localhost.
- [ ] `rsync` with `--delete --dry-run`, then for real.
- [ ] "Bastion + private host" mini-project.

---

## 19. Firewalls: nftables, ufw, iptables

Stateful filtering, default-deny posture, NetFilter internals.

Main note:

- [4_Firewalls.md](./4_Firewalls.md)

Commands:

- [`ufw`](../commands/firewall/ufw.md)
- [`nftables`](../commands/firewall/nftables.md) — TODO
- [`iptables`](../commands/firewall/iptables.md) — TODO

Still to do:

- [ ] `nft list ruleset` — map an existing `ufw` config to its underlying nftables rules.
- [ ] Write a raw `.nft` ruleset from scratch and load with `nft -f`.
- [ ] Convert one rule to its `iptables` equivalent.
- [ ] "Hardened web server firewall" mini-project — SSH from one IP, 80/443 open, SSH rate-limited to 3/min, persisted across reboot.

---

## 20. Phase Capstone — Three-Tier Home Lab Network

Not started. Roadmap project: `bastion` / `web` / `db` VMs, dnsmasq-based `.lab` name resolution, nftables on every host, one-command `ProxyJump` access via `~/.ssh/config`. Depends on Weeks 17–19 gaps above being closed first (dnsmasq, SSH hardening, nftables).

Deliverables when you get here:

- [ ] Network diagram.
- [ ] Each host's firewall config committed to git.
- [ ] Step-by-step runbook to rebuild the lab from scratch.
