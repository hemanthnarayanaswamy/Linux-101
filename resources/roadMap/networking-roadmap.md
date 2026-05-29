# Networking Roadmap for DevOps / Platform / SRE

A pragmatic, depth-first roadmap to take you from "I sort of know what an IP address is" to **passing networking-heavy interviews and operating networks at FAANG/big-tech scale.**

This isn't a list of tools. It's a sequence designed so each layer compounds on the previous one. Skip phases at your own risk — gaps in fundamentals show up later as 2 AM incidents you can't debug.

**Total realistic timeline:** 12–18 months part-time if you're already a working engineer. 6–9 months full-time. The Linux + Kubernetes + eBPF middle is where most people skim and later regret it.

---

## How to use this roadmap

1. **Each phase has three sections:** _theory_, _hands-on labs_, _what "done" looks like_.
2. **Don't read passively.** If a phase says "build X," build X. The interview questions at big companies are about things you've _touched_, not things you've _read about_.
3. **Always have a homelab running.** A cheap VPS, an old laptop with Linux, or a Kubernetes cluster in `kind`/`minikube`. You will use it constantly.
4. **Wireshark and `tcpdump` should become muscle memory.** Open them every time you learn a new protocol. Watch the bytes.

---

## Phase 0 — Prerequisites (skip if you already have them)

You need these _before_ networking starts to make sense. If you don't have them, fix this first.

- **Comfortable Linux CLI:** navigating, file permissions, processes (`ps`, `top`, `kill`), package management, `bash` basics, pipes/redirection, `grep`/`awk`/`sed`.
- **One scripting language:** Python or Bash to start. Go later for systems work.
- **Git fundamentals:** branch, merge, rebase, PR workflow.
- **A working brain-model of how a computer works:** processes, memory, syscalls, file descriptors. You don't need OS internals yet, but you should know what a syscall is.

**Resources:** _The Linux Command Line_ (William Shotts, free), _Linux Journey_ (linuxjourney.com), _Missing Semester_ (MIT, free).

---

## Phase 1 — Networking Fundamentals (4–6 weeks)

This is the layer that everyone says they know and most people don't. You should be able to explain every concept here on a whiteboard from memory.

### 1.1 Models & Layering

- **OSI 7-layer model** — know what each layer does, not just the names.
- **TCP/IP 4-layer model** — what real systems use.
- **Encapsulation/decapsulation** — what an Ethernet frame looks like with an IP packet inside it with a TCP segment inside that with HTTP inside that.

### 1.2 Layer 2: Ethernet & MAC

- MAC addresses, MAC tables, switches vs hubs.
- **ARP** — how a host with an IP finds the MAC. _Crucial._ Most "weird L2 issues" come back to ARP.
- VLANs and 802.1Q tagging.
- Spanning Tree Protocol (STP) — at a conceptual level.

### 1.3 Layer 3: IP

- IPv4 address structure, classes (historical), **CIDR notation, subnetting, VLSM**. You should be able to subnet `10.0.0.0/16` into eight `/19` blocks on paper without thinking.
- Public vs private address space (RFC 1918), link-local, loopback, multicast.
- **IPv6** — addressing, SLAAC, dual stack. Don't skip this; big companies are increasingly v6-first internally.
- **Routing basics** — routing tables, default gateway, longest prefix match.
- **NAT/PAT** — what it does, why it exists, why it breaks things.
- **ICMP** — ping, traceroute, MTU/PMTUD.

### 1.4 Layer 4: TCP & UDP

- **TCP three-way handshake**, `SYN/SYN-ACK/ACK`, sequence numbers, ACKs.
- **TCP state machine** — `LISTEN`, `SYN_SENT`, `ESTABLISHED`, `FIN_WAIT_1/2`, `TIME_WAIT`, `CLOSE_WAIT`. Know what each state means and what causes a host to be stuck in it.
- Flow control (windowing) vs congestion control (CUBIC, BBR).
- Retransmissions, fast retransmit, SACK.
- UDP — when and why you'd use it.
- **Sockets API mental model** — `socket → bind → listen → accept` vs `socket → connect`.

### 1.5 Layer 7: Application Protocols

- **DNS** — record types (A, AAAA, CNAME, MX, TXT, NS, SOA, PTR, SRV), recursion, caching, TTLs, glue records, the resolution path from your laptop to a root server.
- **HTTP/1.1, HTTP/2, HTTP/3 (QUIC)** — methods, status codes, headers, persistent connections, pipelining vs multiplexing, HoL blocking.
- **TLS 1.2 vs 1.3 handshake** — what gets encrypted, what's a cert, what's a CA, what's mTLS.
- **DHCP** — DORA process.
- SSH, SMTP, FTP at a conceptual level.

### Lab work (Phase 1)

- Capture a TCP handshake in Wireshark; identify every flag and sequence number.
- Manually compute a subnet plan for a fake org with 5 VLANs.
- Use `dig +trace example.com` and trace the resolution top to bottom.
- Use `openssl s_client -connect example.com:443` and read the cert chain.
- Write a tiny TCP echo server in Python, then in Go.

### Resources

- **Book:** _Computer Networking: A Top-Down Approach_ — Kurose & Ross. (The single best networking textbook. Read it.)
- **Book:** _TCP/IP Illustrated, Volume 1_ — Stevens. (Reference; dense but legendary.)
- **Free course:** Stanford CS144 (Introduction to Computer Networking) on YouTube — also has labs where you build a TCP stack.
- **Site:** [howdns.works](https://howdns.works), [howhttps.works](https://howhttps.works) (illustrated, fast).

### "Done" check

You can sit in front of a whiteboard and explain what happens, byte by byte, when you type `https://www.google.com` into a browser — DNS, ARP, TCP, TLS, HTTP, all of it.

---

## Phase 2 — Routing, Switching & Network Security (3–4 weeks)

You're not becoming a CCIE, but you need to read network engineers' minds when something breaks.

### 2.1 Routing

- Static routing.
- **Distance-vector vs link-state.** RIP (history), OSPF (areas, LSAs at a conceptual level), EIGRP.
- **BGP** — _this is the one that matters at scale._ eBGP vs iBGP, AS, route advertisements, path selection. Big companies run BGP internally between data centers and racks. Cilium can run BGP. AWS Direct Connect is BGP. Cloud load balancers announce VIPs via BGP.
- Anycast — how Cloudflare/Google DNS works.

### 2.2 Switching

- VLANs, trunking, access ports.
- LAG/LACP (link aggregation).
- L3 switches vs routers (in practice they're the same thing).

### 2.3 Network security primitives

- **Stateful firewalls** vs stateless ACLs.
- **NAT** revisited (SNAT, DNAT, port-forwarding).
- VPN protocols: **IPSec**, **WireGuard** (modern, simple, fast), OpenVPN.
- TLS, mTLS, PKI, certificate chains, OCSP, CRLs.
- DDoS attack categories (volumetric, protocol, application) and mitigations.
- Zero-trust networking (BeyondCorp, identity-based perimeters).

### Lab work

- Set up two Linux VMs, configure one as a router with `iptables` and IP forwarding, route between them.
- Run a WireGuard tunnel between two VMs.
- Set up a private CA with `step-ca` or `cfssl` and issue mTLS certs.
- Read about a real BGP route leak incident (e.g., the 2019 Cloudflare/Verizon incident) until you understand why it broke the internet.

### "Done" check

You can explain BGP path selection, draw an mTLS handshake, and write `iptables` rules from memory to NAT a private subnet to the internet.

---

## Phase 3 — Linux Networking Deep Dive (6–8 weeks) ⭐

**This is the most undervalued phase.** Cloud and Kubernetes networking is _just Linux networking with extra YAML on top._ If you understand `ip`, `iptables`, namespaces, and bridges, K8s networking becomes obvious. If you don't, it stays magic forever.

### 3.1 The Linux network stack

- How a packet enters a NIC, gets a softirq, traverses Netfilter hooks, hits the routing decision, goes through `iptables`, and reaches a socket.
- The diagram of Netfilter hooks (`PREROUTING`, `INPUT`, `FORWARD`, `OUTPUT`, `POSTROUTING`) — print it, memorize it.
- Network device queues, RPS/RFS, RSS.

### 3.2 The `iproute2` toolkit (replace `ifconfig`/`route`/`netstat` forever)

- `ip addr`, `ip link`, `ip route`, `ip neigh`, `ip rule`, `ip netns`.
- `ss` instead of `netstat` (faster, more info).
- `bridge` for L2 bridge management.
- `tc` for traffic control (qdiscs, classes, filters) — used heavily in container networking and SLO-shaping.

### 3.3 Linux network namespaces ⭐⭐⭐

This is **the** feature that makes containers possible. Spend serious time here.

- Create a namespace: `ip netns add ns1`.
- Create a `veth` pair, put one end in the namespace, give it an IP, ping across.
- Build a bridge in the host namespace, connect three namespaces to it, route between them.
- Add NAT so namespaces can reach the internet.

If you can build "Docker networking" by hand with `ip netns` and `iptables`, you understand more than 80% of Kubernetes networking already.

### 3.4 `iptables` and `nftables`

- The four tables (`filter`, `nat`, `mangle`, `raw`) and the chains in each.
- Rule traversal order — _this is on every interview._
- `conntrack` — stateful connection tracking, why it's important, how it can fall over at scale.
- `nftables` — the modern replacement; learn the syntax.

### 3.5 Diagnostic tools — your daily toolkit

| Tool                       | Use                                           |
| -------------------------- | --------------------------------------------- |
| `tcpdump`                  | Capture packets at any layer                  |
| `wireshark`/`tshark`       | Analyze captures                              |
| `ss -tnp`, `ss -tlnp`      | Open sockets + processes                      |
| `dig`, `drill`, `host`     | DNS queries                                   |
| `mtr`                      | Like `traceroute` + `ping` combined           |
| `traceroute` / `tracepath` | Path discovery                                |
| `nc`/`ncat`                | Swiss army knife — listen, connect, port scan |
| `socat`                    | `nc` on steroids — proxies, tunnels           |
| `iperf3`                   | Throughput testing                            |
| `nmap`                     | Port scanning, service discovery              |
| `curl -v` / `httpie`       | HTTP debugging                                |
| `openssl s_client`         | TLS debugging                                 |
| `ethtool`                  | NIC stats, offloads, speed/duplex             |
| `conntrack -L`             | Inspect connection tracking table             |

### 3.6 Kernel network tuning

- The `sysctl` parameters that actually matter:
  - `net.core.somaxconn`
  - `net.ipv4.tcp_max_syn_backlog`
  - `net.ipv4.tcp_tw_reuse`, `tcp_fin_timeout`
  - `net.ipv4.ip_local_port_range`
  - `net.netfilter.nf_conntrack_max`
  - `net.core.rmem_max`/`wmem_max` and the TCP autotuning trio.
- Congestion control selection: `net.ipv4.tcp_congestion_control` (BBR vs CUBIC).

### 3.7 Linux as a router/load balancer

- IP forwarding, policy routing (`ip rule`).
- IPVS / LVS — the kernel L4 load balancer that powers `kube-proxy`'s IPVS mode.
- Source-NAT vs Direct Server Return (DSR).

### Lab work (the most important labs of this whole roadmap)

1. **"Build Docker networking from scratch":** With nothing but `ip netns`, `veth`, `bridge`, and `iptables`, build a setup where two namespaces share a bridge, get IPs from a fake DHCP, NAT out to the internet, and can `ping` each other and the host.
2. **"Be a load balancer":** Configure `iptables` DNAT to round-robin between three backend webservers running in namespaces.
3. **"Inspect a real workload":** Run `tcpdump` on a real K8s pod, identify which packets are pod-to-pod, pod-to-service, pod-to-internet.
4. **"Tune a TCP server":** Run a server, hammer it with 10K concurrent connections, watch it fail, tune the right `sysctl` knobs, watch it succeed.

### Resources

- **Book:** _Linux Network Administrator's Guide_ (older, but conceptually solid).
- **Book:** _The Linux Programming Interface_ — Kerrisk. (Chapters on sockets, network programming.)
- **Blog series:** Julia Evans' zines on networking and Linux debugging — wonderfully approachable.
- **Site:** [iximiuz.com/en/](https://iximiuz.com/en/) — interactive container/networking labs.
- **Doc:** The kernel's `Documentation/networking/` tree.

### "Done" check

You can build a working multi-host overlay network using only `ip`, `iptables`, and `vxlan`. You can debug "this pod can't reach that pod" by reasoning about Netfilter and routing tables, not by guessing.

---

## Phase 4 — Cloud Networking (3–4 weeks)

Pick **one** cloud (AWS unless your target company tells you otherwise) and learn it deeply. Then learn the equivalents for the others — they map cleanly.

### 4.1 AWS-centric (most common)

- **VPC** — CIDR planning, subnets (public vs private), route tables, internet gateways, NAT gateways.
- **Security groups** (stateful, attached to ENIs) vs **NACLs** (stateless, on subnets). Knowing the difference is interview-table-stakes.
- **ENIs** — elastic network interfaces, secondary IPs, where pod IPs come from with the AWS VPC CNI.
- **VPC Peering, Transit Gateway, PrivateLink** — when to use which.
- **Direct Connect** vs **Site-to-Site VPN**.
- **Route 53** — public/private zones, routing policies (latency, geo, weighted, failover), health checks.
- **Load Balancers:** **NLB** (L4, static IPs, eBPF-friendly) vs **ALB** (L7, host/path routing) vs **GLB** (Gateway LB for inline appliances). Know the differences cold.
- **CloudFront** — CDN, origin shielding, OAI/OAC.
- **AWS Global Accelerator** — anycast at AWS edge.

### 4.2 GCP & Azure equivalents (one-page mental map)

- GCP: VPC (global!), Cloud Load Balancing (one of the most sophisticated load balancers in production), Cloud DNS, Cloud CDN, Cloud Interconnect.
- Azure: VNet, Network Security Groups, Azure Load Balancer / Application Gateway / Front Door, ExpressRoute.

### 4.3 Hybrid & multi-cloud

- Why hub-and-spoke topologies dominate.
- BGP across clouds (Megaport, Equinix Fabric).
- Why "multi-cloud" usually means "multi-cloud disaster" without proper network design.

### Lab work

- Build a 3-tier VPC (public/private/data subnets across two AZs) with Terraform.
- Set up a NAT gateway, run a private EC2, watch it reach the internet.
- Set up VPC peering between two VPCs in different regions.
- Set up an ALB with host-based routing to two services.
- Configure Route 53 weighted routing and watch traffic split.

### Resources

- AWS official **VPC** documentation (yes, really, it's good).
- _AWS Networking Workshop_ (free): networking.workshop.aws.
- Adrian Cantrill's AWS courses (paid, but the networking deep dives are exceptional).
- For GCP: the Google "Reliable Data Pipelines" / "VPC Deep Dive" videos.

### "Done" check

You can architect a multi-region VPC topology, draw it on a whiteboard, and explain every routing decision and security boundary.

---

## Phase 5 — Container & Kubernetes Networking (6–8 weeks) ⭐

Phase 3 was about Linux networking. Now stack Kubernetes on top.

### 5.1 Container networking basics (review/extend)

- Docker bridge mode, host mode, none mode, macvlan, overlay (just enough to know they exist).
- How `docker run -p 8080:80` works under the hood — `iptables` `DNAT`.
- The CNI spec — what it actually is (a binary that gets called with JSON on stdin).

### 5.2 Kubernetes networking model — the four problems K8s solves

1. **Container-to-container** within a pod (loopback — same network namespace).
2. **Pod-to-pod** (every pod has its own IP, flat network, no NAT).
3. **Pod-to-service** (Services give stable VIPs).
4. **External-to-service** (NodePort, LoadBalancer, Ingress, Gateway API).

### 5.3 Services & `kube-proxy`

- ClusterIP, NodePort, LoadBalancer, ExternalName, Headless.
- `kube-proxy` modes: iptables (default, doesn't scale past ~5K services), IPVS (kernel L4 LB, much better at scale), eBPF (via Cilium — replaces `kube-proxy` entirely).
- Why `kube-proxy` is increasingly being replaced.

### 5.4 CNI plugins — pick your weapon

| CNI                       | Strategy                                   | When to use                                                           |
| ------------------------- | ------------------------------------------ | --------------------------------------------------------------------- |
| **Flannel**               | VXLAN overlay, simple                      | Small clusters, learning                                              |
| **Calico**                | BGP routing or VXLAN, strong NetworkPolicy | Most prod clusters in the past decade                                 |
| **Cilium**                | eBPF, kernel-level                         | The current best-in-class. Default in GKE Dataplane V2 and AKS Cilium |
| **AWS VPC CNI**           | Real VPC IPs per pod via ENIs              | EKS default; great integration, IP exhaustion gotchas                 |
| **Calico eBPF dataplane** | Like Cilium-lite                           | Calico shops moving toward eBPF                                       |

### 5.5 DNS in Kubernetes

- CoreDNS architecture, plugins, caching.
- The `ndots:5` problem and how it eats DNS performance at scale.
- ExternalDNS to sync K8s resources to Route53/CloudDNS.

### 5.6 Ingress and Gateway API

- Ingress controllers: nginx-ingress, Traefik, HAProxy, AWS ALB controller.
- TLS termination, cert-manager + Let's Encrypt.
- **Gateway API** — the successor to Ingress. More expressive (`HTTPRoute`, `GRPCRoute`, traffic splitting). Learn this; it's where the ecosystem is heading.

### 5.7 Network Policies

- Default-deny, then allow. Zero-trust at the pod level.
- `NetworkPolicy` (built-in, L3/L4 only) vs Cilium `CiliumNetworkPolicy` (L7-aware).

### Lab work

- Spin up a `kind` cluster with Cilium as CNI, no `kube-proxy`. Watch it work.
- Switch CNIs (delete cluster, re-create with Calico). Diff the behavior.
- Write a `NetworkPolicy` that denies all traffic to a namespace, then re-allow only what's needed.
- Trace a packet from a pod on Node A to a service backed by a pod on Node B with Cilium and Calico — watch how different the paths are.
- Install nginx-ingress, set up cert-manager, get a real Let's Encrypt cert on a hostname.

### Resources

- **Book:** _Networking and Kubernetes_ — James Strong & Vallery Lancey (O'Reilly). Best single book on K8s networking.
- **Site:** [learnk8s.io](https://learnk8s.io) — extremely high-quality K8s networking deep dives.
- **Free:** Cilium's official docs and the Isovalent labs (free, hands-on).
- **Talk:** "Life of a Packet" by Michael Rubin (KubeCon) — watch it twice.

### "Done" check

You can debug "this pod can't reach that service" with `tcpdump`, `cilium monitor`, `iptables -t nat -L`, and `conntrack`, _not_ by re-applying YAML and hoping.

---

## Phase 6 — Service Mesh (2–3 weeks)

The honest 2026 answer: **maybe you don't need one** if you're using Cilium. But you need to understand them either way.

### 6.1 What a service mesh actually does

- **Traffic management:** retries, timeouts, circuit breakers, traffic splitting, canary.
- **Security:** automatic mTLS between services.
- **Observability:** golden signals (latency, traffic, errors, saturation) per-service for free.

### 6.2 The architectural question of the decade

- **Sidecar mesh (classic Istio, Linkerd):** Envoy proxy injected into every pod. Heavyweight, latency overhead, but mature and feature-complete.
- **Sidecarless / ambient (Istio Ambient, Cilium Service Mesh):** L3/L4 in eBPF in the kernel; L7 in a per-node shared proxy. Far lower overhead.

### 6.3 Envoy

- The data plane that powers most meshes (Istio, AWS App Mesh, Consul Connect).
- Listeners, filters, clusters, routes — the four core concepts. Learn them; even if you don't run Envoy directly, every gateway product you'll touch is built on it.

### Lab work

- Install Istio in `demo` profile on `kind`, deploy bookinfo, do a canary deployment with `VirtualService`.
- Replace it with Linkerd; compare resource overhead.
- Try Cilium Service Mesh and observe with Hubble.

### Resources

- _Istio in Action_ — Christian Posta (Manning).
- Linkerd's own docs — exceptionally well-written.
- The Cilium Service Mesh blog series on isovalent.com.

### "Done" check

You can articulate, for a given workload, whether a service mesh is justified, and which architecture (sidecar vs ambient vs eBPF-native) fits.

---

## Phase 7 — eBPF & Modern Linux Networking (4–6 weeks) ⭐

If Phase 3 was the most undervalued phase, **eBPF is the most career-defining skill** you can pick up right now. It's how cutting-edge networking, observability, and security are all converging.

### 7.1 eBPF fundamentals

- The kernel was traditionally not user-extendable safely. eBPF changes that — it's a verifier-protected, JIT-compiled mini-VM running in the kernel.
- BPF programs attach to **hook points**: kprobes, uprobes, tracepoints, XDP, TC, cgroups, sockets.
- The BPF verifier — what it checks, why programs get rejected.
- Maps — how user space and kernel-space BPF programs share state.

### 7.2 Networking-relevant hooks

- **XDP** (eXpress Data Path) — runs _before_ the kernel networking stack. The fastest place to drop, redirect, or mangle packets. Used for DDoS scrubbing.
- **TC** (Traffic Control) hooks — slightly later in the path, more context.
- **socket** hooks — load balancing at `connect()` time. This is how Cilium does kube-proxy replacement at zero data-path cost.

### 7.3 Cilium architecture (deep dive)

- Cilium agent per node, eBPF programs in the kernel.
- Identity-based security (not IP-based).
- Hubble — observability layer, real-time service maps.
- Tetragon — runtime security and process-level visibility.
- Cluster Mesh — cross-cluster connectivity.

### 7.4 Tools to know

- `bpftool` — inspect/load BPF programs.
- `bcc` and `bpftrace` — high-level BPF tools (`tcpconnect`, `tcpaccept`, `tcplife`, `tcptop`).
- `cilium monitor` — real-time eBPF flow data.
- `pwru` (packet, where are you?) — Cilium's tool for tracing a packet through the kernel. Magical for debugging.

### Lab work

- Install Cilium on a `kind` cluster, enable Hubble UI, watch flows in real time.
- Use `bpftrace` to count TCP retransmits per process: a one-liner that would have taken weeks of kernel work to build five years ago.
- Read the source of a small Cilium eBPF program in `bpf/`.

### Resources

- **Book:** _Learning eBPF_ — Liz Rice (O'Reilly). The on-ramp.
- **Book:** _BPF Performance Tools_ — Brendan Gregg.
- **Free:** ebpf.io — the central hub.
- Brendan Gregg's blog is the canonical source for performance use cases.
- Isovalent's free Cilium training labs.

### "Done" check

You can read a `bpftrace` one-liner and understand it. You've debugged at least one real networking issue using `pwru` or `bpftool`.

---

## Phase 8 — Observability, Performance & Reliability (3–4 weeks)

The skills that separate "operates Kubernetes" from "operates production at scale."

### 8.1 The four golden signals (Google SRE)

**Latency, Traffic, Errors, Saturation.** Apply them to every layer of the network stack — NIC, host, pod, service, edge.

### 8.2 Tooling

- **Prometheus + Grafana** for metrics. Know how to write PromQL for percentile latency, error rate, RED metrics.
- **Hubble** for K8s flow-level observability.
- **Pixie** — eBPF-powered, auto-instrumenting observability for K8s.
- **OpenTelemetry** — distributed tracing standard.
- **Jaeger / Tempo / Honeycomb** — trace storage and analysis.
- **eBPF profilers** — `parca`, `pyroscope`.

### 8.3 Performance debugging methodology

- Brendan Gregg's **USE method** (Utilization, Saturation, Errors).
- The **RED method** (Rate, Errors, Duration) for services.
- The "60-second performance triage" checklist.
- Reading flame graphs.

### 8.4 TCP/network performance topics

- Bufferbloat and how to fix it (`fq_codel`, `cake`).
- BBR vs CUBIC under different network conditions.
- TCP slow start and how it punishes short-lived HTTPS connections.
- HTTP/3 / QUIC and why it matters at the edge.

### Lab work

- Instrument a microservice with OpenTelemetry, ship traces to Tempo, view in Grafana.
- Simulate packet loss with `tc netem` and watch app latency degrade.
- Compare BBR and CUBIC throughput on a synthetic high-latency link.

### Resources

- **Book:** _Site Reliability Engineering_ — the Google "SRE book," free online.
- **Book:** _The Site Reliability Workbook_ — companion, more practical.
- **Book:** _Systems Performance_ (2nd ed) — Brendan Gregg.

### "Done" check

You can take a "the API is slow" complaint and systematically isolate whether the bottleneck is client, network path, load balancer, mesh proxy, app, or database — without guessing.

---

## Phase 9 — Big-Company / FAANG-scale Topics (ongoing)

This is the depth that separates a senior platform engineer from someone who just runs a cluster. Each of these is a rabbit hole.

### 9.1 Load balancing at scale

- L4 vs L7 — why FAANG always has both, often three layers (edge, L4, L7).
- **Maglev / consistent hashing** — Google's paper, why it matters.
- **DSR (Direct Server Return)** — why hyperscalers use it.
- **ECMP** — equal-cost multi-path routing for L4 LB scale-out.

### 9.2 Edge & CDN

- Anycast for low-latency delivery.
- Edge compute (Cloudflare Workers, Lambda@Edge).
- TLS termination at the edge.
- DDoS scrubbing — XDP-based, BGP RTBH/Flowspec.

### 9.3 DNS at scale

- Authoritative vs recursive at planet scale.
- DNSSEC.
- Anycast DNS.
- DNS-over-HTTPS (DoH), DNS-over-TLS (DoT).

### 9.4 Multi-region / multi-cloud

- Active-active vs active-passive.
- Global load balancing (Route 53 latency, Cloud Load Balancing global, Akamai GTM).
- Data gravity and the network cost of distributed databases.

### 9.5 Network design patterns at scale

- Spine-leaf data center fabrics (vs traditional 3-tier).
- Clos networks.
- BGP-in-the-DC (RFC 7938, "Use of BGP for Routing in Large-Scale Data Centers").
- Why Facebook/Meta runs BGP to the rack and what they learned.

### 9.6 Security at scale

- WAF rules and tuning.
- Bot mitigation.
- Zero-trust architecture (BeyondCorp paper from Google — read it).
- Service identity (SPIFFE/SPIRE).

### Resources

- **Papers** (read, not just skim):
  - Maglev (Google, 2016) — software L4 load balancer
  - Andromeda (Google) — VM networking
  - Yahoo!'s Anycast load-balancing paper
  - Facebook's "Robotron" (network management at scale)
  - "BGP in the Data Center" (Dinesh Dutt, free O'Reilly book)
- **Talks:** SREcon and KubeCon "production stories" tracks.
- **Book:** _Designing Data-Intensive Applications_ — Kleppmann. Not networking-only, but every chapter touches it.

---

## Phase 10 — Putting it together: capstone projects

If you've done the labs along the way you've already done most of these. But here are 5 portfolio-grade projects that demonstrate big-company-readiness:

1. **"Build a multi-region web app from scratch on AWS."** Two regions, active-active, Route 53 latency-based routing, CloudFront in front, ALB → ECS/EKS → RDS Aurora Global. Document your network design.
2. **"Production-ready EKS cluster with Cilium, no kube-proxy."** Cilium as CNI, Hubble for observability, Cilium Network Policies for zero-trust, Gateway API for ingress, cert-manager for TLS, ExternalDNS for DNS automation. Terraform everything.
3. **"Observability stack for a microservices app."** Prometheus + Grafana + Tempo + Loki, instrumented with OpenTelemetry, with Hubble/Pixie for network flow observability. Write runbooks for the top 5 alerts.
4. **"Chaos engineering for the network layer."** Use `tc netem`, Chaos Mesh, or LitmusChaos to inject latency/loss/partitions. Document how the system responds. Tune for it.
5. **"From scratch: a working overlay network."** Using only Linux tooling (`ip netns`, `vxlan`, `iptables`), build a working multi-host pod network. Write the blog post explaining how it works. This single project, well-executed and documented, will get you interviews.

---

## Certifications — opinion

You don't need certs to get hired at big tech. But they're great forcing functions for studying. In rough order of relevance:

- **CKA (Certified Kubernetes Administrator)** — most relevant; very practical exam.
- **CKS (Kubernetes Security Specialist)** — heavily networking-focused (NetworkPolicies, mTLS).
- **AWS Advanced Networking — Specialty** — the gold standard for cloud networking depth.
- **CCNA** — controversial. The networking fundamentals are timeless and worth it. The Cisco-specific syntax is less useful for cloud/SRE work, but the foundation it forces you to build is.
- **CCNP Enterprise / Service Provider** — only if you're going _deep_ into network-engineering-adjacent SRE roles (e.g., backbone team at a hyperscaler).

---

## Recommended reading order (the "if I had to start over" list)

1. _Computer Networking: A Top-Down Approach_ — Kurose & Ross
2. _TCP/IP Illustrated, Vol 1_ — Stevens (reference, not cover-to-cover)
3. _The Linux Programming Interface_ — Kerrisk (sockets chapters)
4. _Networking and Kubernetes_ — Strong & Lancey
5. _Learning eBPF_ — Liz Rice
6. _BPF Performance Tools_ — Gregg
7. _Site Reliability Engineering_ (the Google book, free)
8. _Systems Performance_ — Gregg
9. _BGP in the Data Center_ — Dutt (free, O'Reilly)
10. _Designing Data-Intensive Applications_ — Kleppmann

---

## Daily/weekly practice habits

- Subscribe to **the Cloudflare blog**, **Cilium blog**, and **AWS architecture blog**. They publish real production stories.
- Watch one **KubeCon** or **SREcon** talk per week.
- When you debug something at work, write the post-mortem for _yourself_ even if your team doesn't require it. Networking debugging is pattern-matching, and patterns only stick when you reflect.
- Whenever you encounter a protocol you "know" — capture it in Wireshark anyway. You'll be surprised what you don't actually know.

---

## A note on AI/2026 reality

LLMs will write your YAML. They will not debug your `conntrack` table at 3 AM, explain why your service mesh ate 40% of your latency budget, or design a multi-region failover that actually works during a regional control-plane outage. The skills in this roadmap are appreciating in value, not depreciating. The bar to _get hired_ is rising; the bar to be _useful in production_ is rising faster.

Build the foundation. Touch the wires. Read the kernel source occasionally. Networking rewards depth more than almost any other systems discipline.
