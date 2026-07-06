# `nmap` Command in Linux

> STATUS: TODO — new stub. Referenced from [3_DNS_and_NameResolution.md](../../Phase2-Networking/3_DNS_and_NameResolution.md) but not part of the core Phase 3 roadmap topics — a bonus diagnostics tool worth knowing alongside `dig`/`ping`/`mtr`.

`nmap` is a network scanning tool for host discovery, port scanning, and service/version detection. Not DNS-specific, but often used alongside `dig` when diagnosing "is this service actually reachable" questions.

## To Cover

- [ ] Basic host/port scan: `nmap <host>`.
- [ ] Scanning specific ports: `nmap -p 22,80,443 <host>`.
- [ ] Service/version detection: `nmap -sV <host>`.
- [ ] Difference between `nmap` and `nc -zv` (Week 15 Exercise 5 uses `nc` for the same kind of check).
- [ ] When this is appropriate to run vs. when it looks like an attack (scanning hosts you don't own/manage is not okay).

## Quick Reference (fill in as you learn)

```bash
nmap <host>
nmap -p 22,80,443 <host>
nmap -sV <host>
```
