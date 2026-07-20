# Performance Methodology

**Roadmap: Week 35.** Have a *system* for performance debugging, not just tools. Memorize the USE method.

See the concept note: [USE method & methodology](../concepts/performance/use_method.md).

## The USE Method

For **every resource** (CPU, memory, disk, network), check three things:

| | Question |
|:--|:--|
| **U**tilization | How busy is it? |
| **S**aturation | How much work is queued/waiting? |
| **E**rrors | Any error events? |

Key idea: **utilization ≠ saturation**. 100% CPU with an empty run queue is *fully used*, not *overloaded*. 100% CPU with 20 processes waiting is **saturated** — that's where latency comes from. High utilization is normal; high **saturation** is the problem.

## The 60-Second Checklist

First-minute triage on a "slow server," in order:

```bash
uptime            # load trend (1/5/15 min)
dmesg | tail      # kernel errors, OOM kills, disk errors
vmstat 1          # run queue (r), swap (si/so), iowait (wa)
mpstat -P ALL 1   # per-CPU balance
pidstat 1         # per-process CPU
iostat -xz 1      # per-disk %util / await
free -m           # memory + available
sar -n DEV 1      # network throughput
top / htop        # overall + top consumers
```

Commands: [`vmstat`/`mpstat`/`pidstat`](../commands/performance/vmstat_mpstat_pidstat.md), [`iostat`](../commands/performance/iostat_iotop.md), [`free`](../commands/performance/free.md), [`sar`](../commands/performance/sar_dstat.md), [`dmesg`](../commands/logging/dmesg.md), [`top`/`htop`](../commands/system-monitoring/top_htop.md), [`uptime`](../commands/system-monitoring/uptime.md).

## Baseline First

You can't call something "slow" without knowing "normal." Record these metrics on a healthy system so an incident is a *comparison*, not a guess. **"The server is slow" is a useless ticket** — no resource, metric, or baseline named.

## Practice (roadmap exercises)

1. Walk the 60-second checklist on your box; write down expected ranges for each metric.
2. `stress-ng --cpu 4 --timeout 60s`; re-run the checklist; note which metrics moved.
3. Repeat with `stress-ng --vm 2 --vm-bytes 1G` (memory).
4. Repeat with `stress-ng --io 4` (I/O).
5. Repeat with network using `iperf3` between two VMs.

Load tool: [`stress-ng`](../commands/performance/stress-ng.md).

**Mini-project:** a "USE-method runbook" — a decision tree: given symptom X (high latency / high CPU / OOM / slow disk), which tools do you reach for and in what order?

**Self-check:** Difference between utilization and saturation? Why is "the server is slow" a useless ticket?

## Related

- Concept: [USE method](../concepts/performance/use_method.md)
- Downstream: [CPU & memory](./2_cpu_memory.md), [disk I/O](./3_disk_io.md), [network](./4_network_performance.md)
