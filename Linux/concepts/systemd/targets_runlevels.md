# Targets and Runlevels

A **target** is a unit (`.target`) that groups other units to reach a known system state. Targets are systemd's replacement for the old SysV **runlevels**.

## Runlevel → Target Mapping

| Runlevel | systemd Target | Meaning |
|:--------:|:---------------|:--------|
| 0 | `poweroff.target` | Halt / shut down |
| 1 | `rescue.target` | Single-user, minimal (recovery) |
| 3 | `multi-user.target` | Multi-user, text mode, networking (servers) |
| 5 | `graphical.target` | Multi-user + GUI |
| 6 | `reboot.target` | Reboot |

- `multi-user.target` is the normal state for a server. `graphical.target` pulls in `multi-user.target` and adds the display manager.

## Key Commands

```bash
systemctl get-default              # show the boot target
systemctl set-default multi-user.target   # boot to text mode next time

systemctl list-units --type=target        # currently active targets
systemctl list-dependencies multi-user.target   # what it pulls in

systemctl isolate rescue.target    # switch to another target now (like changing runlevel)
```

## Why Targets Instead of Runlevels

- Runlevels were a single number in a fixed sequence. Targets are named units that can depend on each other via `Wants=`/`Requires=`, so they compose flexibly.
- Special "sync point" targets like `network.target` or `basic.target` exist only to order other units — nothing runs "at" them directly.

## Related

- [systemd units](./units.md)
- [Boot process](./boot_process.md)
- Command reference: [`systemctl`](../../commands/systemd/systemctl.md)
