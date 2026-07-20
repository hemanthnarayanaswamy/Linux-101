# `iperf3`, `tc`, `nstat`

Network performance tools: `iperf3` measures throughput, `tc` **injects** impairment to reproduce bad links, `nstat` reads protocol counters. See [network performance](../../concepts/performance/network_performance.md).

## `iperf3` — Throughput

Client/server bandwidth test between two hosts.

```bash
# on the server VM
iperf3 -s

# on the client VM
iperf3 -c <server-ip>            # TCP throughput
iperf3 -c <server-ip> -u -b 100M # UDP at 100 Mbit (also reports loss/jitter)
iperf3 -c <server-ip> -R         # reverse (download) direction
iperf3 -c <server-ip> -P 4       # 4 parallel streams
```

TCP measures achievable bandwidth; UDP (`-u`) additionally reports **loss and jitter** since it won't retransmit.

## `tc` — Traffic Control / netem

Inject latency, loss, and rate limits so you can *prove* a diagnosis by watching metrics change.

```bash
# add 100ms delay + 1% loss on eth0
sudo tc qdisc add dev eth0 root netem delay 100ms loss 1%

# ...re-run iperf3 — watch throughput fall off a cliff (loss kills TCP)...

sudo tc qdisc show dev eth0        # inspect
sudo tc qdisc del dev eth0 root    # remove ALL impairment (cleanup)
```

Other netem knobs: `delay 100ms 20ms` (jitter), `duplicate 1%`, `corrupt 0.1%`, `rate 1mbit`.

> ⚠️ `tc` affects real traffic on that interface — use it on a lab VM, and remember to `del` it (a forgotten rule looks like a mysterious network problem later).

## `nstat` — Protocol Counters

```bash
nstat                    # counters changed since last call (great for deltas)
nstat -az | grep -i retrans   # TCP retransmits
cat /proc/net/snmp       # raw SNMP-style counters
```

Rising retransmits confirm packet loss is hurting a TCP connection.

## Socket-Level Detail

```bash
ss -tin                  # per-socket rtt, cwnd, retrans (see ss.md)
```

## Related

- Concept: [network performance](../../concepts/performance/network_performance.md)
- [`ss`](../network-interfaces/ss.md), [`mtr`](../network-diagnostics/mtr.md), [`tcpdump`](../network-diagnostics/tcpdump.md), [`traceroute`](../network-diagnostics/traceroute.md)
