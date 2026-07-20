# systemd Units

A **unit** is anything systemd knows how to manage. Every unit is described by a plain-text **unit file**, and its type is set by the file extension.

| Unit type | Extension | Manages |
|:----------|:----------|:--------|
| Service | `.service` | A daemon or process (most common) |
| Socket | `.socket` | A socket that starts a service on demand |
| Timer | `.timer` | A scheduled trigger (systemd's cron replacement) |
| Mount | `.mount` | A filesystem mount point |
| Target | `.target` | A named group of units (a "sync point") |
| Path | `.path` | Watches a file/dir and triggers a unit on change |

## Where Unit Files Live

Precedence is low → high; a file in a higher directory **overrides** the same-named file below it.

```bash
/lib/systemd/system/      # shipped by packages (do not edit)
/etc/systemd/system/      # your overrides and custom units (edit here)
~/.config/systemd/user/   # per-user units (systemctl --user)
```

- After editing any unit file, run `systemctl daemon-reload` so systemd re-reads it.

## Unit File Anatomy

A `.service` file has three sections:

```ini
[Unit]
Description=My app
After=network.target          # ordering only, not a hard dependency

[Service]
ExecStart=/usr/bin/myapp
Restart=on-failure

[Install]
WantedBy=multi-user.target    # where "enable" hooks it in
```

- **`[Unit]`** — metadata and dependencies (`Description`, `After`, `Before`, `Wants`, `Requires`).
- **`[Service]`** — how to run it (`ExecStart`, `Type`, `Restart`, `User`). Only for `.service` units.
- **`[Install]`** — what `systemctl enable` does (`WantedBy`). Without it, a unit cannot be enabled.

## Dependencies and Ordering

Two independent ideas — do not confuse them:

- **Requirement** (`Wants=`, `Requires=`): should another unit also be started?
  - `Wants=foo` → start `foo` too, but continue even if it fails (soft).
  - `Requires=foo` → start `foo` too, and fail if it fails (hard).
- **Ordering** (`After=`, `Before=`): in what *order*? These say nothing about whether the other unit is pulled in — only sequencing.

> A common pattern: `Requires=postgresql.service` + `After=postgresql.service` — pull it in *and* wait for it.

## Related

- [Targets and runlevels](./targets_runlevels.md)
- Command reference: [`systemctl`](../../commands/systemd/systemctl.md)
