# The `stress-ng` Command

`stress-ng` deliberately loads a system — CPU, memory, I/O, network — so you can practice diagnosis against a *known* cause. Essential for the Phase 6 exercises: apply a load, then confirm your tools detect it.

```bash
sudo apt install stress-ng
```

## Load One Resource at a Time

```bash
# CPU: 4 workers spinning, for 60s
stress-ng --cpu 4 --timeout 60s

# Memory: 2 workers each allocating 1GB
stress-ng --vm 2 --vm-bytes 1G --timeout 60s

# Push toward OOM: one worker taking 90% of RAM
stress-ng --vm 1 --vm-bytes 90% --timeout 60s

# Disk I/O: 4 workers doing I/O
stress-ng --io 4 --timeout 60s

# Combined + a metrics summary at the end
stress-ng --cpu 2 --vm 1 --vm-bytes 512M --timeout 30s --metrics-brief
```

## The Point: Load → Observe

The workflow the roadmap drills:

```bash
# terminal 1 — apply a known load
stress-ng --cpu 4 --timeout 60s

# terminal 2 — confirm the tools show it
vmstat 1          # r (run queue) climbs above #CPUs
mpstat -P ALL 1   # cores go to 100%
pidstat -u 1      # stress-ng workers top the list
```

Do the same for `--vm` (watch `free`, `si`/`so`, then the OOM kill in `dmesg`) and `--io` (watch `iostat -xz 1`).

## Related

- Concepts: [USE method](../../concepts/performance/use_method.md), [CPU & memory](../../concepts/performance/cpu_memory.md)
- [`vmstat`, `mpstat`, `pidstat`](./vmstat_mpstat_pidstat.md), [`free`](./free.md), [`iostat`, `iotop`](./iostat_iotop.md), [`iperf3`, `tc`](./iperf3_tc.md) (network load)
