# `vmstat`, `mpstat`, `pidstat`

Three system-stat tools from the 60-second checklist. `vmstat` = whole-system snapshot, `mpstat` = per-CPU, `pidstat` = per-process. See [CPU & memory](../../concepts/performance/cpu_memory.md) and the [USE method](../../concepts/performance/use_method.md).

`mpstat`/`pidstat` come from the `sysstat` package (`apt install sysstat`).

## `vmstat` — System-Wide

```bash
vmstat 1              # refresh every 1s
vmstat 1 30          # 30 samples then stop
vmstat -w             # wide, readable columns
```

Key columns:

```
r  b   swpd free buff cache   si so   bi bo   us sy id wa st
```

| Col | Watch for |
|:----|:----------|
| `r` | Run queue > #CPUs → CPU **saturation** |
| `b` | Blocked (usually on I/O) |
| `si`/`so` | Sustained swap in/out → memory pressure |
| `wa` | High iowait → I/O bottleneck (not CPU) |
| `st` | Stolen time → noisy VM neighbor |

## `mpstat` — Per-CPU

Reveals imbalance a single averaged number hides (one hot core vs all busy):

```bash
mpstat -P ALL 1       # every CPU, each second
mpstat 1              # aggregate
```

One core pinned at 100% while others idle = a single-threaded bottleneck.

## `pidstat` — Per-Process, Over Time

Like `top` but sampled and scriptable — find *which process* is responsible:

```bash
pidstat 1             # CPU per process each second
pidstat -u 1          # CPU
pidstat -r 1          # memory (faults, RSS)
pidstat -d 1          # disk I/O per process
pidstat -t 1          # per-thread
```

## Related

- Concepts: [CPU & memory](../../concepts/performance/cpu_memory.md), [USE method](../../concepts/performance/use_method.md)
- [`free`](./free.md), [`iostat`, `iotop`](./iostat_iotop.md), [`top`, `htop`](../system-monitoring/top_htop.md)
