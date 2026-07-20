# The `systemctl` Command

`systemctl` is the main tool to inspect and control systemd units (services, targets, timers, sockets). See the concept note on [systemd units](../../concepts/systemd/units.md) for what a unit is.

## Lifecycle Control

```bash
systemctl start   nginx      # start now
systemctl stop    nginx      # stop now
systemctl restart nginx      # stop then start
systemctl reload  nginx      # re-read config without dropping connections (if supported)
systemctl status  nginx      # state + recent log lines + PID + cgroup
```

## Boot Behaviour

`enable`/`disable` control whether a unit starts **at boot** — separate from whether it is running **now**.

```bash
systemctl enable  nginx      # start at boot (does not start it now)
systemctl disable nginx      # don't start at boot
systemctl enable --now nginx # enable AND start immediately
systemctl is-enabled nginx
systemctl is-active  nginx
```

> **enable vs start:** `start` = run now. `enable` = run on every boot. They are independent — a service can be enabled but stopped, or running but not enabled.

## Masking

```bash
systemctl mask   cups        # symlink to /dev/null — cannot be started at all
systemctl unmask cups        # reverse it
```

Masking is stronger than `disable`: a masked unit can't be started manually or as a dependency.

## Inspecting Units

```bash
systemctl cat nginx                       # show the full unit file(s)
systemctl show nginx                       # all resolved properties
systemctl list-units --type=service        # loaded, active services
systemctl list-units --type=target         # active targets
systemctl list-unit-files --state=enabled  # what's enabled at boot
systemctl list-dependencies multi-user.target
```

## After Editing a Unit File

```bash
systemctl daemon-reload      # re-read unit files from disk (required after edits)
systemctl edit nginx         # create a drop-in override in /etc/systemd/system/
systemctl edit --full nginx  # edit a full copy
```

## Related

- Concepts: [units](../../concepts/systemd/units.md), [targets and runlevels](../../concepts/systemd/targets_runlevels.md)
- [`journalctl`](./journalctl.md) for a unit's logs (`journalctl -u nginx`)
