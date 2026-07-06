# `nmcli` and Netplan

> STATUS: TODO — new stub. Roadmap Phase 3 / Week 16 topic: "nmcli (NetworkManager) basics on Ubuntu desktop; netplan on server."

Referenced from [2._Linux_Networking_Fundmentals.md](../../Phase2-Networking/2._Linux_Networking_Fundmentals.md), which already covers the older `/etc/network/interfaces` style config — these two are the modern equivalents.

## `nmcli` — To Cover

- [ ] `nmcli device status` / `nmcli connection show` — list interfaces and profiles.
- [ ] `nmcli device show <iface>` — inspect one interface in detail.
- [ ] Create/activate/deactivate a connection profile.
- [ ] Assign a static IPv4 address, gateway, and DNS via `nmcli con mod ... ipv4.addresses/ipv4.gateway/ipv4.dns`.
- [ ] When `nmcli` applies vs. when Netplan is the source of truth (desktop vs. server).

## Netplan — To Cover

- [ ] `/etc/netplan/*.yaml` structure (renderer, ethernets, dhcp4/addresses/gateway4/nameservers).
- [ ] `netplan generate`, `netplan apply`, and importantly `netplan try` (auto-reverts if you get locked out) — Week 16 Exercise 5.
- [ ] Static IP config end-to-end in a VM (snapshot first!), then revert.

## Quick Reference (fill in as you learn)

```bash
nmcli device status
nmcli connection show
nmcli device show eth0

# /etc/netplan/00-installer-config.yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]

sudo netplan try
sudo netplan apply
```
