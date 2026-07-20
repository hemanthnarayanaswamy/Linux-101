# Network Performance

**Roadmap: Week 38.** Measure throughput and latency, and diagnose connection issues at multiple layers.

See the concept note: [network performance](../concepts/performance/network_performance.md). Builds on [TCP](../concepts/networking/tcp.md).

## The Four Numbers

| Number | Tool |
|:-------|:-----|
| Throughput | [`iperf3`](../commands/performance/iperf3_tc.md) |
| Latency (RTT) | [`ping`](../commands/network-diagnostics/ping.md), [`mtr`](../commands/network-diagnostics/mtr.md) |
| Loss rate | `mtr`, `tc`-injected |
| Retransmits | `nstat`, `ss -i` |

## Loss vs Latency

**Loss is the killer.** TCP treats a dropped packet as congestion and shrinks its congestion window (`cwnd`), throttling hard. On long (high-RTT) links, each loss costs a full round-trip to recover and cwnd grows back slowly — so even 1% loss can collapse throughput far more than the same added latency. (Week 38 self-check.)

## Measuring

```bash
iperf3 -s                        # server VM
iperf3 -c <server-ip>            # TCP throughput
iperf3 -c <server-ip> -u -b 100M # UDP: also reports loss + jitter

ss -tin                          # per-socket rtt, cwnd, retrans
nstat -az | grep -i retrans      # TCP retransmit counters
```

See [`iperf3`, `tc`, `nstat`](../commands/performance/iperf3_tc.md) and [`ss`](../commands/network-interfaces/ss.md).

## Simulate a Bad Link (tc netem)

Prove a diagnosis by injecting impairment and watching metrics move:

```bash
sudo tc qdisc add dev eth0 root netem delay 100ms loss 1%   # 100ms + 1% loss
# ...re-run iperf3 — watch the throughput cliff...
sudo tc qdisc del dev eth0 root                             # cleanup
```

## Capture

```bash
sudo tcpdump -i any -A 'tcp port 80'      # read an HTTP request
```

See [`tcpdump`](../commands/network-diagnostics/tcpdump.md).

## Practice (roadmap exercises)

1. `iperf3` between two VMs; note throughput; repeat with `-u` (UDP).
2. `nstat | grep -i retrans`; trigger retransmits by adding latency/loss with `tc`.
3. `tc qdisc add dev eth0 root netem delay 100ms loss 1%`; re-run iperf3; observe the cliff.
4. `tcpdump -i any -A 'tcp port 80'`; read a captured HTTP request.
5. `ss -tin` during a transfer; look at `cwnd`, `rtt`, `retrans`.

**Mini-project:** a "network forensics report" for a deliberately bad connection (use `tc`): identify latency, loss rate, throughput, retransmit count, and the probable cause (loss vs latency vs congestion).

**Self-check:** What does TCP `cwnd` represent? Why does packet loss kill throughput more than added latency on long links?

## Related

- Concept: [network performance](../concepts/performance/network_performance.md), [TCP](../concepts/networking/tcp.md)
- Commands: [`iperf3`, `tc`, `nstat`](../commands/performance/iperf3_tc.md), [`ss`](../commands/network-interfaces/ss.md), [`mtr`](../commands/network-diagnostics/mtr.md), [`tcpdump`](../commands/network-diagnostics/tcpdump.md)
