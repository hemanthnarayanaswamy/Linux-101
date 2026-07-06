# CIDR and Subnetting

> STATUS: TODO — new stub. Roadmap Phase 3 / Week 15 topic. [1_intro_networking.md](../../Phase2-Networking/1_intro_networking.md) already has the private-range table (/8, /12, /16); this note is for the subnetting math itself.

## To Cover

- [ ] What a CIDR suffix means: `/24` = 24 network bits, 8 host bits.
- [ ] Host count math for `/24`, `/16`, `/8` (Week 15 Exercise 2) — including the "usable hosts = 2^n − 2" network/broadcast caveat.
- [ ] Subnet mask ↔ CIDR notation conversion (`255.255.255.0` ↔ `/24`).
- [ ] Splitting a `/24` into smaller subnets (e.g. four `/26`s) — when and why you'd do this.
- [ ] Reading `ip addr` output and identifying the CIDR suffix on your own interface (ties into [ip.md](../../commands/network-interfaces/ip.md)).

## Quick Reference (fill in as you learn)

```bash
/24  → 255.255.255.0   → 256 addresses  → 254 usable hosts
/16  → 255.255.0.0     → 65536 addresses → 65534 usable hosts
/8   → 255.0.0.0       → 16777216 addresses → 16777214 usable hosts
```
