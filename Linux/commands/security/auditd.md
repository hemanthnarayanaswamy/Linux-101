# The `auditd` Commands (`auditctl`, `ausearch`, `aureport`)

`auditd` is the Linux audit daemon — it records kernel-level security events (syscalls, file access) that `journald` never sees. See [auditing & intrusion detection](../../concepts/security/auditing.md).

## Rules — `auditctl`

Rules define **what to watch**. Two common kinds:

```bash
# Watch a file: log write (w) and attribute-change (a) with a tag (key)
sudo auditctl -w /etc/passwd -p wa -k passwd-watch
sudo auditctl -w /etc/shadow -p rwxa -k shadow-watch    # read/write/exec/attr

# Syscall rule: log every use of a syscall by non-system users
sudo auditctl -a always,exit -F arch=b64 -S execve -k exec-log

sudo auditctl -l                 # list active rules
sudo auditctl -s                 # audit subsystem status
```

Runtime rules are lost on reboot — make them **persistent** in `/etc/audit/rules.d/*.rules`, then `augenrules --load`.

```
# /etc/audit/rules.d/watch.rules
-w /etc/passwd -p wa -k passwd-watch
-w /etc/shadow -p rwxa -k shadow-watch
```

## Search — `ausearch`

Find events, usually by the `-k` key you tagged:

```bash
sudo ausearch -k passwd-watch                 # everything tagged passwd-watch
sudo ausearch -k shadow-watch -ts today       # since start of today
sudo ausearch -m avc -ts recent               # SELinux denials
sudo ausearch -ua alice                        # by user
```

## Report — `aureport`

Human-readable summaries:

```bash
sudo aureport --summary
sudo aureport -au           # authentication events
sudo aureport -f           # file access events
sudo aureport --failed     # failures only
```

## auditd vs journald

auditd answers "who accessed this sensitive file?"; journald answers "what did the service log?" They complement each other — see the [concept note](../../concepts/security/auditing.md).

## Related

- Concept: [auditing & intrusion detection](../../concepts/security/auditing.md)
- [AIDE & Lynis](./aide_lynis.md), [login tracking](./login_tracking.md), [`journalctl`](../systemd/journalctl.md)
