# `perf` and `bpftrace`

CPU profiling and modern kernel tracing. `perf` samples where CPU time goes; `bpftrace` runs low-overhead eBPF probes. See [tracing & profiling](../../concepts/performance/tracing.md).

## `perf` — CPU Profiler

Samples the CPU thousands of times a second to build a statistical picture of the hottest code (kernel + userspace).

```bash
sudo perf top                       # live: hottest functions right now
sudo perf record -g -- ./myapp      # record with call graphs (-g)
sudo perf record -g -p <pid> sleep 30   # profile a running process for 30s
sudo perf report                    # browse the recorded profile
sudo perf stat ./myapp              # counters: cycles, instructions, cache misses, IPC
```

`perf top` while the system is loaded points straight at the hottest function (often a kernel path when the bottleneck is syscalls/locking).

### Flame Graphs

Turn a `perf record` into a flame graph (Brendan Gregg's [FlameGraph](https://github.com/brendangregg/FlameGraph) scripts):

```bash
sudo perf record -F 99 -g -- ./myapp
sudo perf script | stackcollapse-perf.pl | flamegraph.pl > flame.svg
```

Read it: **X axis = fraction of samples (width = CPU spent), not time; Y axis = stack depth (caller → callee).** Scan for the **widest** boxes.

## `bpftrace` — eBPF One-Liners

Runs small sandboxed programs **inside the kernel** — same visibility as strace but low-overhead and system-wide, safe for production.

```bash
# every filename opened, system-wide
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { printf("%s %s\n", comm, str(args->filename)); }'

# count openat() per process over 10s (Ctrl-C to print)
sudo bpftrace -e 'tracepoint:syscalls:sys_enter_openat { @[comm] = count(); }'

# histogram of read() sizes
sudo bpftrace -e 'tracepoint:syscalls:sys_exit_read { @ = hist(args->ret); }'
```

`bpftrace -l` lists available probes. The `bcc` toolkit ships many ready-made tools (`opensnoop`, `execsnoop`, `biolatency`).

## perf vs strace vs bpftrace

- **strace** — full syscall detail, high overhead, one process.
- **perf** — where CPU time goes (sampling), low overhead.
- **bpftrace** — custom events anywhere in the kernel, very low overhead, system-wide.

## Related

- Concept: [tracing & profiling](../../concepts/performance/tracing.md)
- [`strace`, `ltrace`](./strace_ltrace.md), [`lsof`](./lsof.md)
