# `iptables` Command in Linux

> STATUS: TODO — new stub, not yet studied. Roadmap Phase 3 / Week 19 calls this out as "legacy but ubiquitous — you'll see this in production for years."

`iptables` is the older userspace tool for configuring the kernel's `netfilter` packet-filtering rules, table by table (`filter`, `nat`, `mangle`, `raw`). Superseded by [nftables](./nftables.md), but still common in the wild (Docker manipulates it directly — see [ufw.md](./ufw.md)).

## To Cover

- [ ] Tables and chains: `filter` (INPUT/OUTPUT/FORWARD), `nat` (PREROUTING/POSTROUTING), `mangle`.
- [ ] Basic rule syntax: `iptables -A INPUT -p tcp --dport 22 -j ACCEPT`.
- [ ] Listing and flushing: `iptables -L -n -v`, `iptables -F`.
- [ ] Stateful matching the old way: `-m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT`.
- [ ] Default policies: `iptables -P INPUT DROP`.
- [ ] Persisting rules (`iptables-save` / `iptables-restore`, `netfilter-persistent`).
- [ ] Translate one rule you wrote in [nftables.md](./nftables.md) into `iptables` syntax and confirm they're equivalent (Week 19 Exercise 5).
- [ ] Why Docker bypassing UFW is really a `FORWARD` chain / `DOCKER-USER` chain issue.

## Quick Reference (fill in as you learn)

```bash
iptables -L -n -v
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
iptables -P INPUT DROP
iptables-save > /etc/iptables/rules.v4
```
