# The `fail2ban` Command

`fail2ban` watches log files for repeated failures (e.g. SSH auth failures) and **temporarily bans** the offending IP via the firewall. The standard brute-force defense for internet-facing SSH.

## How It Works

- **Filters** define what a failure looks like (regex against a log).
- **Jails** tie a filter to an action: after `maxretry` failures within `findtime`, ban the IP for `bantime`.
- Bans are enforced by adding firewall rules (nftables/iptables — see [`ufw`](../firewall/ufw.md), [`nftables`](../firewall/nftables.md)).

## Configure

Never edit `jail.conf` — use `jail.local` (overrides survive package upgrades):

```ini
# /etc/fail2ban/jail.local
[DEFAULT]
bantime  = 1h
findtime = 10m
maxretry = 3

[sshd]
enabled = true
port    = 2222        # match your sshd port
```

This is the roadmap's "3 strikes in 10 min → 1 hour ban."

```bash
sudo systemctl restart fail2ban
```

## Operate

```bash
sudo fail2ban-client status                 # list active jails
sudo fail2ban-client status sshd            # banned IPs, totals for the sshd jail
sudo fail2ban-client set sshd unbanip 1.2.3.4   # manually unban
sudo fail2ban-client set sshd banip 1.2.3.4     # manually ban
```

## Verify a Ban

Trigger failed logins from another host, then:

```bash
sudo fail2ban-client status sshd     # the IP appears under "Banned IP list"
sudo journalctl -u fail2ban -f       # watch bans happen live
```

## Related

- [SSH server hardening](../ssh/sshd_hardening.md), [`ufw`](../firewall/ufw.md), [`nftables`](../firewall/nftables.md)
- Concept: [PAM](../../concepts/security/pam.md) (`pam_faillock` is the local-login equivalent)
