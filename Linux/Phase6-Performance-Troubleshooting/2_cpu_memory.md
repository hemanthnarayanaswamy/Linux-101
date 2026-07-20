# CPU & Memory

**Roadmap: Week 36.** Diagnose CPU saturation, context-switch storms, memory pressure, and OOM.

See the concept note: [CPU & memory](../concepts/performance/cpu_memory.md).

## top / htop Columns

- `%CPU`, `%MEM` — per-process usage.
- **`VIRT` (VSZ)** vs **`RES` (RSS)** vs `SHR` — virtual (mapped) vs resident (real RAM) vs shared. **RES is the one that matters** for memory pressure; VIRT can be huge and meaningless.
- `S` — process state (R running, S sleeping, D uninterruptible/usually I/O, Z zombie).

See [`top`, `htop`](../commands/system-monitoring/top_htop.md).

## vmstat 1

```
r  b   swpd free buff cache  si so  ...  us sy id wa st
```

- `r` > #CPUs → CPU **saturation** (the real signal, not %busy).
- `si`/`so` sustained → memory pressure/swapping.
- `wa` high → **iowait**: CPU idle waiting on I/O — an **I/O** problem, chase it in [disk I/O](./3_disk_io.md), don't add CPUs.
- `st` → stolen by hypervisor (noisy VM neighbor).

Tools: [`vmstat`, `mpstat`, `pidstat`](../commands/performance/vmstat_mpstat_pidstat.md).

## RSS vs VSZ

- **VSZ / VIRT** — everything mapped (code, libs, reserved, mmap). Often gigabytes; reserving address space isn't using RAM.
- **RSS / RES** — physical RAM actually occupied. The number to watch.

```bash
grep -E 'VmRSS|VmSwap' /proc/<pid>/status    # real RAM + swapped-out for a process
```

## Memory: free -m and /proc/meminfo

- **Watch `available`, not `free`.** Linux uses spare RAM as reclaimable page cache, so `free` is often near zero on a healthy box.
- `Buffers`/`Cached` = page cache = good, reclaimable.

See [`free`](../commands/performance/free.md).

## Swap: Fine vs Disaster

- A little swap **used** (static) = fine.
- Continuous swap **activity** (thrashing) = disaster.

## OOM Killer

When RAM + swap are exhausted, the kernel kills a process (by `oom_score`) to survive:

```bash
dmesg | grep -i -E 'killed process|oom'      # find the victim
cat /proc/<pid>/oom_score
```

See [`dmesg`](../commands/logging/dmesg.md).

## Practice (roadmap exercises)

1. `vmstat 1 30` idle, then during a big compile; compare.
2. `pidstat -u 1` to find the top CPU consumer; cross-check with `htop`.
3. `stress-ng --vm-bytes 90% --vm 1` until OOM; find the event in `dmesg`.
4. `/proc/<pid>/status`: explain `VmRSS`, `VmSwap`.
5. `lscpu` and `/proc/cpuinfo`: physical cores vs threads.

Load tool: [`stress-ng`](../commands/performance/stress-ng.md).

**Mini-project:** a "CPU/memory triage script" that dumps a full snapshot of CPU + memory diagnostics for a postmortem.

**Self-check:** RSS vs VSZ? Is high `%wa` (iowait) a CPU problem or an I/O problem?

## Related

- Concept: [CPU & memory](../concepts/performance/cpu_memory.md), [CPU](../concepts/cpu.md), [system load](../concepts/system_load.md)
- Commands: [`vmstat`, `mpstat`, `pidstat`](../commands/performance/vmstat_mpstat_pidstat.md), [`free`](../commands/performance/free.md), [`top`, `htop`](../commands/system-monitoring/top_htop.md), [`dmesg`](../commands/logging/dmesg.md)
