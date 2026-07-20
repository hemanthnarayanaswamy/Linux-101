# The USE Method & Performance Methodology

Performance debugging needs a **system**, not a pile of tools. Brendan Gregg's **USE method** gives you one.

## USE — Utilization, Saturation, Errors

For **every resource** (CPU, memory, disk, network), check three things:

| Metric | Question | Example |
|:-------|:---------|:--------|
| **Utilization** | How busy is it? (% of time in use) | CPU 90% busy, disk 80% `%util` |
| **Saturation** | How much extra work is *queued/waiting*? | Run-queue length, swap activity, iowait |
| **Errors** | Are there error events? | Dropped packets, disk errors, OOM kills |

The insight: **utilization and saturation are different**. A CPU at 100% utilization with an empty run queue is *fully used but not overloaded*. The same CPU at 100% with 20 processes waiting is *saturated* — that's where latency comes from. High utilization is normal; high **saturation** is the problem.

## The 60-Second Checklist

Gregg's first-60-seconds triage on a "slow server" — run these in order and note what's abnormal:

```bash
uptime            # load averages: trend over 1/5/15 min
dmesg | tail      # recent kernel errors, OOM kills, disk errors
vmstat 1          # r (run queue), b (blocked), si/so (swap), wa (iowait)
mpstat -P ALL 1   # per-CPU balance — one hot core vs all busy?
pidstat 1         # per-process CPU, refreshed each second
iostat -xz 1      # per-disk: %util, await (disk pressure)
free -m           # memory + available; swap used?
sar -n DEV 1      # network throughput per interface
top / htop        # overall picture, top consumers
```

Map each to USE: `uptime`/`vmstat r` → CPU saturation; `free`/`si so` → memory saturation; `iostat await/%util` → disk; `dmesg` → errors.

## Establish a Baseline First

You can't call something "slow" without knowing "normal." Record these metrics on a healthy system so that during an incident you're comparing against reality, not a guess. **"The server is slow" is a useless ticket** — it names no resource, no metric, and no baseline.

## Related

- Concepts: [CPU & memory](./cpu_memory.md), [disk I/O](./disk_io.md), [network performance](./network_performance.md)
- Existing notes: [system load](../system_load.md), [load average](../process-management/load_average.md), [CPU](../cpu.md)
- Commands: [`vmstat`, `mpstat`, `pidstat`](../../commands/performance/vmstat_mpstat_pidstat.md), [`top`, `htop`](../../commands/system-monitoring/top_htop.md), [`uptime`](../../commands/system-monitoring/uptime.md)
