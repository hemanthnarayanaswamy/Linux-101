# Tracing & Profiling (strace, perf, eBPF)

When metrics tell you *which* resource is the problem but not *why*, tracing tools show what a program is actually doing. Builds on [system calls](../system-internals/system_calls.md).

## The Ladder of Tools

| Tool | Observes | Overhead |
|:-----|:---------|:---------|
| `strace` | **System calls** a process makes | High (stops the process on every syscall) |
| `ltrace` | **Library calls** (e.g. libc) | High |
| `perf` | **CPU sampling** — where CPU time goes, kernel + user | Low (sampling) |
| `bpftrace` / eBPF | Custom **in-kernel** probes on almost anything | Very low |

## strace — Syscall Tracing

Every interaction with the kernel (open files, read/write, network) is a syscall. `strace` shows them.

```bash
strace -p <pid>                 # attach to a running process
strace -f -e openat ls /tmp     # follow forks, only openat calls
strace -c ls /tmp               # SUMMARY: count + time per syscall (start here)
```

> **strace is slow** — it traps into the kernel on *every* syscall, so a syscall-heavy program can run **orders of magnitude** slower under strace. Great for diagnosis, never leave it attached to production. Use `-c` for a cheap summary first.

Common use: "why can't it find its config?" → `strace -f -e openat` shows every path it tries and the `ENOENT` on the one it can't find.

## perf — CPU Profiling

Samples the CPU many times a second to build a statistical picture of where time goes.

```bash
sudo perf top                   # live: hottest functions right now
sudo perf record -g -- ./myapp  # record with call graphs
sudo perf report                # browse the recorded profile
```

### Flame Graphs

A flame graph visualizes a `perf record` profile (Brendan Gregg):

- **X axis = population/breadth**, *not time* — the width of a box is the fraction of samples in that function (wider = more CPU).
- **Y axis = stack depth** — boxes stacked on top are callers → callees.

You read it by scanning for the **widest boxes** (or plateaus): those are where the CPU actually spends its time.

## bpftrace / eBPF

eBPF runs small sandboxed programs **inside the kernel** to observe events with tiny overhead. `bpftrace` is the one-liner front end.

```bash
# print every filename opened, system-wide
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s\n", str(args->filename)); }'

# count openat() per process for a while
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { @[comm] = count(); }'
```

Same visibility as strace but low-overhead and system-wide — the modern way to trace in production.

## Poking at /proc

No tools needed to inspect a running process:

```bash
ls -l /proc/<pid>/fd/        # open file descriptors
cat  /proc/<pid>/maps        # memory-mapped regions (libs, heap, stack)
cat  /proc/<pid>/status      # RSS, swap, threads, state
```

## Related

- Concept: [system calls](../system-internals/system_calls.md), [USE method](./use_method.md)
- Commands: [`strace`, `ltrace`](../../commands/performance/strace_ltrace.md), [`perf`, `bpftrace`](../../commands/performance/perf_bpftrace.md), [`lsof`](../../commands/performance/lsof.md)
