# `pkill` and `killall`

Both commands send signals to processes by name.

## `pkill`

Matches processes by name or pattern.

```bash
pkill nginx
pkill -TERM nginx
pkill -u alice
pkill -f 'python app.py'
```

Useful options:

| Option | Meaning |
|:-------|:--------|
| `-TERM` | Send graceful termination signal |
| `-KILL` | Force kill |
| `-u user` | Match by user |
| `-f` | Match full command line |

## `killall`

Kills processes by exact command name.

```bash
killall nginx
killall -TERM nginx
killall -KILL nginx
```

Prefer `pkill` when you need filters. Use `killall` carefully because it may affect every matching process.
