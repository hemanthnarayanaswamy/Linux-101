# Netcat (`nc`) Command in Linux

[nc](https://linuxize.com/post/netcat-nc-command-with-examples/), is a command-line tool for reading and writing data over network connections. It can connect to TCP or UDP ports, listen for incoming connections, transfer files, test services, and send raw requests.

By default, Netcat opens a TCP connection to the specified host and port. To use UDP instead, add the `-u` option:

```bash
nc host port
nc -u host port
```

<ul><li><code>-l</code> - Listen for an incoming connection instead of opening a connection.</li><li><code>-z</code> - Scan for listening services without sending data.</li><li><code>-v</code> - Print more connection details.</li><li><code>-u</code> - Use UDP instead of TCP.</li><li><code>-w</code> - Set a connection timeout in seconds.</li><li><code>-k</code> - Keep listening after a client disconnects.</li><li><code>-p</code> - Set the local source port, when supported by your Netcat version.</li></ul>

## Examples

1. Scanning ports is one of the most common uses for Netcat. You can scan a single port or a port range.

```bash
nc -z -v 10.10.8.8 20-80
# -z option tells nc to only scan for open ports without sending any data to them
# -v option provides more verbose information

# nc: connect to 10.10.8.8 port 20 (tcp) failed: Connection refused
# nc: connect to 10.10.8.8 port 21 (tcp) failed: Connection refused
# Connection to 10.10.8.8 22 port [tcp/ssh] succeeded!
# nc: connect to 10.10.8.8 port 23 (tcp) failed: Connection refused
# ...
# nc: connect to 10.10.8.8 port 79 (tcp) failed: Connection refused
# Connection to 10.10.8.8 80 port [tcp/http] succeeded!
```

# Quick Refernce

<table><thead><tr><th>Task</th><th>Command</th></tr></thead><tbody><tr><td>Scan ports</td><td><code>nc -z -v host 20-80</code></td></tr><tr><td>Scan UDP ports</td><td><code>nc -z -v -u host 20-80</code></td></tr><tr><td>Listen on port</td><td><code>nc -l 5555</code></td></tr><tr><td>Connect to port</td><td><code>nc host 5555</code></td></tr><tr><td>Transfer file (receive)</td><td><code>nc -l 5555 &gt; file</code></td></tr><tr><td>Transfer file (send)</td><td><code>nc host 5555 &lt; file</code></td></tr><tr><td>Send HTTP request</td><td><code>printf "GET / HTTP/1.1\r\nHost: example.com\r\n\r\n" | nc example.com 80</code></td></tr><tr><td>Set timeout</td><td><code>nc -w 5 host 80</code></td></tr><tr><td>Keep listening</td><td><code>nc -k -l 5555</code></td></tr><tr><td>UDP connection</td><td><code>nc -u host 5555</code></td></tr></tbody></table>
