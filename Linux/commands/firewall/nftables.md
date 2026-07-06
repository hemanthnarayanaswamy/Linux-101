# `nftables` Command in Linux

> STATUS: TODO — stub only. Fill in as you work through Week 19 (Roadmap Phase 3).

`nftables` is the modern Linux packet-filtering framework that replaces `iptables`/`ip6tables`/`arptables`/`ebtables` with one unified tool, built on the same `netfilter` kernel hooks. See [4_Firewalls.md](../../Phase2-Networking/4_Firewalls.md) for the conceptual background (chains, stateful filtering, ACCEPT/DROP/REJECT).

## To Cover

- [ ] Core vocabulary: tables, chains, rules — how they nest and how base chains hook into netfilter (`input`, `output`, `forward`).
- [ ] `nft list ruleset` — read an existing ruleset (compare it against an equivalent `ufw` config, per Week 19 Exercise 3).
- [ ] `nft add table`, `nft add chain`, `nft add rule` — building a ruleset interactively.
- [ ] Writing a ruleset in a `.nft` file and loading it with `nft -f file.nft` (Week 19 Exercise 4):
  - allow loopback
  - allow established/related (`ct state established,related accept`)
  - allow SSH from a specific subnet
  - drop everything else inbound
- [ ] Persisting rules across reboot (`nftables.service`, `/etc/nftables.conf`).
- [ ] Translating one rule to its legacy `iptables` equivalent — see [iptables.md](./iptables.md) (Week 19 Exercise 5).
- [ ] Rate-limiting (e.g. SSH to 3/min) for the "Hardened web server firewall" mini-project.

## Quick Reference (fill in as you learn)

```bash
nft list ruleset
nft list tables
nft add table inet filter
nft add chain inet filter input { type filter hook input priority 0 \; policy drop \; }
nft add rule inet filter input iif lo accept
nft add rule inet filter input ct state established,related accept
nft -f /etc/nftables.conf
```
