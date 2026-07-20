# Audit & Intrusion Detection

**Roadmap: Week 33.** Track who did what on a system, and detect changes to critical files.

See the concept note: [auditing & intrusion detection](../concepts/security/auditing.md).

## auditd — Kernel-Level Auditing

`auditd` records security events the kernel observes — file access, syscalls — which `journald` never sees. See [`auditd` commands](../commands/security/auditd.md).

```bash
# log every write/attr-change to sensitive files, with a searchable key
sudo auditctl -w /etc/shadow -p rwxa -k shadow-watch
sudo auditctl -w /etc/passwd -p wa   -k passwd-watch

sudo ausearch -k shadow-watch        # find those events
sudo aureport --summary
```

Make rules survive reboot in `/etc/audit/rules.d/*.rules`.

**auditd vs journald:** auditd = security/forensics (who touched `/etc/shadow`); journald = operational logs (what the service said).

## File Integrity — AIDE

AIDE stores a cryptographic **baseline** of important files, then detects tampering. See [AIDE & Lynis](../commands/security/aide_lynis.md).

```bash
sudo aideinit                        # build baseline
# ...later...
sudo aide --check                    # report anything changed vs baseline
```

> **Store the AIDE database off-host** — an attacker who edits a file can also rewrite a local baseline to hide it.

## Login Tracking

See [login tracking](../commands/security/login_tracking.md).

```bash
last -F | head        # successful logins (wtmp)
sudo lastb | head     # FAILED logins (btmp) — brute-force signal
lastlog               # last login per account
```

## Automated Audit — Lynis

```bash
sudo lynis audit system --quick      # scan host, get findings + hardening score
```

Pick the top findings and remediate; keep a before/after report.

## Practice (roadmap exercises)

1. Audit rule logging every read/write of `/etc/shadow`; trigger it; find events with `ausearch`.
2. Watch `/etc/passwd` for changes; modify it; confirm the audit event.
3. Install AIDE, init a baseline, modify a file under `/etc`, `aide --check`, see the alert.
4. `lynis audit system`; read the report; remediate at least one of the top 3 findings.
5. `last -F | head` and `lastb | head`; identify failed logins.

**Mini-project:** a "compliance dashboard" — a bash script (daily via systemd timer) that runs `lynis audit system --quick`, `aide --check`, greps the audit log for recent privilege escalations, and emits a single JSON report.

**Self-check:** Difference between auditd and journald? Why store AIDE's database off-host?

## Related

- Concept: [auditing & intrusion detection](../concepts/security/auditing.md)
- Commands: [`auditd`](../commands/security/auditd.md), [AIDE & Lynis](../commands/security/aide_lynis.md), [login tracking](../commands/security/login_tracking.md)
