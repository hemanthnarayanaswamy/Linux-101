# Disk I/O

**Roadmap: Week 37.** Diagnose disk-bound systems and understand the I/O stack from app down to the block layer.

See the concept note: [disk I/O](../concepts/performance/disk_io.md).

## iostat -xz 1

The per-device view. Key columns:

| Column | Meaning |
|:-------|:--------|
| `%util` | % time device had I/O in flight (unreliable on NVMe) |
| `await` | Avg ms per I/O **including queue wait** — the real signal |
| `r/s`, `w/s` | IOPS |
| `rkB/s`, `wkB/s` | Throughput |
| `aqu-sz` | Avg queue depth → saturation |

**`await` climbing** = requests piling up = the app feels it. Trust `await`/`aqu-sz` over `%util` on SSD/NVMe. See [`iostat`, `iotop`](../commands/performance/iostat_iotop.md).

## Who's Doing the I/O?

`%util` says the disk is busy; it doesn't say who:

```bash
sudo iotop -oP           # processes actually doing I/O
pidstat -d 1             # per-process read/write kB/s
sudo lsof +D /var        # what files are open under a path
```

See [`lsof`](../commands/performance/lsof.md).

## The Block Layer & Schedulers

```bash
cat /sys/block/sda/queue/scheduler       # active scheduler in [ ]
echo none | sudo tee /sys/block/sda/queue/scheduler   # change (temporary)
```

- `none` — fast NVMe/SSD; least overhead.
- `mq-deadline` — general default; bounded latency.
- `bfq` — desktop fairness, more overhead.

## Don't Guess — Prove It

A "stuck" process could be CPU, memory, disk, or network. Follow the [USE method](../concepts/performance/use_method.md): high `%wa` → confirm with `iostat`/`iotop` before blaming the disk (a process blocked on a slow NFS mount also looks blocked).

## Practice (roadmap exercises)

1. `iostat -xz 1` idle, then `dd if=/dev/zero of=/tmp/big bs=1M count=2000`; compare.
2. `iotop -oP` — find the top I/O process right now.
3. `lsof +D /var | head` — open files under /var.
4. `cat /sys/block/sda/queue/scheduler`; change it temporarily.
5. Diagnose a "stuck" process — CPU, memory, disk, or network? Use the methodology.

**Mini-project:** an "I/O-bound failure simulation" — a disk-thrashing background process degrading a foreground service; use the tools to identify and prove the culprit; write the postmortem.

**Self-check:** What's `await` and when should you worry? Practical difference between `mq-deadline` and `none`?

## Related

- Concept: [disk I/O](../concepts/performance/disk_io.md), [filesystems](../concepts/storage/filesystems.md)
- Commands: [`iostat`, `iotop`](../commands/performance/iostat_iotop.md), [`lsof`](../commands/performance/lsof.md), [`sar`, `dstat`](../commands/performance/sar_dstat.md)
