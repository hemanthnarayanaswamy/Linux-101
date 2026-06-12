# Load Average

Load average shows system demand over time.

You can see it with:

```bash
uptime
cat /proc/loadavg
top
```

Example:

```text
load average: 2.35, 1.80, 1.20
```

These are the 1-minute, 5-minute, and 15-minute averages.

## What It Counts

On Linux, load average counts tasks that are:

```text
R = running or waiting for CPU
D = uninterruptible sleep, usually waiting on I/O
```

So high load can mean CPU pressure or blocked I/O.

## Compare To CPU Cores

Check cores:

```bash
nproc
```

Rule of thumb:

| Example | Meaning |
|:--------|:--------|
| Load `1.00` on 1 core | Fully used |
| Load `1.00` on 4 cores | Light |
| Load `4.00` on 4 cores | Fully used |
| Load `8.00` on 4 cores | Overloaded |

## Interpret Trend

```text
1 min > 5 min > 15 min = rising load
1 min < 5 min < 15 min = recovering load
all similar = steady load
```

## SRE/DevOps Rule

Load average tells you demand, not the cause.

Use these next:

```bash
top
vmstat 1
iostat -xz 1
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
```

High load with low CPU often points to disk, NFS, or other I/O waits.
