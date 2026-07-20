# The `journalctl` Command

`journalctl` reads the systemd journal — the binary log store managed by `systemd-journald`. See [journald and rsyslog](../../concepts/logging/journald_rsyslog.md) for how the journal works.

## Filter by Unit

```bash
journalctl -u nginx              # all logs for one unit
journalctl -u nginx -f           # follow (like tail -f)
journalctl -u nginx --since today
```

## Filter by Time

```bash
journalctl --since "1 hour ago"
journalctl --since "2024-01-01" --until "2024-01-02"
journalctl -S "10:00" -U "11:00"   # -S/--since, -U/--until
```

## Filter by Priority

Severity levels 0–7 (`emerg`…`debug`):

```bash
journalctl -p err                       # err and worse
journalctl -p err --since "24 hours ago"
journalctl -p warning..err              # a range
```

## Other Useful Flags

```bash
journalctl -b            # logs since the current boot
journalctl -b -1         # the previous boot
journalctl -k            # kernel messages only (like dmesg)
journalctl -e            # jump to the end
journalctl -n 50         # last 50 lines
journalctl -o json-pretty   # structured output with all metadata fields
```

## Disk Usage / Cleanup

```bash
journalctl --disk-usage
sudo journalctl --vacuum-size=200M
sudo journalctl --vacuum-time=2weeks
```

## Structured Logging

Send a log entry with custom metadata (readable back with `journalctl` filters):

```bash
logger -p user.info "app started"
systemd-cat -t myapp echo "hello from myapp"
```

## Related

- Concept: [journald and rsyslog](../../concepts/logging/journald_rsyslog.md)
- [`systemctl`](./systemctl.md), [`dmesg`](../logging/dmesg.md)
