# journald and rsyslog

Two logging systems coexist on modern Linux. Understanding who owns what avoids a lot of confusion.

## journald (systemd-journald)

- The systemd logging service. Captures stdout/stderr of every service, kernel messages, and structured metadata.
- Stores logs in a **binary** format (not plain text) — you read it with [`journalctl`](../../commands/systemd/journalctl.md), not `cat`.
- Each entry carries metadata fields: `_SYSTEMD_UNIT`, `PRIORITY`, `_PID`, `MESSAGE_ID`, etc. — which is why you can filter by unit, priority, or time so precisely.

### Volatile vs Persistent

| Location | Persistence |
|:---------|:------------|
| `/run/log/journal/` | Volatile — lost on reboot (default if the dir below doesn't exist) |
| `/var/log/journal/` | Persistent — survives reboot |

Make it persistent:

```bash
sudo mkdir -p /var/log/journal
sudo systemctl restart systemd-journald
```

### Controlling Disk Usage

Config: `/etc/systemd/journald.conf`

```ini
[Journal]
Storage=persistent
SystemMaxUse=500M     # cap total journal size
```

```bash
journalctl --disk-usage
sudo journalctl --vacuum-size=200M
sudo journalctl --vacuum-time=2weeks
```

## rsyslog

- The older, plain-text syslog daemon. Still present because many tools and remote log servers speak the syslog protocol.
- Writes traditional text files under `/var/log/` (`syslog`, `auth.log`, `kern.log`).
- Config: `/etc/rsyslog.conf` and drop-ins in `/etc/rsyslog.d/`.
- On most modern Ubuntu systems journald is the source of truth and forwards to rsyslog, which writes the familiar text files.

## Priorities (severity levels)

Shared syslog levels, used by both systems (`journalctl -p err`):

```
0 emerg   1 alert   2 crit   3 err
4 warning 5 notice  6 info   7 debug
```

## logrotate

Neither system rotates *file-based* logs (app logs, rsyslog files) on its own — that's [`logrotate`](../../commands/logging/logrotate.md)'s job: rotate, compress, and expire files under `/var/log/` so they don't fill the disk. (journald manages its own size via the vacuum settings above.)

## Related

- Commands: [`journalctl`](../../commands/systemd/journalctl.md), [`logrotate`](../../commands/logging/logrotate.md), [`dmesg`](../../commands/logging/dmesg.md)
