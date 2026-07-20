# AIDE and Lynis

Two audit tools: **AIDE** detects file tampering; **Lynis** scans the whole host against hardening best practices. See [auditing & intrusion detection](../../concepts/security/auditing.md).

## AIDE — File Integrity

AIDE builds a cryptographic **baseline** of chosen files, then later re-checks for changes (edits, permission changes, new/deleted files).

```bash
sudo apt install aide
sudo aideinit                        # build the initial database (slow)
# on Ubuntu the new DB is written as aide.db.new — activate it:
sudo mv /var/lib/aide/aide.db.new /var/lib/aide/aide.db

# ...later, after something may have changed...
sudo aide --check                    # compare current state to the baseline
sudo aide --update                   # accept current state as the new baseline
```

Config: `/etc/aide/aide.conf` — selects which paths to watch and which attributes (hash, size, perms, inode…).

> **Store the database off-host.** An attacker who edits `/etc/sudoers` can also rewrite a local `aide.db` to match. A copy kept elsewhere is the only trustworthy reference (Week 33 self-check).

## Lynis — Automated Hardening Audit

Lynis runs hundreds of checks and prints warnings, suggestions, and a **hardening index** (0–100).

```bash
sudo apt install lynis
sudo lynis audit system              # full scan
sudo lynis audit system --quick      # non-interactive (good for cron/timers)
```

Output:
- **Warnings** and **Suggestions** with IDs.
- A **Hardening index** score (the roadmap capstone targets ≥ 75).
- Full log at `/var/log/lynis.log`, report data at `/var/log/lynis-report.dat`.

Workflow: run it → pick the top findings → remediate → re-run and watch the score climb (keep a before/after report).

## Related

- Concept: [auditing & intrusion detection](../../concepts/security/auditing.md)
- [`auditd`](./auditd.md), [login tracking](./login_tracking.md)
