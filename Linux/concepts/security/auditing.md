# Auditing & Intrusion Detection

How to answer "who did what, and did anything change?" on a server.

## auditd vs journald

Both log events, but they answer different questions:

| | journald | auditd |
|:--|:--|:--|
| Purpose | Operational logs — what services *said* | Security auditing — what the *kernel observed* |
| Source | Service stdout/stderr, kernel messages | Kernel audit subsystem (syscalls, file access) |
| Tamper focus | Convenience | Compliance/forensics (records access to sensitive files) |
| Read with | `journalctl` | `ausearch`, `aureport` |

auditd can record events journald never sees — e.g. *every read of `/etc/shadow`*, or *any change to `/etc/passwd`* — because it hooks the kernel's audit path, not application logging.

```bash
# rule: log every write/attribute-change to /etc/passwd, tag it "passwd-watch"
auditctl -w /etc/passwd -p wa -k passwd-watch
ausearch -k passwd-watch          # find those events
aureport --summary
```

Persistent rules live in `/etc/audit/rules.d/*.rules`. See [`auditd` commands](../../commands/security/auditd.md).

## File Integrity Monitoring (AIDE)

A **file integrity** tool records a cryptographic baseline (hashes, sizes, perms, inodes) of important files, then later re-checks to detect tampering.

```
aide --init      → build baseline database
   ...time passes, maybe an attacker edits /etc/sudoers...
aide --check     → report anything that changed vs the baseline
```

> **Store the AIDE database off-host.** An attacker who edits a file can also edit the local baseline to hide it. A copy kept elsewhere is the trustworthy reference. See [AIDE & Lynis](../../commands/security/aide_lynis.md).

## Login Tracking

Linux records login history in binary files, read via dedicated tools (see [login tracking](../../commands/security/login_tracking.md)):

| File | Tool | Contains |
|:-----|:-----|:---------|
| `/var/log/wtmp` | `last` | Successful logins/logouts + reboots |
| `/var/log/btmp` | `lastb` | **Failed** login attempts (brute-force signal) |
| `/var/log/lastlog` | `lastlog` | Most recent login per user |

## Automated Auditing (Lynis)

`lynis audit system` scans the host against hardening best practices and prints findings + a hardening index score — a fast way to see what to fix next.

## Related

- Commands: [`auditd`](../../commands/security/auditd.md), [AIDE & Lynis](../../commands/security/aide_lynis.md), [login tracking](../../commands/security/login_tracking.md)
- Concept: [DAC vs MAC](./dac_vs_mac.md)
