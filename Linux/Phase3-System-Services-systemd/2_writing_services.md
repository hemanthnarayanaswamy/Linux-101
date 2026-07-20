# Writing Your Own Services

**Roadmap: Week 22.** Write a unit file from scratch, with restart policies, environment files, and security hardening.

## Unit File Anatomy

A `.service` file has three sections (see [systemd units](../concepts/systemd/units.md)):

```ini
[Unit]
Description=My Python app
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 /opt/myapp/app.py
Restart=on-failure
RestartSec=3
User=myapp
Group=myapp
WorkingDirectory=/opt/myapp
EnvironmentFile=/etc/default/myapp

[Install]
WantedBy=multi-user.target
```

Save to `/etc/systemd/system/myapp.service`, then:

```bash
sudo systemctl daemon-reload      # required after creating/editing a unit
sudo systemctl enable --now myapp
systemctl status myapp
journalctl -u myapp -f
```

## `Type=` — How systemd Decides the Service Is "Up"

| Type | Use when |
|:-----|:---------|
| `simple` | ExecStart *is* the main process, stays in foreground (default) |
| `exec` | Like simple, but "started" means the binary actually exec'd |
| `forking` | The process daemonizes (forks and the parent exits) — set `PIDFile=` |
| `oneshot` | Runs once and exits (setup scripts); pair with `RemainAfterExit=yes` |
| `notify` | The service tells systemd it's ready via `sd_notify` |

> **Type=oneshot** is for one-off/setup tasks that run to completion — e.g. a migration or a mount step — rather than a long-running daemon.

## Restart Policy

```ini
Restart=on-failure       # restart only on non-zero exit / signal
RestartSec=3             # wait 3s between restarts
StartLimitBurst=5        # give up after 5 restarts...
StartLimitIntervalSec=60 # ...within 60s
```

Other `Restart=` values: `no`, `always`, `on-abnormal`, `on-success`.

## Environment

```ini
Environment=LOG_LEVEL=info          # inline, for one or two vars
EnvironmentFile=/etc/default/myapp  # from a file (KEY=value per line)
```

> Prefer **`EnvironmentFile=`** for secrets/config so values aren't baked into the unit (which is world-readable) and can change without editing the unit.

## Security Hardening

Cheap wins that sandbox a service:

```ini
NoNewPrivileges=true     # process can never gain new privileges
ProtectSystem=strict     # most of the filesystem is read-only
ProtectHome=true         # /home, /root hidden
PrivateTmp=true          # private /tmp, isolated from other services
ReadWritePaths=/var/lib/myapp   # carve out what it IS allowed to write
```

Check a running service's hardening score with `systemd-analyze security myapp`.

## Practice (roadmap exercises)

1. Unit for a Python script that loops printing to stdout → `start`, `status`, `journalctl -u`.
2. Add `Restart=on-failure`, make the script raise an exception, watch it restart.
3. Run as a non-root `User=` → verify with `ps -u myapp`.
4. Add `ProtectSystem=strict` + `PrivateTmp=true`, have it write to `/etc/foo` → confirm it fails.
5. Convert an existing bash script into a proper service.

**Mini-project:** ship a Flask "hello world" as a production-style service — dedicated system user, `/etc/default/myapp` env file, restart policy, `Protect*`/`NoNewPrivileges`, logs to journal.

**Self-check:** When would you use `Type=oneshot`? Why `EnvironmentFile=` over inline `Environment=`?

## Related

- Concept: [systemd units](../concepts/systemd/units.md)
- Commands: [`systemctl`](../commands/systemd/systemctl.md), [`journalctl`](../commands/systemd/journalctl.md)
