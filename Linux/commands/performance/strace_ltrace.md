# `strace` and `ltrace`

Tracing what a program actually does: `strace` shows **system calls** (kernel interactions), `ltrace` shows **library calls**. See [tracing & profiling](../../concepts/performance/tracing.md) and [system calls](../../concepts/system-internals/system_calls.md).

## `strace` — System Calls

```bash
strace ls /tmp                 # trace a command from the start
strace -p <pid>                # attach to a running process
strace -f -p <pid>             # follow child processes/threads too
strace -c ls /tmp              # SUMMARY: count + time per syscall
strace -e trace=openat ls      # only these syscalls
strace -e trace=network -p <pid>   # only network syscalls
strace -o out.txt -p <pid>     # write to a file
```

**Start with `-c`** for a cheap overview of which syscalls dominate, then drill into specific ones with `-e`.

### The classic use — "why won't it start?"

```bash
strace -f -e openat myapp 2>&1 | grep -i -E 'enoent|error'
```

Shows every file it tried to open and the `ENOENT` on the missing config/library.

### Cost

`strace` traps to the kernel on **every** syscall, so a syscall-heavy program can slow down **10–100×**. Fine for diagnosis; never leave it attached to a production hot path. For low-overhead tracing use [`bpftrace`](./perf_bpftrace.md).

## `ltrace` — Library Calls

Traces calls into shared libraries (e.g. libc `malloc`, `strcmp`) rather than syscalls:

```bash
ltrace ls /tmp
ltrace -c ls /tmp              # summary per library function
ltrace -p <pid>
```

Use `ltrace` when the interesting behavior is *inside* userspace library logic; use `strace` when it's about files, sockets, or kernel interaction.

## Poke at /proc Instead

Sometimes you don't need to trace — just look:

```bash
ls -l /proc/<pid>/fd/          # what files/sockets are open
cat  /proc/<pid>/maps          # loaded libraries and memory regions
```

## Related

- Concepts: [tracing & profiling](../../concepts/performance/tracing.md), [system calls](../../concepts/system-internals/system_calls.md)
- [`perf`, `bpftrace`](./perf_bpftrace.md), [`lsof`](./lsof.md)
