# Login Tracking (`last`, `lastb`, `lastlog`)

Linux records login history in binary files under `/var/log/`. These tools read them — useful for spotting brute-force attempts and confirming who logged in when. See [auditing & intrusion detection](../../concepts/security/auditing.md).

## The Files and Their Tools

| File | Tool | Contains |
|:-----|:-----|:---------|
| `/var/log/wtmp` | `last` | Successful logins/logouts + reboots/shutdowns |
| `/var/log/btmp` | `lastb` | **Failed** login attempts |
| `/var/log/lastlog` | `lastlog` | The most recent login per user |

(These are binary — don't `cat` them; use the tools.)

## `last` — Successful Logins

```bash
last                 # recent logins/logouts, newest first
last -F              # full timestamps (login + logout)
last -a              # show the source host in the last column
last reboot          # reboot history
last -n 20           # limit to 20 entries
```

## `lastb` — Failed Logins (brute-force signal)

```bash
sudo lastb           # failed attempts (needs root)
sudo lastb | head    # most recent failures — repeated IPs/users = attack
```

A flood of `lastb` entries from one IP is exactly what [`fail2ban`](./fail2ban.md) automates blocking.

## `lastlog` — Last Login Per User

```bash
lastlog                        # every account's most recent login
lastlog -u deployer            # one user
lastlog -b 30                  # accounts NOT logged in for 30+ days (stale accounts)
```

## Also Useful

```bash
who                  # who is logged in right now
w                    # who + what they're running + load
```

(See [`who`](../users-groups-permissions/who.md) and [`groupadd`, `groups`, `id`, `w`](../users-groups-permissions/groupadd_groups_id_w.md).)

## Related

- Concept: [auditing & intrusion detection](../../concepts/security/auditing.md)
- [`fail2ban`](./fail2ban.md), [`auditd`](./auditd.md)
