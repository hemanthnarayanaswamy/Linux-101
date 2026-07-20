# strace, perf, eBPF

**Roadmap: Week 39.** Trace syscalls of misbehaving programs, use `perf` for CPU profiling, and get a taste of eBPF / `bpftrace`.

See the concept note: [tracing & profiling](../concepts/performance/tracing.md). Builds on [system calls](../concepts/system-internals/system_calls.md).

## The Ladder

| Tool | Observes | Overhead |
|:-----|:---------|:---------|
| `strace` | System calls | High |
| `ltrace` | Library calls | High |
| `perf` | CPU sampling | Low |
| `bpftrace` (eBPF) | Custom kernel probes | Very low |

## strace / ltrace

```bash
strace -c ls /tmp                 # summary: top syscalls by count + time (start here)
strace -f -p <pid> -e openat      # follow forks, only openat — "what files is it opening?"
ltrace -c ./prog                  # library-call summary
```

> `strace` traps on **every** syscall → can slow a program 10–100×. Diagnosis only, never leave attached to a hot path. See [`strace`, `ltrace`](../commands/performance/strace_ltrace.md).

## perf & Flame Graphs

```bash
sudo perf top                     # live hottest functions
sudo perf record -g -- ./myapp    # record with call graphs
sudo perf report                  # browse
```

Flame graph: **X = fraction of samples (width = CPU), not time; Y = stack depth.** Scan for the widest boxes. See [`perf`, `bpftrace`](../commands/performance/perf_bpftrace.md).

## bpftrace (eBPF)

```bash
# count openat() per process for a while
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { @[comm] = count(); }'
```

Same visibility as strace, low overhead, system-wide — the production-safe way to trace.

## /proc for free

```bash
ls -l /proc/<pid>/fd/         # open file descriptors
cat  /proc/<pid>/maps         # mapped libraries and memory regions
```

## Practice (roadmap exercises)

1. `strace -c ls /tmp` — top syscall by count and time.
2. `strace -fp <pid> -e openat 2>&1 | head -50` on a running web server — what files it opens.
3. `perf top` under load — the hottest kernel function.
4. Generate a flame graph for a CPU-heavy workload (Brendan Gregg's FlameGraph).
5. `bpftrace` one-liner: count `openat()` per process for 10s.

**Mini-project:** "mystery binary analysis" — take any binary, use `strace`, `ltrace`, and `/proc/<pid>/` to write a one-page report: what files it opens, what syscalls it makes, what libs it loads.

**Self-check:** When does strace slow a program down, and how much? What's a flame graph showing on the X axis vs Y axis?

## Related

- Concept: [tracing & profiling](../concepts/performance/tracing.md), [system calls](../concepts/system-internals/system_calls.md)
- Commands: [`strace`, `ltrace`](../commands/performance/strace_ltrace.md), [`perf`, `bpftrace`](../commands/performance/perf_bpftrace.md), [`lsof`](../commands/performance/lsof.md)
