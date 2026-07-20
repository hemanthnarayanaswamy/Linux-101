# The `lsof` Command

`lsof` = **list open files**. On Linux almost everything is a file (regular files, sockets, pipes, devices), so `lsof` answers a huge range of "what's using this?" questions. See [disk I/O](../../concepts/performance/disk_io.md) and [tracing](../../concepts/performance/tracing.md).

## By File / Directory

```bash
lsof /var/log/syslog        # who has this exact file open
lsof +D /var/log            # everything open under a directory (recursive)
lsof /dev/sda1              # who's using a device
```

## By Process

```bash
lsof -p <pid>               # every file/socket a process has open
lsof -c nginx               # by command name
```

## Network Sockets

```bash
lsof -i                      # all network connections
lsof -i :443                 # who's using port 443
lsof -i TCP:22 -sTCP:LISTEN  # what's listening on TCP/22
lsof -iUDP                   # UDP sockets
```

(For pure socket work `ss` is faster — see [`ss`](../network-interfaces/ss.md) — but `lsof -i` ties a socket to its file/process context.)

## Two Classic Uses

**"Device is busy, can't unmount":**

```bash
lsof +D /mnt/data           # find who's holding files open under the mount
```

**"Disk full but `df` and `du` disagree"** — a deleted file still held open by a process keeps consuming space until the process closes it:

```bash
lsof +L1                    # open files with link count 0 = deleted-but-open
lsof | grep deleted
```

## Related

- Concepts: [disk I/O](../../concepts/performance/disk_io.md), [tracing](../../concepts/performance/tracing.md)
- [`iostat`, `iotop`](./iostat_iotop.md), [`ss`](../network-interfaces/ss.md), [`file`, `stat`, `du`, `df`](../file-metadata/file_stat_du_df.md)
