# The `traceroute` command in Linux

The [traceroute](https://linuxize.com/post/traceroute-command-in-linux/) command is a network diagnostic tool that displays the path packets take from your system to a destination host. It shows each hop (router) along the route and the time it takes for packets to reach each one.

Network administrators use traceroute to identify where packets are being delayed or dropped, making it essential for troubleshooting connectivity issues, latency problems, and routing failures.

```bash
sudo apt update && sudo apt install traceroute

traceroute [OPTIONS] DESTINATION

# options flags that modify the behavior of the command
# DESTINATION the target hostname or IP address to trace
```

## How `traceroute` works

When you run `traceroute`, it sends packets with incrementally increasing TTL (Time to Live) values, starting at 1. Each router along the path decrements the TTL by 1. When the TTL reaches 0, the router discards the packet and sends back an ICMP “Time Exceeded” message.

By increasing the TTL with each round of packets, traceroute discovers each hop along the route until the packets reach the final destination.

By default, traceroute sends three UDP packets per hop (on Linux) and displays the round-trip time for each packet.

## Basic Usage

To trace the route to a destination, run `traceroute` followed by the hostname or IP address:

```bash
traceroute google.com

# traceroute to google.com (142.250.185.78), 30 hops max, 60 byte packets
#  1  router.local (192.168.1.1)  1.234 ms  1.102 ms  1.056 ms
#  2  10.0.0.1 (10.0.0.1)  12.345 ms  12.234 ms  12.123 ms
#  3  isp-gateway.example.net (203.0.113.1)  15.678 ms  15.567 ms  15.456 ms
#  4  core-router.example.net (198.51.100.1)  20.123 ms  20.012 ms  19.901 ms
#  5  google-peer.example.net (192.0.2.1)  22.345 ms  22.234 ms  22.123 ms
#  6  142.250.185.78 (142.250.185.78)  25.678 ms  25.567 ms  25.456 ms
```

##### Understanding the output

Each line in the `traceroute` output represents a hop along the route. Let us break down what each field means:

- **Hop number** - The sequential number of the router in the path (1, 2, 3, etc.).
- **Hostname** - The DNS name of the router, if available.
- **IP address** - The IP address of the router in parentheses.
- **Round-trip times** - Three time measurements in milliseconds, one for each probe packet sent to that hop.

The first line shows the destination, maximum number of hops (default 30), and packet size (default 60 bytes).

_Asterisks (_ \* _)_ indicate that no response was received for that hop. This can happen when:

- The router is configured to not respond to traceroute probes.
- A firewall is blocking the packets.
- The packets were lost due to network congestion.
- Increasing latency at a specific hop suggests a bottleneck or congested link at that point in the network.

Consistent high latency from a certain hop onward indicates the issue is at or before that router.

1. Skip DNS Resolution

```bash
traceroute -n google.com
# The output shows numeric IP addresses without hostnames:

# traceroute to google.com (142.250.185.78), 30 hops max, 60 byte packets
#  1  192.168.1.1  1.234 ms  1.102 ms  1.056 ms
#  2  10.0.0.1  12.345 ms  12.234 ms  12.123 ms
#  3  203.0.113.1  15.678 ms  15.567 ms  15.456 ms

# This is useful when DNS resolution is slow or when you only need IP addresses.
```

2. Change Maximum Hops
   By default, traceroute stops after **30** hops. To change this limit, use the `-m` option:

```bash
traceroute -m 15 google.com
```

3. Change Number of Probes
   To send a different number of probe packets per hop, use the `-q` option:

```bash
traceroute -q 1 google.com
```

4. Use ICMP instead of UDP
   By default, Linux traceroute uses UDP packets. Some networks block UDP, so you can use ICMP ECHO packets instead:

```bash
sudo traceroute -I google.com
```

- Use TCP instead of UDP

```bash
sudo traceroute -T google.com
# You can also specify a port, such as port 443 for HTTPS
sudo traceroute -T -p 443 google.com
```

5. Specify Source Interface
   If your system has multiple network interfaces, you can specify which one to use:

```bash
traceroute -i eth0 google.com
```

Or specify the Source IP address

```bash
traceroute -s 192.168.1.100 google.com
```

## Practical Usecase Examples

1. **Diagnose Slow Connections**
   If a website is loading slowly, trace the route to identify where the delay occurs:

```bash
traceroute -n example.com
```

Look for hops with significantly higher latency than the previous ones. The hop before the latency spike is often the source of the problem.

2. **Check if a Host is Reachable**
   If `ping` shows packet loss, use traceroute to find where packets are being dropped:

Hops showing `* * *` followed by successful hops indicate a router that does not respond to probes but forwards traffic. If all remaining hops show \* \* \*, the issue is at or after the last responding hop.

3. **Trace Through a Firewall**
   If standard UDP probes are blocked, try _ICMP or TCP_:

```bash
sudo traceroute -I google.com
sudo traceroute -T -p 80 google.com
```

4. **Compare Routes to Different Servers**
   To understand routing differences, trace routes to multiple servers.

```bash
traceroute -n server1.example.com
traceroute -n server2.example.com
```

This helps identify whether traffic to different destinations takes different paths through your network.
