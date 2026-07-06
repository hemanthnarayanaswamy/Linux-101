# The `ss` command in Linux

[ss](https://linuxize.com/post/ss-command-in-linux/) is a command-line utility for displaying socket statistics on Linux.

Linux `sockets` are software endpoints that serve as the primary interface between user processes and the network protocol stack, functioning effectively as file descriptors that allow programs to send and receive data using standard I/O operations like `read()` and `write()`.

Each Linux socket consists of the device's IP address and a selected port number.

A socket connection is a bidirectional communication pipe that allows two processes to exchange information within a network.

[How Sockets work in Linux](https://phoenixnap.com/kb/linux-socket)

```bash
ss [OPTIONS] [FILTER]

# when invoked without options, ss displays all non-listing sockets that have an established connection.
```

## List All Sockets

To list all sockets regardless of stste, use the `-a` option.

```bash
ss -a

#Netid  State   Recv-Q  Send-Q  Local Address:Port   Peer Address:Port
tcp    ESTAB   0       0       192.168.1.10:ssh      192.168.1.5:52710
tcp    LISTEN  0       128     0.0.0.0:http          0.0.0.0:*
udp    UNCONN  0       0       0.0.0.0:bootpc        0.0.0.0:*
```

- `-t`: TCP Sockets
- `-u`: UDP sockets
- `-l`: shows only sockets that are in the listening state
- `-p`: show the process name and PID
- `-n`: show numeric addresses and ports instead of resolving hostnames and service names

## Quick Reference

<table><thead><tr><th>Command</th><th>Description</th></tr></thead><tbody><tr><td><code>ss -a</code></td><td>List all sockets</td></tr><tr><td><code>ss -t</code></td><td>List TCP sockets</td></tr><tr><td><code>ss -u</code></td><td>List UDP sockets</td></tr><tr><td><code>ss -x</code></td><td>List Unix domain sockets</td></tr><tr><td><code>ss -l</code></td><td>Show listening sockets only</td></tr><tr><td><code>ss -tulpn</code></td><td>Listening TCP/UDP with process and numeric output</td></tr><tr><td><code>ss -tp</code></td><td>TCP sockets with process names</td></tr><tr><td><code>ss -tn</code></td><td>TCP sockets with numeric addresses</td></tr><tr><td><code>ss -s</code></td><td>Show socket summary statistics</td></tr><tr><td><code>ss -tn state ESTABLISHED</code></td><td>Show established TCP connections</td></tr><tr><td><code>ss -tnp dport = :80</code></td><td>Filter by destination port</td></tr><tr><td><code>ss -tn dst 192.168.1.5</code></td><td>Filter by remote address</td></tr><tr><td><code>ss -4</code></td><td>IPv4 sockets only</td></tr><tr><td><code>ss -6</code></td><td>IPv6 sockets only</td></tr></tbody></table>
