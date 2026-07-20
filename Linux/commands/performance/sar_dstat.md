# `sar` and `dstat`

Two broad monitors. `sar` records/replays **historical** metrics; `dstat` is a live all-in-one dashboard.

## `sar` — System Activity Reporter

`sar` (from `sysstat`) collects metrics on a schedule and lets you **look back in time** — invaluable for "what happened at 3am?" when you weren't watching.

```bash
sar 1 5              # CPU, 5 samples now
sar -r 1             # memory
sar -b 1             # I/O rates
sar -n DEV 1         # network per interface
sar -q 1             # run queue + load average
```

Historical (needs the `sysstat` collector enabled — daily files in `/var/log/sysstat/`):

```bash
sar                  # today's CPU history
sar -r -f /var/log/sysstat/sa15    # memory from the 15th
sar -s 03:00:00 -e 04:00:00        # a specific window
```

> Enable collection: install `sysstat`, set `ENABLED="true"` in `/etc/default/sysstat`. Without it, `sar` can only show live samples.

## `dstat` — Live Combined View

`dstat` shows CPU, disk, net, and memory **side by side**, refreshing live — a quick "everything at once" glance.

```bash
dstat                        # default: cpu, disk, net, paging, system
dstat -cdngy 1               # cpu, disk, net, page, sys every 1s
dstat --top-cpu --top-io     # + the top CPU and I/O process each interval
```

(On newer systems `dstat` may be provided by `pcp-dstat`.)

## When to Use Which

- **Now, at a glance:** `dstat` (or `top`/`htop`).
- **What happened earlier / trend over hours:** `sar`.

## Related

- Concepts: [USE method](../../concepts/performance/use_method.md), [CPU & memory](../../concepts/performance/cpu_memory.md)
- [`vmstat`, `mpstat`, `pidstat`](./vmstat_mpstat_pidstat.md), [`iostat`, `iotop`](./iostat_iotop.md)
