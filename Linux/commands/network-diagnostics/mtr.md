# `mtr` Command in Linux

The `mtr` (My Traceroute) command in Linux is a network diagnostic tool that combines the functionality of ping and traceroute into a single utility, providing real-time statistics on packet loss and latency for each hop between the source and destination. It continuously probes the network path using low Time-To-Live (TTL) values, allowing administrators to identify intermittent issues, bottlenecks, or routing problems that static tools might miss.

```bash
ping + traceroute
```

## Usage

```bash
mtr <host> # This opens an interactive live view

mtr -rwzc 100 google.com # Most useful commands with option

# -r Report mode
# -w Wide output
# -z Show AS number info, if available
# -c 100 Send 100 packets

HOST: server01                 Loss%   Snt   Last   Avg  Best  Wrst StDev
  1. 192.168.1.1                0.0%   100    1.2   1.5   1.0   3.0   0.4
  2. 10.10.0.1                  0.0%   100    4.5   5.1   4.0   8.2   0.9
  3. 203.0.113.1                0.0%   100   15.0  16.2  14.8  25.1   2.1
  4. 8.8.8.8                    0.0%   100   20.4  21.0  19.5  30.0   2.5
```

- `Loss%` packet loss percentage
- `Snt` packets sent
- `Last` Last packet latency
- `Avg` Average latency
- `Best` Lowest latency
- `Wrst` Hightest latency
- `StDev` Latency variation

## How to Read Output

- Good Output

```bash
Loss% is 0.0% at final destination
Latency is stable
No major jumps
```

This means the network path is healthy.

- Real packet loss
  If packet loss starts at one hop and continues to all later hops:

```bash
Hop 3: 20% loss
Hop 4: 20% loss
Hop 5: 20% loss
Destination: 20% loss
```

This usually means real packet loss starts around hop 3.

- Not Real Packet loss
  If one middle hop shows packet loss, but later hops are fine:

```bash
Hop 3: 80% loss
Hop 4: 0% loss
Destination: 0% loss
```

This usually means the router is deprioritizing ICMP replies. It does not mean traffic is actually dropping.

> Packet loss matters only if it continues to the final destination or later hops.
