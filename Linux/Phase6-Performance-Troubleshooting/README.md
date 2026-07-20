# Phase 6 - Performance & Troubleshooting

This is Phase 7 (Performance & Troubleshooting) of the roadmap, kept as Phase 6 in this repo because the roadmap's Phase 2 (Shell Scripting & Automation) was skipped.

**Phase outcome:** systematically diagnose any "the server is slow" complaint and pinpoint the cause.

Primary roadmap sources:

- [linux-mastery-roadmap.md](../../resources/roadMap/linux-mastery-roadmap.md)
- [linux-mastery-roadmap-detailed.md](../../resources/roadMap/linux-mastery-roadmap-detailed.md)

## Quick Index

| Order | Topic | Main Note |
|---:|:------|:----------|
| 1 | Performance methodology (USE method) | [1_performance_methodology.md](./1_performance_methodology.md) |
| 2 | CPU & memory | [2_cpu_memory.md](./2_cpu_memory.md) |
| 3 | Disk I/O | [3_disk_io.md](./3_disk_io.md) |
| 4 | Network performance | [4_network_performance.md](./4_network_performance.md) |
| 5 | strace, perf, eBPF | [5_strace_perf_ebpf.md](./5_strace_perf_ebpf.md) |

## Reference Indexes

| Area | Index |
|:-----|:------|
| Commands | [Linux commands index](../commands/README.md) |
| Concepts | [Linux concepts index](../concepts/README.md) |

---

## 1. Performance Methodology

Brendan Gregg's USE method (Utilization, Saturation, Errors), the 60-second checklist, and establishing a baseline.

Main note:

- [1_performance_methodology.md](./1_performance_methodology.md)

Concepts:

- [USE method & methodology](../concepts/performance/use_method.md)

Commands:

- [`vmstat`, `mpstat`, `pidstat`](../commands/performance/vmstat_mpstat_pidstat.md)
- [`stress-ng`](../commands/performance/stress-ng.md)
- [`top`, `htop`](../commands/system-monitoring/top_htop.md)
- [`uptime`](../commands/system-monitoring/uptime.md)

---

## 2. CPU & Memory

top/htop columns, `vmstat`, RSS vs VSZ, `/proc/meminfo`, swap, and the OOM killer.

Main note:

- [2_cpu_memory.md](./2_cpu_memory.md)

Concepts:

- [CPU & memory](../concepts/performance/cpu_memory.md)
- [CPU](../concepts/cpu.md), [system load](../concepts/system_load.md)

Commands:

- [`vmstat`, `mpstat`, `pidstat`](../commands/performance/vmstat_mpstat_pidstat.md)
- [`free`](../commands/performance/free.md)
- [`top`, `htop`](../commands/system-monitoring/top_htop.md)
- [`dmesg`](../commands/logging/dmesg.md)

---

## 3. Disk I/O

`iostat -xz`, `iotop`, per-process I/O, the block layer, and I/O schedulers.

Main note:

- [3_disk_io.md](./3_disk_io.md)

Concepts:

- [Disk I/O](../concepts/performance/disk_io.md)

Commands:

- [`iostat`, `iotop`](../commands/performance/iostat_iotop.md)
- [`lsof`](../commands/performance/lsof.md)
- [`sar`, `dstat`](../commands/performance/sar_dstat.md)

---

## 4. Network Performance

Throughput and latency, loss vs latency, TCP cwnd/RTT/retransmits, and simulating bad links with `tc`.

Main note:

- [4_network_performance.md](./4_network_performance.md)

Concepts:

- [Network performance](../concepts/performance/network_performance.md)
- [TCP](../concepts/networking/tcp.md)

Commands:

- [`iperf3`, `tc`, `nstat`](../commands/performance/iperf3_tc.md)
- [`ss`](../commands/network-interfaces/ss.md)
- [`mtr`](../commands/network-diagnostics/mtr.md), [`tcpdump`](../commands/network-diagnostics/tcpdump.md)

---

## 5. strace, perf, eBPF

Syscall tracing, library tracing, CPU profiling with perf and flame graphs, and eBPF/`bpftrace`.

Main note:

- [5_strace_perf_ebpf.md](./5_strace_perf_ebpf.md)

Concepts:

- [Tracing & profiling](../concepts/performance/tracing.md)
- [System calls](../concepts/system-internals/system_calls.md)

Commands:

- [`strace`, `ltrace`](../commands/performance/strace_ltrace.md)
- [`perf`, `bpftrace`](../commands/performance/perf_bpftrace.md)
- [`lsof`](../commands/performance/lsof.md)

---

## Phase Capstone — "Performance War Room Exercise"

Set up a lab VM running nginx + a simple web app, with a load generator (siege/wrk/`ab`) hitting it from another VM. Inject one deliberate problem at a time (rotate): CPU starvation, memory leak, slow disk, packet loss.

For each problem:

1. Reproduce.
2. Diagnose using the methodology (§1).
3. Identify root cause.
4. Fix it.
5. Write a postmortem (timeline, root cause, fix, prevention).

Deliverable: 4 postmortems in a public repo — these look great in interviews.
