# `iostat` and `iotop`

Disk I/O diagnostics. `iostat` = per-**device** stats; `iotop` = per-**process** I/O. See [disk I/O](../../concepts/performance/disk_io.md).

`iostat` is from `sysstat`; `iotop` needs root.

## `iostat` — Per-Device

```bash
iostat -xz 1         # extended (-x), skip idle devices (-z), each second
iostat -xz 1 5       # 5 samples
```

Key columns:

| Column | Meaning | Worry when |
|:-------|:--------|:-----------|
| `%util` | % time the device had I/O in flight | ~100% (but unreliable on NVMe) |
| `await` | Avg ms per I/O **including queue wait** | Climbing above normal → the real signal |
| `r/s`, `w/s` | IOPS (operations/sec) | — |
| `rkB/s`, `wkB/s` | Throughput | — |
| `aqu-sz` | Avg queue depth | Growing → **saturation** |

**Trust `await` and `aqu-sz` over `%util`**, especially on SSD/NVMe where `%util` saturates well before the device does.

## `iotop` — Per-Process

Answers "*which* process is hammering the disk?":

```bash
sudo iotop -oP       # only procs doing I/O (-o), per-process (-P)
sudo iotop -oPa      # accumulated totals (-a)
```

Also useful: `pidstat -d 1` (see [pidstat](./vmstat_mpstat_pidstat.md)).

## Typical Flow

```bash
iostat -xz 1         # 1. is a disk saturated? (await/aqu-sz high?)
sudo iotop -oP       # 2. who's doing it?
sudo lsof +D /var    # 3. what files? (see lsof.md)
```

## I/O Scheduler

```bash
cat /sys/block/sda/queue/scheduler        # active scheduler in [ ]
```

See [disk I/O](../../concepts/performance/disk_io.md) for `none` vs `mq-deadline` vs `bfq`.

## Related

- Concept: [disk I/O](../../concepts/performance/disk_io.md)
- [`lsof`](./lsof.md), [`vmstat`, `pidstat`](./vmstat_mpstat_pidstat.md), [`sar`, `dstat`](./sar_dstat.md)
