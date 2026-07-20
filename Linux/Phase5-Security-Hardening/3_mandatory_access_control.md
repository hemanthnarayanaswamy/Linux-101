# Mandatory Access Control: AppArmor / SELinux

**Roadmap: Week 32.** Understand what MAC adds beyond regular permissions, and read/modify AppArmor or SELinux enough to unblock yourself.

## DAC vs MAC

See the concept note: [DAC vs MAC](../concepts/security/dac_vs_mac.md).

- **DAC** (Discretionary) — standard Unix perms. The owner decides access; `chmod`/`chown`. A compromised service can reach everything its user can.
- **MAC** (Mandatory) — a system-wide policy that confines each *program* to exactly what it's declared to need, and that **even root can't casually override**.

> Key point: on a MAC-enforcing system, `chmod 777` may **not** fix an "access denied" — the MAC layer can still block it. Always check the security layer, not just the mode bits.

## AppArmor (Ubuntu default)

Path-based profiles in `/etc/apparmor.d/`. See [AppArmor commands](../commands/security/apparmor.md).

```bash
sudo aa-status                       # loaded profiles, enforce vs complain
sudo aa-complain /usr/sbin/nginx     # audit-only: log would-be denials
sudo aa-enforce  /usr/sbin/nginx     # enforce
sudo journalctl -k | grep -i apparmor  # read denials (apparmor="DENIED")
```

Two modes: **enforce** (block violations) and **complain** (log only — for building a profile).

## SELinux (RHEL default)

Label/context-based. Every file and process has a `user:role:type:level` context. See [SELinux commands](../commands/security/selinux.md).

```bash
getenforce                    # Enforcing | Permissive | Disabled
sudo setenforce 0             # Permissive (log, don't block) — for debugging
ls -Z /var/www/html           # file contexts (the "type" is what policy keys on)
sudo audit2allow -a           # turn recent denials into a candidate policy
sudo restorecon -Rv /var/www/html   # fix mislabeled files
```

- **Enforcing** = policy applied, violations blocked. **Permissive** = violations logged but allowed (debugging only).

## Is it permissions or MAC?

When nginx can't read a file in a non-default location:

1. Check DAC: `ls -l` / `namei -l /path` — are the mode/owner right?
2. Check MAC: look for `apparmor="DENIED"` in the journal (Ubuntu) or `ls -Z` + `ausearch -m avc` (RHEL).

A normal permission problem is `EACCES`; a MAC block shows up in the security logs even when the mode bits look fine.

## Practice (roadmap exercises)

1. `aa-status` on Ubuntu; list enforced profiles; read one (e.g. nginx).
2. Put a profile in complain mode, generate violations, read the logs.
3. (RHEL VM) Toggle SELinux to permissive, run a service, use `audit2allow`.
4. `ls -Z /var/www/html` — explain the type field.
5. Diagnose: nginx fails to read a file in a non-default location — permissions or AppArmor? How do you check?

**Mini-project:** write an AppArmor profile for a small custom binary (e.g. your Phase 3 Flask app) allowing only what it needs; test enforcement.

**Self-check:** Practical difference between Enforcing and Permissive? Why won't `chmod 777` always fix a problem on an SELinux-enforcing system?

## Related

- Concept: [DAC vs MAC](../concepts/security/dac_vs_mac.md)
- Commands: [AppArmor](../commands/security/apparmor.md), [SELinux](../commands/security/selinux.md)
