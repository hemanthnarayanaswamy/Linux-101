# Disk I/O Performance

Diagnosing disk-bound systems and understanding the I/O stack from application down to the block layer.

## Reading `iostat -xz 1`

The key per-device columns:

| Field | Meaning | Worry when |
|:------|:--------|:-----------|
| `%util` | % of time the device had I/O in flight | Near 100% = device busy (but see note) |
| `await` | Avg time (ms) per I/O, **including queue wait** | Rising well above device's normal service time |
| `r/s`, `w/s` | Read / write **operations** per sec (IOPS) | — |
| `rkB/s`, `wkB/s` | Read / write **throughput** | — |
| `aqu-sz` | Avg queue depth | Growing queue = **saturation** |

**`await` is the number to watch.** It's what the *application* actually feels — total latency per request including time spent waiting in the queue. A disk at 100% `%util` with low `await` is busy-but-keeping-up; high `await` means requests are piling up.

> On modern SSDs/NVMe that handle many I/Os in parallel, `%util` can read ~100% while the device is far from its limit — trust `await` and `aqu-sz` over `%util` there.

## Per-Process I/O

`%util` tells you the disk is busy; it doesn't say **who**. For that:

```bash
iotop -oP            # processes actually doing I/O (-o), per-process (-P)
pidstat -d 1         # per-process read/write kB/s each second
lsof +D /var         # what has files open under a path
```

## The Block Layer & Schedulers

Between the filesystem and the device sits the **I/O scheduler**, which orders/merges requests:

```bash
cat /sys/block/sda/queue/scheduler        # [] marks the active one
echo mq-deadline | sudo tee /sys/block/sda/queue/scheduler   # change (temporary)
```

| Scheduler | Use |
|:----------|:----|
| `none` | Fast NVMe/SSD — the device reorders better than the kernel can; least overhead |
| `mq-deadline` | General default; caps latency by deadline, light and predictable |
| `bfq` | Desktop/interactive — fairness between processes, more overhead |

Practical difference: `none` gets out of the way on fast flash; `mq-deadline` adds fair, bounded-latency ordering for mixed workloads.

## Is It Really Disk?

High iowait (`%wa` in [cpu_memory](./cpu_memory.md)) points at I/O, but confirm with `iostat`/`iotop` before blaming the disk — a process blocked on a slow **network** filesystem also shows as blocked. Follow the [USE method](./use_method.md): check the resource, don't guess.

## Related

- Concepts: [USE method](./use_method.md), [CPU & memory](./cpu_memory.md), [filesystems](../storage/filesystems.md)
- Commands: [`iostat`, `iotop`](../../commands/performance/iostat_iotop.md), [`lsof`](../../commands/performance/lsof.md), [`sar`, `dstat`](../../commands/performance/sar_dstat.md)
