# Network Performance

Measuring throughput and latency, and diagnosing connection issues at multiple layers. Builds on [TCP](../networking/tcp.md) and [UDP](../networking/udp.md).

## The Four Numbers

Any "network is slow" investigation comes down to:

| Number | Tool | Meaning |
|:-------|:-----|:--------|
| **Throughput** | `iperf3` | Bandwidth actually achievable end to end |
| **Latency (RTT)** | `ping`, `mtr` | Round-trip time |
| **Loss rate** | `mtr`, `tc`-injected | % packets dropped along the path |
| **Retransmits** | `nstat`, `ss -i` | TCP segments resent (a consequence of loss) |

## Latency vs Loss vs Throughput

- **Latency** adds delay but a healthy TCP connection still fills the pipe.
- **Loss** is the killer. TCP treats a dropped packet as congestion and **shrinks its congestion window**, throttling itself hard. On **long (high-RTT) links** this is brutal: every loss event costs a full round-trip to recover, and the window grows back slowly — so even 1% loss can collapse throughput far more than the same added latency would. (Week 38 self-check.)

## TCP cwnd and RTT

- **cwnd (congestion window)** — how much unacknowledged data TCP will keep "in flight." TCP grows it until it sees loss, then backs off. cwnd × (1/RTT) roughly bounds throughput — which is why high RTT *and* loss together cap you.
- Watch them live per-socket:

```bash
ss -tin                    # per-socket: rtt, cwnd, retrans, etc.
ss -i                      # internal TCP info
```

## Protocol Counters

```bash
nstat                      # per-interval kernel net counters (retrans, drops)
nstat | grep -i retrans    # TCP retransmit counters
cat /proc/net/snmp         # raw SNMP-style protocol counters
sar -n DEV 1               # throughput per interface
sar -n TCP,ETCP 1          # TCP-level stats over time
```

## Simulating a Bad Link (`tc netem`)

To *prove* a diagnosis, inject the impairment and watch metrics move:

```bash
sudo tc qdisc add dev eth0 root netem delay 100ms loss 1%   # 100ms + 1% loss
# ...re-run iperf3, watch throughput fall off a cliff...
sudo tc qdisc del dev eth0 root                              # remove it
```

## Capturing Traffic

```bash
sudo tcpdump -i any -A 'tcp port 80'      # read an HTTP request
sudo tcpdump -i any 'tcp[tcpflags] & tcp-syn != 0'   # SYNs (connection attempts)
```

## Related

- Concepts: [TCP](../networking/tcp.md), [UDP](../networking/udp.md), [USE method](./use_method.md)
- Commands: [`iperf3`, `tc`](../../commands/performance/iperf3_tc.md), [`ss`](../../commands/network-interfaces/ss.md), [`mtr`](../../commands/network-diagnostics/mtr.md), [`tcpdump`](../../commands/network-diagnostics/tcpdump.md)
