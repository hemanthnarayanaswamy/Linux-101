# `tcpdump` Command in Linux

`tcpdump` is a command-line utility that you can use to capture and inspect network traffic going to and from your system.

- `tcpdump`, you can also capture non-TCP traffic such as `UDP, ARP, or ICMP`.

```bash
sudo apt update && sudo apt install tcpdump
```

### Capturing Packets with tcpdump

The general syntax for the tcpdump command is as follows:

```bash
tcpdump [options] [expression]
```

- Only root or a user with sudo privileges can run `tcpdump`

```bash
sudo tcpdump

sudo tcpdump -vv
```

`tcpdump` continues to capture packets and write to the standard output until it receives an interrupt signal. Use the `Ctrl+C` key combination to send an interrupt signal and stop the command.

- pass the `-v` option, or `-vv` for even more verbose output

> Start small with a specific interface and `-c` to avoid flodding your terminal.

You can specify the number of packets to be captured using the `-c` option.

```bash
sudo tcpdump -c 10
# After capturing the packets, tcpdump stops
```

When no interface is specified, tcpdump uses the first interface it finds and dumps all packets going through that interface.

Use the `-D` option to print a list of all available network interfaces that tcpdump can collect packets from:

```bash
sudo tcpdump -D

# 1.ens3 [Up, Running]
# 2.any (Pseudo-device that captures on all interfaces) [Up, Running]
# 3.lo [Up, Running, Loopback]
```

The output above shows that `ens3` is the first interface found by tcpdump and used when no interface is provided to the command. The second interface `any` is a special device that allows you to capture all active interfaces.

## Understanding the `tcpdump` output

`tcpdump` outputs information for each captured packet on a new line. Each line includes a timestamp and information about that packet, depending on the protocol.

```bash
[Timestamp] [Protocol] [Src IP].[Src Port] > [Dst IP].[Dst Port]: [Flags], [Seq], [Ack], [Win Size], [Options], [Data Length]

15:47:24.248737 IP 192.168.1.185.22 > 192.168.1.150.37445: Flags [P.], seq 201747193:201747301, ack 1226568763, win 402, options [nop,nop,TS val 1051794587 ecr 2679218230], length 108
```

- `15:47:24.248737` - The timestamp of the captured packet is in local time and uses the following format: hours:minutes:seconds.frac, where frac is fractions of a second since midnight.

- `IP` - The packet protocol. In this case, IP means the Internet protocol version 4 (IPv4).
  - `192.168.1.185.22` - The source IP address and port, separated by a dot (.).
  - `192.168.1.150.37445` - The destination IP address and port, separated by a dot (.).

- `Flags [P.]` - TCP Flags field. In this example, [P.] means Push Acknowledgment packet, which is used to acknowledge the previous packet and send data. Other typical flag field values are as follows:
  - `[.]` - ACK (Acknowledgment)
  - `[S]` - SYN (Start Connection)
  - `[P]` - PSH (Push Data)
  - `[F]` - FIN (Finish Connection)
  - `[R]` - RST (Reset Connection)
  - `[S.]` - SYN-ACK (SynAcK Packet)

- `seq 201747193:201747301` - The sequence number is in the **_first:last_** notation.
  - It shows the number of data contained in the packet. Except for the first packet in the data stream where these numbers are absolute, all subsequent packets use as relative byte positions.
  - In this example, the number is `201747193:201747301`, meaning that this packet contains bytes 201747193 to 201747301 of the data stream. Use the -S option to print absolute sequence numbers.

- `ack 1226568763` The acknowledgment number is the sequence number of the next data expected by the other end of this connection.

- `win 402` - The window number is the number of available bytes in the receiving buffer.

- `options [nop,nop,TS val 1051794587 ecr 2679218230]` - TCP options. nop, or “no operation” is padding used to make the TCP header multiple of 4 bytes. TS val is a TCP timestamp, and ecr stands for an echo reply. Visit the IANA documentation for more information about TCP options.

- `length 108` - The length of payload data

## Filters with `tcpdump`

**When `tcpdump` is invoked with no filters, it captures all traffic and produces a huge amount of output that makes it very difficult to find and analyze the packets of interest.**

#### 1. Filtering by Protocol

To restrict the capture to a particular protocol, specify the protocol as a filter.

```bash
sudo tcpdump -n udp

# Another way to define the protocol is to use the proto qualifier.

sudo tcpdump -n proto 17
```

[Protocol Numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)

#### 2. Filtering by Host

To capture only packets related to a specific host, use the `host` qualifier:

```bash
sudo tcpdump -n host 192.168.1.185
```

The host can be either an IP address or a name.

#### 3. Filtering by Port

To limit capture only to packets from or to a specific `port`, use the port qualifier. The command below captures packets related to the `SSH (port 22)` service by using this command:

```bash
sudo tcpdump -n port 22

sudo tcpdump -n port 443

# The port range qualifier allows you to capture traffic in a range of ports
sudo tcpdump -n portrange 110-150
```

#### 4. Filtering by Source and Destination

You can also filter packets based on the source or destination port or host using the `src`, `dst`, `src and dst`, and `src or dst` qualifiers.

```bash
sudo tcpdump -n src host 192.168.1.185

sudo tcpdump -n dst port 80
```

#### 5. Complex Filters

Filters can be combined using the and `(&&)`, or `(||)`, and not `(!)` operators.

```bash
# to capture all HTTP traffic coming from a source IP address 192.168.1.185 you would use this command
sudo tcpdump -n src 192.168.1.185 and tcp port 80

sudo tcpdump -n 'host 192.168.1.185 and (tcp port 80 or tcp port 443)'

# Here is another example command to capture all traffic except SSH from a source IP address 192.168.1.185:
sudo tcpdump -n src 192.168.1.185 and not dst port 22
```
