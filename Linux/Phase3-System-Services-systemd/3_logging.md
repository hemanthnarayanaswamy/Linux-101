# Logging: journald, rsyslog, log rotation

**Roadmap: Week 23.** Read the journal effectively, understand journald vs rsyslog, and rotate logs so they don't fill the disk.

## Two Logging Systems

See the concept note: [journald and rsyslog](../concepts/logging/journald_rsyslog.md).

- **journald** — systemd's logging service. Captures stdout/stderr of every unit + kernel messages into a **binary** store with rich metadata. Read with [`journalctl`](../commands/systemd/journalctl.md).
- **rsyslog** — the older plain-text syslog daemon. Writes `/var/log/syslog`, `auth.log`, etc. Still used for the classic text files and remote log shipping.

## Reading the Journal

```bash
journalctl -u myapp -f                 # follow one unit's logs
journalctl -p err --since "24 hours ago"   # errors in the last day
journalctl -b                          # since this boot
journalctl -k                          # kernel messages only
journalctl --since today -p warning
```

Priority levels: `0 emerg … 3 err … 6 info … 7 debug`.

## Persistent Journal

By default the journal may be volatile (`/run/log/journal/`, lost on reboot). Make it persistent:

```bash
sudo mkdir -p /var/log/journal
sudo systemctl restart systemd-journald
```

Cap its size in `/etc/systemd/journald.conf`:

```ini
[Journal]
Storage=persistent
SystemMaxUse=500M
```

```bash
journalctl --disk-usage
sudo journalctl --vacuum-size=200M
```

## Rotating File-Based Logs

journald manages its own size, but plain text logs (app logs, rsyslog files) need [`logrotate`](../commands/logging/logrotate.md).

`/etc/logrotate.d/myapp`:

```conf
/var/log/myapp.log {
    daily
    rotate 14
    compress
    notifempty
    missingok
    create 0640 myapp myapp
}
```

```bash
logrotate -d /etc/logrotate.d/myapp   # dry-run
sudo logrotate -f /etc/logrotate.d/myapp
```

## Structured Logging

Ship an entry with metadata systemd can filter on:

```bash
logger -p user.info "app started"
systemd-cat -t myapp -p info echo "hello"
```

## Practice (roadmap exercises)

1. `journalctl -u myapp -f` — tail your Week-22 service.
2. `journalctl -p err --since "24 hours ago"` — every recent error.
3. Make the journal persistent, restart journald, verify it survives reboot.
4. Limit journal to 500MB via `SystemMaxUse=` in `journald.conf`.
5. Write a logrotate config for `/var/log/myapp.log`: daily, keep 14, gzip, `notifempty`.

**Mini-project:** a "log triage runbook" that walks a junior engineer through diagnosing "the app is broken" using only `journalctl` and logrotate-managed logs — 5 example failure modes and how to find each.

**Self-check:** How do you ship a journal entry with metadata? What does `journalctl -k` show?

## Related

- Concept: [journald and rsyslog](../concepts/logging/journald_rsyslog.md)
- Commands: [`journalctl`](../commands/systemd/journalctl.md), [`logrotate`](../commands/logging/logrotate.md), [`dmesg`](../commands/logging/dmesg.md)
