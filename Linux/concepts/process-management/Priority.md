# Process Priority

Linux decides which runnable process gets CPU time using scheduling priority.

For Phase 1, focus on three tools:

```text
nice
renice
ionice
```

## CPU Priority: Nice Value

A process has a nice value from `-20` to `19`.

```text
-20 = highest priority
  0 = normal priority
 19 = lowest priority
```

Lower nice value means the process is less "nice" to others and gets more scheduling preference.

Start a low-priority process:

```bash
nice -n 19 command
```

Change an existing process:

```bash
sudo renice -n 10 -p <pid>
```

## I/O Priority

`ionice` controls disk I/O priority.

```bash
ionice -c 3 command
sudo ionice -c 2 -n 7 -p <pid>
```

Common classes:

| Class | Meaning |
|:------|:--------|
| `1` | Real-time I/O |
| `2` | Best-effort I/O |
| `3` | Idle I/O only |

## SRE/DevOps Use

Use priority controls when background jobs should not hurt interactive or production workloads.

Examples:

```bash
nice -n 19 tar -czf backup.tar.gz /data
ionice -c 3 rsync -a /data/ /backup/
```

Priority is not resource isolation. For stronger control, use cgroups or systemd resource settings in later phases.
