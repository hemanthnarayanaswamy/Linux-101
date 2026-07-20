# CPU & Memory Performance

Diagnosing CPU saturation, memory pressure, and the OOM killer. Builds on [CPU](../cpu.md), [system load](../system_load.md), and [load average](../process-management/load_average.md).

## Reading `vmstat 1`

```
 r  b   swpd   free   buff  cache   si   so  ...  us sy id wa st
```

| Field | Meaning |
|:------|:--------|
| `r` | Processes **runnable** (running + waiting for CPU). `r` > #CPUs = CPU **saturation** |
| `b` | Processes **blocked** (usually on I/O) |
| `si` / `so` | Memory **swapped in / out** per sec. Nonzero and sustained = memory pressure |
| `us` / `sy` | % CPU in user / kernel |
| `id` | % idle |
| `wa` | % waiting on I/O (**iowait**) |
| `st` | % stolen by the hypervisor (noisy neighbor on a VM) |

## Utilization vs Saturation (CPU)

`%CPU` busy (utilization) alone isn't a problem. The **run queue** `r` relative to CPU count is the saturation signal — that's what creates scheduling latency. See [USE method](./use_method.md).

## iowait Is Not a CPU Problem

High `%wa` means the CPU is **idle, waiting for disk/network I/O to return**. It's a symptom of an **I/O** bottleneck, not a CPU one — chase it with [disk I/O](./disk_io.md) tools, not by adding CPUs.

## RSS vs VSZ (and SHR)

The memory columns confuse everyone:

| Term | What it is |
|:-----|:-----------|
| **VSZ / VIRT** | *Virtual* size — everything the process has mapped (code, libs, reserved-but-unused, mmap). Often huge and mostly meaningless for "how much RAM is used" |
| **RSS / RES** | *Resident* set — physical RAM **actually** occupied right now. The number that matters for memory pressure |
| **SHR** | Shared portion of RSS (shared libs, shared mem) that other processes also use |

`VSZ` can be gigabytes while `RSS` is tiny — reserving address space isn't using RAM.

```bash
cat /proc/<pid>/status | grep -E 'VmRSS|VmSwap'   # real RAM + swapped-out for one process
```

## MemAvailable, Buffers, Cached

`free -m` / `/proc/meminfo`:

- **Cached / Buffers** — RAM used for the page cache. This is *good* — Linux uses free RAM to cache disk. It's reclaimable, not "used up."
- **MemAvailable** — the honest estimate of memory available for new work (free + reclaimable cache). **Watch this**, not `MemFree`.

## Swap: Fine vs Disaster

- A little swap **used** (static) is fine — idle pages parked on disk.
- Continuous swap **activity** (`si`/`so` steadily nonzero, "thrashing") is disaster — the system spends its time moving pages instead of working.

## The OOM Killer

When memory is exhausted and swap is gone, the kernel's **Out-Of-Memory killer** picks a process (by `oom_score`) and kills it to save the system.

```bash
dmesg | grep -i -E 'killed process|oom'      # find the OOM event and the victim
cat /proc/<pid>/oom_score                     # a process's current OOM score
```

## Related

- Concepts: [USE method](./use_method.md), [disk I/O](./disk_io.md), [CPU](../cpu.md), [system load](../system_load.md)
- Commands: [`vmstat`, `mpstat`, `pidstat`](../../commands/performance/vmstat_mpstat_pidstat.md), [`free`](../../commands/performance/free.md), [`top`, `htop`](../../commands/system-monitoring/top_htop.md), [`dmesg`](../../commands/logging/dmesg.md)
