# Transmission Control Protocol

TCP is used when applications need reliable, ordered, connection-based communication.

- TCP makes sure data reaches the other side correctly and in order.

```bash
SSH # SSH uses TCP, usually port 22.
HTTP/HTTPS # HTTPS uses TCP, usually port 443.
MySQL/PostgreSQL
SMTP
FTP
Kubernetes API server
Docker registry
Git over HTTPS/SSH
```

## TCP Connection Lifecycle

![tcp](https://imgs.search.brave.com/xxHuAkWxRiwyRESKfphr7R_UxFFq935hGH7ziixwyHE/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly93d3cu/dHV0b3JpYWxzcG9p/bnQuY29tL2Fzc2V0/cy9xdWVzdGlvbnMv/bWVkaWEvNTE5MzEv/UGFnZS03MC1JbWFn/ZS0xMTYuanBn)

```bash
LISTEN # Service is waiting for connections
SYN-SENT # Application has initiated opening a connection
SYN-RECEIVED # Connection request received, waiting for ACK
ESTABLISHED # Active TCP connection exists
FIN-WAIT-1 # Application has finished sending data
FIN-WAIT-2 # other side has agreed to close, waiting for its FIN
CLOSE-WAIT # other side has initiated connection release
TIME-WAIT # TCP connection recently closed
CLOSING # Both sides have attempted to close simultaneously
CLOSE-WAIT # Other side has initiated connection release
CLOSED # No connection is active or pending
```

## TCP Three-Way Handshake

Before TCP sends application data, it creates a connection using a three-way handshake.

![handshake](https://imgs.search.brave.com/ig55W3_v3hq6N87zs321OjfKqvfBJCdN9lS4YGQhrrI/rs:fit:500:0:1:0/g:ce/aHR0cHM6Ly9icm93/c2VyLXdvcmtpbmcu/aGFzaG5vZGUuZGV2/L19uZXh0L2ltYWdl/P3VybD1odHRwczov/L2Nkbi5oYXNobm9k/ZS5jb20vcmVzL2hh/c2hub2RlL2ltYWdl/L3VwbG9hZC92MTc2/OTc4NTUyODE0Mi9l/NWRkMjU0OS02NDBj/LTQ1MjYtYWY4My05/NGE1ZWVjMjFmOWEu/cG5nJnc9Mzg0MCZx/PTc1)

1. **SYN (Synchronize)**: The sender sends a `SYN` segment to the receiver to request a connection.
2. **SYN-ACK (Synchronize-Acknowledge)**: The receiver responds with a `SYN-ACK` segment, acknowledging the request and agreeing to the connection.
3. **ACK (Acknowledge)**: The sender replies with an `ACK`, confirming the connection is established.

This process ensures both sender and receiver are ready and synchronized, preventing lost or misordered data at the start.

```ini
curl https://example.com

- DNS lookup happens
- TCP connection to port 443 starts
- TLS handshake happens
- HTTP request is sent
- HTTP response is received
```

## Connection Termination (Four-Way Handshake)

Closing a TCP connection requires a four-step handshake to ensure both sides finish transmitting data safely:

1. **FIN (Finish)**: The sender who wants to close the connection sends a FIN segment to the receiver.
2. **ACK (Acknowledge)**: The receiver acknowledges the FIN with an ACK.
3. **FIN (Finish)** from Receiver: The receiver then sends its own FIN when it is ready to close the connection.
4. **ACK (Acknowledge)**: The sender responds with an ACK, completing the termination.
   This ensures that all remaining data is transmitted before the connection is fully closed.

## Viewing TCP Handshake on Linux

`tcpdump` is a command-line utility that you can use to capture and inspect network traffic going to and from your system.

- Despite its name, with `tcpdump`, you can also capture non-TCP traffic such as `UDP, ARP, or ICMP`. The captured packets can be written to a file or standard output.

[tcpdump](../../commands/network-diagnostics/tcpdump.md)

## TCP ports in Linux

##### Web and Network Services

- `80`: HTTP (Unencrypted web traffic)
- `443`: HTTPS (Encrypted web traffic via TLS/SSL)
- `53`: DNS (Domain Name System, also uses UDP)
- `25`: SMTP (Simple Mail Transfer Protocol)
- `110`: POP3 (Post Office Protocol)
- `143`: IMAP (Internet Message Access Protocol)
- `123`: NTP (Network Time Protocol, primarily UDP but can use TCP)

##### Remote Access and File Transfer

- `22`: SSH (Secure Shell) and SFTP (SSH File Transfer Protocol)
- `21`: FTP (File Transfer Protocol)
- `23`: Telnet (Unencrypted remote access, deprecated)
- `5900`: VNC (Virtual Network Computing)
- `990`: FTPS (FTP over TLS/SSL)
- `6443`: Kubernetes API Sever

##### Databases and System Services

- `3306`: MySQL Database
- `5432`: PostgreSQL Database
- `27017`: MongoDB
- `6379`: Redis
- `445`: SMB (Server Message Block for file sharing)

## Important Linux Commands for TCP

- To show listining TCP ports we need the [ss](../../commands/network-interfaces/ss.md) command.

```bash
ss -tlnp
# sshd is listening on TCP port 22
# nginx is listening on TCP port 80
```

- The [nc](../../commands/network-diagnostics/nc.md) for testing if the TCP port is reachable
- The [tcpdump](../../commands/network-diagnostics/tcpdump.md) to analysis the TCP traffic and connection.

## Common TCP Errors

##### 1. Connection Refused

Server is reachable, but serrvice is not listening on that port.

```bash
ss -tlnp | grep :22
```

There are multipe possibilities for this cause.

- Service stopped
- wrong port
- Application binding only to localhost
- Firewall actively rejecting

##### 2. Connection timed out

Packet is being dropped or server is unreachable.

```bash
ip route get <server-ip>
nc -vz <server-ip> <port>
sudo tcpdump -i any -nn 'tcp port <port>'
```

There are multipe possibilities for this cause.

- Firewall blocking
- Security group blocking
- Network ACL blocking
- Wrong route
- Server down
- Load Balancer issue

##### 3. Connection Reset by Peer

Remote side forecefully closed the TCP connection.

There are multipe possibilities for this cause.

- Application Crashed
- Service rejected connection
- Load Balancer closed connection
- Protocol mismatch
- Firewall sent TCP reset

## TCP Binding

TCP binding in Linux involves a process reserving a specific local IP address and port combination for network communication, handled by the kernel’s networking stack rather than a filesystem lock.

A service can listen on different addresses.

```bash
0.0.0.0:80 # Listen on all interfaces
# Accept connections from outside the server.

127.0.0.1:5432 # Listen only on localhost
# Only local machine can connect.
# Remote machines cannot connect.

ss -tlnp | grep 5432
# LISTEN 0 128 127.0.0.1:5432

# This means postgreSQL is only listening locally. If another server tries to connect, it will fail.
```

## TCP and Firewalls

TCP ports must be allowed in firewalls.

```bash
sudo ufw allow 22/tcp
sudo ufw allow 443/tcp

# AWS Security Group
# GCP Firewall Rule
# Kubernetes NetworkPolicy
```

**If app is listening but firewall blocks the port, users still cannot connect.**

## TIME-WAIT & CLOSE-WAIT

- `TIME-WAIT` means: TCP connection closed normally, but Linux keeps it for a short time.

```bash
ss -tan state time-wait
# Many TIME-WAIT connections are normal on busy servers.
```

- `CLOSE-WAIT` means: Remote side closed the connection, but local application did not close it properly.

```bash
ss -tan state close-wait

#Many CLOSE-WAIT connections often indicate:

# Application bug
# Socket leak
# Thread stuck
# App not closing connections
```

- TIME-WAIT is usually normal
- CLOSE-WAIT often points to application-side issue.

## TCP Troubleshooting Flow

When a TCP service is not reachable, follow this order.

1. Is the service running ?

```bash
systemctl status nginx
ps aux | grep nginx
```

2. Is it listing ?

```bash
ss -tlnp | grep :80
```

3. Is it listening on the correct IP ?

```bash
127.0.0.1:80 # Bad for remote access
0.0.0.0:80 # Good fore remote access
```

4. Is the port Reachable ?

```bash
nc -vz <server-ip> 80
```

5. Is routing correct ?

```bash
ip route get <server-ip>
```

6. Is firewall allowing it ?

```bash
sudo nft list ruleset
```

7. Check packets

```bash
# ON server
sudo tcpdump -i any -nn 'tcp port 80'

# Test from client
nc -vz <server-ip> 80
```

| tcpdump result            | Meaning                        |
| ------------------------- | ------------------------------ |
| No SYN arrives            | Network/firewall before server |
| SYN arrives, no SYN-ACK   | Server/firewall issue          |
| SYN, SYN-ACK, ACK visible | TCP connection works           |
| RST visible               | Connection rejected/reset      |

- **Connection refused** usually means the server is reachable, but no service is listening on that port, or the connection is actively rejected.
- **Connection timed** out usually means packets are being dropped somewhere, commonly due to firewall, security group, routing issue, or the server being unreachable.
