# The `free` Command

`free` shows memory and swap usage at a glance. The subtlety is reading it correctly — see [CPU & memory](../../concepts/performance/cpu_memory.md).

```bash
free -h              # human-readable (KB/MB/GB)
free -m              # in MB
free -s 1            # refresh every 1s
free -w              # wide: split buffers and cache
```

Example:

```
              total   used   free   shared  buff/cache   available
Mem:           15Gi   4Gi    1Gi    200Mi   10Gi         10Gi
Swap:         2.0Gi   0B     2.0Gi
```

## Reading It Right

| Column | Meaning |
|:-------|:--------|
| `used` | RAM in active use by processes |
| `free` | Completely unused RAM (often small — that's normal) |
| `buff/cache` | RAM used as **page cache** for disk — reclaimable, this is *good* |
| **`available`** | The honest "how much can a new app get" number (free + reclaimable cache) |

> **Watch `available`, not `free`.** Linux deliberately uses spare RAM to cache disk, so `free` is often near zero on a healthy box. That cache is instantly reclaimable, so `available` is the real headroom.

## Swap

- `Swap: used` static and small → fine (idle pages parked).
- Swap **growing continuously** with steady `si`/`so` in [`vmstat`](./vmstat_mpstat_pidstat.md) → thrashing → trouble.

## Underlying Source

`free` just formats `/proc/meminfo`:

```bash
grep -E 'MemTotal|MemAvailable|Buffers|Cached|SwapTotal|SwapFree' /proc/meminfo
```

## Related

- Concept: [CPU & memory](../../concepts/performance/cpu_memory.md)
- [`vmstat`, `mpstat`, `pidstat`](./vmstat_mpstat_pidstat.md), [`top`, `htop`](../system-monitoring/top_htop.md)
