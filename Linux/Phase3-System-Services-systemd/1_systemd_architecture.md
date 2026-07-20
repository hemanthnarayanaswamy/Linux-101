# systemd Architecture

**Roadmap: Week 21.** Understand what systemd is, why it replaced SysV init, and how to read and control units.

## What systemd Is

systemd is the **init system** on modern Linux — the first userspace process, **PID 1**, started by the kernel. Everything else is (directly or indirectly) its child. It's responsible for bringing the system up, managing services, and shutting it down.

```bash
ps -p 1 -o comm=      # → systemd
```

### Why It Replaced SysV init

| SysV init | systemd |
|:----------|:--------|
| Sequential shell scripts in `/etc/init.d/` | Declarative unit files |
| Runlevels (fixed numbers) | Targets (named, composable) |
| Starts services one after another | Starts in parallel using dependencies |
| No process supervision | Restarts crashed services, tracks them via cgroups |

## Units and Their Types

Everything systemd manages is a **unit**, described by a unit file. See the concept note: [systemd units](../concepts/systemd/units.md).

```
.service   a daemon/process        .target   a group of units (sync point)
.socket    socket activation       .mount    a mount point
.timer     scheduled trigger       .path     watch a path, trigger a unit
```

Where they live (higher overrides lower):

```bash
/lib/systemd/system/      # package-shipped (don't edit)
/etc/systemd/system/      # your custom units and overrides
~/.config/systemd/user/   # per-user units
```

## Targets vs Runlevels

A **target** groups units to reach a system state — the modern replacement for runlevels. See [targets and runlevels](../concepts/systemd/targets_runlevels.md).

```bash
systemctl get-default                    # e.g. multi-user.target
systemctl list-units --type=target
```

## Dependencies

- `Wants=` — soft: also start this, but don't fail if it fails.
- `Requires=` — hard: also start this, and fail together.
- `After=` / `Before=` — ordering only (says nothing about pulling a unit in).

## Core Commands (Week 21)

```bash
systemctl status  <unit>     systemctl start   <unit>
systemctl stop    <unit>     systemctl restart <unit>
systemctl reload  <unit>     systemctl enable  <unit>   # start at boot
systemctl disable <unit>     systemctl mask    <unit>   # forbid starting
systemctl cat     <unit>     # show the unit file
systemctl list-units --type=service
systemctl list-dependencies multi-user.target
```

Full reference: [`systemctl`](../commands/systemd/systemctl.md).

## Practice (roadmap exercises)

```bash
systemctl list-units --type=service          # 1. all loaded services + state
systemctl cat ssh                            # 2. read a unit: ExecStart, deps, Restart
systemctl list-units --type=target           # 3. list targets...
systemctl get-default                        #    ...and find the default
systemctl mask cups && systemctl start cups  # 4. mask then try to start (fails)
systemctl unmask cups
systemctl list-dependencies multi-user.target # 5. dependency tree
```

**Mini-project:** a "systemd cheat sheet" — the 20 commands you'll use 95% of the time, one-line example each.

**Self-check:** What's the difference between `enable` and `start`? What is a target?

## Related

- Concepts: [units](../concepts/systemd/units.md), [targets and runlevels](../concepts/systemd/targets_runlevels.md)
- Commands: [`systemctl`](../commands/systemd/systemctl.md)
