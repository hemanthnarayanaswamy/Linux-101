# The `logrotate` Command

`logrotate` rotates, compresses, and expires file-based log files so they don't fill the disk. It handles plain-text logs under `/var/log/` (app logs, rsyslog files) — **not** the systemd journal, which manages its own size (see [journald and rsyslog](../../concepts/logging/journald_rsyslog.md)).

## How It Runs

- Config: `/etc/logrotate.conf` (global defaults) + drop-ins in `/etc/logrotate.d/` (one file per app).
- Triggered on a schedule by a systemd timer (`logrotate.timer`) or cron — usually daily.

## Example Config

`/etc/logrotate.d/myapp`:

```conf
/var/log/myapp.log {
    daily              # rotate once a day
    rotate 14          # keep 14 old copies, then delete
    compress           # gzip rotated files
    delaycompress      # compress on the next cycle (keeps newest .1 readable)
    missingok          # don't error if the log is absent
    notifempty         # skip rotation if the file is empty
    create 0640 www-data www-data   # recreate with these perms/owner
    postrotate
        systemctl reload myapp >/dev/null 2>&1 || true
    endscript
}
```

## Common Directives

| Directive | Meaning |
|:----------|:--------|
| `daily` / `weekly` / `monthly` | Rotation frequency |
| `size 100M` | Rotate when the file exceeds a size instead |
| `rotate N` | Keep N rotated copies |
| `compress` | gzip old copies |
| `notifempty` | Don't rotate an empty file |
| `copytruncate` | Copy then truncate in place (for apps that keep the file open and can't reopen) |
| `postrotate`…`endscript` | Command to run after rotation (e.g. signal the app to reopen its log) |

## Testing

```bash
logrotate -d /etc/logrotate.d/myapp     # debug/dry-run — shows what WOULD happen
sudo logrotate -f /etc/logrotate.d/myapp # force a rotation now
cat /var/lib/logrotate/status            # last-rotation state
```

## Related

- Concept: [journald and rsyslog](../../concepts/logging/journald_rsyslog.md)
- [`journalctl`](../systemd/journalctl.md)
