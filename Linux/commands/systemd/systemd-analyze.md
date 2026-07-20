# The `systemd-analyze` Command

`systemd-analyze` inspects boot performance and unit timing. Main tool for the "why is boot slow?" question. See [the boot process](../../concepts/systemd/boot_process.md) for context.

## Boot Timing

```bash
systemd-analyze
# Startup finished in 3.1s (kernel) + 8.4s (userspace) = 11.5s
```

Shows total time split between kernel/initramfs and userspace (systemd).

## Slowest Units

```bash
systemd-analyze blame
# lists every unit by how long it took to initialize, slowest first
```

Use this to find the service dragging boot down.

## Critical Chain

```bash
systemd-analyze critical-chain
# the chain of units that actually gated boot time (time-ordered dependencies)
```

`blame` shows the slowest units in isolation; `critical-chain` shows which of them were actually on the critical path (a slow unit that runs in parallel may not matter).

## Other Useful Subcommands

```bash
systemd-analyze plot > boot.svg     # visual timeline of the boot
systemd-analyze verify myapp.service # check a unit file for errors
systemd-analyze security nginx       # score a service's hardening (Protect*, etc.)
```

## Related

- Concept: [the boot process](../../concepts/systemd/boot_process.md)
- [`systemctl`](./systemctl.md), [`dmesg`](../logging/dmesg.md)
