# `nohup` and `disown`

These help long-running commands survive terminal logout.

## `nohup`

Runs a command while ignoring `SIGHUP`.

```bash
nohup ./backup.sh > backup.log 2>&1 &
```

Meaning:

| Part | Meaning |
|:-----|:--------|
| `nohup` | Ignore terminal hangup |
| `> backup.log` | Send stdout to log file |
| `2>&1` | Send stderr to same log |
| `&` | Run in background |

## `disown`

Removes a job from the shell's job table.

```bash
long-task > task.log 2>&1 &
jobs
disown %1
```

If a command is already running in foreground:

```bash
Ctrl+Z
bg
disown
```

For real services, prefer `systemd` over `nohup`.
