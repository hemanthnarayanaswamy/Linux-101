# The `dmesg` Command

`dmesg` prints the **kernel ring buffer** — messages from the kernel itself, especially hardware detection and driver events during and after boot. Useful for diagnosing hardware, disk, and driver problems that never reach the application logs.

## Basic Usage

```bash
dmesg                 # dump the whole kernel ring buffer
dmesg | head -100     # early boot / hardware detection messages
dmesg -H              # human-readable, paged, with relative timestamps
dmesg -w              # follow (wait for new messages, like tail -f)
dmesg -T              # human-readable timestamps instead of seconds-since-boot
```

> On many systems reading `dmesg` as a normal user is restricted — use `sudo dmesg`.

## Filtering

```bash
dmesg -l err,warn     # only error and warning level messages
dmesg -f kern         # only kernel facility
dmesg | grep -i usb   # hardware you care about (usb, eth, sda, memory)
```

## dmesg vs journalctl

- `dmesg` reads the live kernel ring buffer (a fixed-size in-memory buffer that wraps around).
- `journalctl -k` shows the **same** kernel messages but from the journal, so they persist across reboots and carry timestamps/metadata.

```bash
journalctl -k          # kernel messages, current boot
journalctl -k -b -1    # kernel messages from the previous boot
```

## Related

- Concepts: [boot process](../../concepts/systemd/boot_process.md), [journald and rsyslog](../../concepts/logging/journald_rsyslog.md)
- [`journalctl`](../systemd/journalctl.md)
