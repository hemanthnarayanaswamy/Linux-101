# Linux Mastery Roadmap — SRE / DevOps / Platform Engineer Track

> **Profile**: Beginner → Senior/Staff level
> **Target roles**: SRE, DevOps, Platform Engineer
> **Pace**: ~5 hrs/week, open-ended (mastery > speed)
> **Lab environment**: Ubuntu 22.04/24.04 LTS + RHEL 9 (or Rocky/Alma 9 as free equivalent)
> **Estimated total duration**: 10–14 months at 5 hrs/week

---

## How to Use This Roadmap

1. **Never skip labs.** Reading ≠ knowing. You must break and fix systems to remember them.
2. **Keep a lab journal** (`~/linux-journal/` in Markdown). For each topic, write: _what I learned, what broke, how I fixed it, one real-world scenario it applies to_.
3. **Set up two VMs day one**: one Ubuntu, one Rocky/RHEL. Use VirtualBox, UTM (Mac), Multipass, or a cheap VPS ($5 Hetzner/DigitalOcean).
4. **One phase at a time.** Don't jump ahead — each phase is a prerequisite for the next.
5. **End every phase** by answering that phase's interview questions out loud, on video, or to a rubber duck.

---

## Phase 0 — Environment Setup (Week 1)

**Goal**: Have a reproducible lab you're not afraid to destroy.

### Tasks

- [ ] Install VirtualBox or Multipass on your host machine.
- [ ] Provision Ubuntu 24.04 Server VM (2 vCPU, 4 GB RAM, 20 GB disk).
- [ ] Provision Rocky Linux 9 VM (same specs).
- [ ] Configure SSH key-based login to both; disable password auth.
- [ ] Snapshot both VMs in a clean state — you will restore to this many times.
- [ ] Install `tmux`, `vim`/`neovim`, `git`. Learn 10 tmux shortcuts.
- [ ] Create a GitHub repo `linux-journal` and commit notes weekly.

### Deliverable

Two working VMs you can SSH into in under 10 seconds, plus a public journal repo.

---

## Phase 1 — Linux Foundations (Weeks 2–6)

**Goal**: Be fluent at the shell. No more Googling basic commands.

### Topics

1. **Filesystem Hierarchy Standard (FHS)** — `/etc`, `/var`, `/usr`, `/proc`, `/sys`, `/dev`, `/opt`, `/tmp`
2. **File operations** — `ls`, `cp`, `mv`, `rm`, `find`, `locate`, `stat`, `file`
3. **Text processing** — `cat`, `less`, `head`, `tail`, `grep`, `sed`, `awk`, `cut`, `tr`, `sort`, `uniq`, `wc`, `xargs`
4. **Redirection & pipes** — `>`, `>>`, `<`, `2>&1`, `|`, `tee`, process substitution `<()`
5. **Permissions** — `chmod` (symbolic + octal), `chown`, `umask`, setuid/setgid/sticky, ACLs (`getfacl`, `setfacl`), extended attributes
6. **Users & groups** — `/etc/passwd`, `/etc/shadow`, `/etc/group`, `useradd`, `usermod`, PAM basics
7. **Package management** — `apt`, `dpkg` (Ubuntu); `dnf`, `rpm` (RHEL); repo configuration, GPG signing
8. **Processes** — `ps`, `top`, `htop`, `pgrep`, `pkill`, `nice`, `renice`, signals (`kill -l`)
9. **Manual pages** — sections (1, 2, 3, 5, 8), `man -k`, `apropos`, `info`

### Labs

- [ ] Write a one-liner that finds the 10 largest files in `/var` and emails yourself the list.
- [ ] Create users `alice`, `bob`, `charlie`; create group `devs`; configure a shared directory `/srv/devs` where any group member can read/write but only file owners can delete.
- [ ] Break a broken `/etc/sudoers` recovery scenario (use `pkexec` / single-user mode to fix).
- [ ] Parse `/var/log/auth.log` with `awk` to count failed SSH logins per source IP, sorted descending.
- [ ] Compare `apt` vs `dnf` workflows: install, upgrade, pin a version, add a third-party repo, verify a GPG key.
- [ ] Write a `find` command that deletes all `*.tmp` files older than 7 days, excluding `/home`.

### Checkpoint

Without Google, explain the difference between `>` and `>>`, what `2>&1` means, and why `cmd | tee file` is useful.

---

## Phase 2 — Shell Scripting & Automation (Weeks 7–11)

**Goal**: Write bash scripts a teammate wouldn't be embarrassed by.

### Topics

1. **Bash fundamentals** — variables, quoting (single vs double vs backtick vs `$()`), exit codes, `set -euo pipefail`
2. **Control flow** — `if`, `case`, `for`, `while`, `until`, `[[ ]]` vs `[ ]`
3. **Functions, arrays, associative arrays**
4. **Parameter expansion** — `${var:-default}`, `${var##pattern}`, `${var//find/replace}`
5. **Traps & signal handling** — cleanup with `trap ... EXIT`
6. **Debugging** — `bash -x`, `PS4`, `shellcheck`
7. **Cron & systemd timers** — when to use which
8. **Intro to Python for ops** — when bash stops being the right tool

### Labs

- [ ] Write a backup script that rsyncs `/etc` to a remote host, logs output, rotates old backups (keep 7 daily, 4 weekly), and alerts on failure.
- [ ] Write a log parser that takes an nginx access log and prints: top 10 IPs, top 10 URLs, 4xx/5xx rate per hour.
- [ ] Convert a messy 200-line bash script (find one on GitHub) into idiomatic bash with `shellcheck` clean output.
- [ ] Build a healthcheck script that checks disk, memory, load, and a list of HTTP endpoints; exits nonzero if anything fails.
- [ ] Schedule your healthcheck via cron _and_ a systemd timer. Document trade-offs.

### Checkpoint

Refactor one of your scripts to handle `SIGINT` gracefully and clean up temp files on any exit path.

---

## Phase 3 — Processes, Memory & Linux Internals (Weeks 12–18)

**Goal**: Understand what's actually happening when a program runs.

### Topics

1. **Process lifecycle** — `fork`, `exec`, `wait`, zombies, orphans, PID 1, process groups, sessions
2. **Signals in depth** — SIGTERM vs SIGKILL vs SIGHUP; what `kill -9` actually bypasses
3. **Virtual memory** — pages, page tables, `mmap`, shared vs private, MMU basics, swap
4. **Memory accounting** — RSS vs VSZ vs PSS vs USS; why `free -h` confuses everyone
5. **File descriptors** — `/proc/<pid>/fd`, `lsof`, descriptor leaks
6. **System calls** — `strace`, `ltrace`; top 20 syscalls every SRE should recognize
7. **`/proc` and `/sys`** — how to read them; `/proc/<pid>/{maps,status,stack,limits}`
8. **Scheduling** — CFS, nice, `chrt`, CPU affinity (`taskset`), real-time classes
9. **ELF binaries & dynamic linking** — `ldd`, `readelf`, `LD_PRELOAD`, `LD_LIBRARY_PATH`

### Labs

- [ ] Write a C or Python program that forks 5 children, has one become a zombie, observe with `ps` and `/proc`.
- [ ] Use `strace -c` on `ls /`, `curl example.com`, and `python -c "import requests"`. Identify the top 5 syscalls in each.
- [ ] Cause and observe an OOM kill. Read `/var/log/kern.log` (Ubuntu) or `journalctl -k` for the OOM message and explain the scoring.
- [ ] Run `pmap -x <pid>` on a running process; map the output to the process's `/proc/<pid>/maps`.
- [ ] Use `LD_PRELOAD` to override `malloc` with a logging wrapper (find a sample online, compile it, apply it to `ls`).
- [ ] Produce a flame graph of a CPU-heavy program using `perf` + FlameGraph scripts.

### Checkpoint

Explain exactly what happens, step by step, when you type `ls` and press Enter. Cover: shell parsing → fork → exec → page faults → syscalls → stdout → terminal.

---

## Phase 4 — Storage, Filesystems & I/O (Weeks 19–23)

**Goal**: Know why disks fill up, why fsync matters, and how to recover data.

### Topics

1. **Block devices** — `/dev/sd*`, `/dev/nvme*`, `lsblk`, `blkid`
2. **Partitioning** — MBR vs GPT, `fdisk`, `parted`, `sgdisk`
3. **Filesystems** — ext4, XFS, btrfs, ZFS (overview); inodes, journals, `fsck`, `tune2fs`
4. **Mounting** — `/etc/fstab`, UUIDs, bind mounts, `mount --move`, `mount -o remount`
5. **LVM** — PV, VG, LV; resizing online; snapshots
6. **RAID** — `mdadm`, RAID levels, hot spares, rebuild behavior
7. **Swap** — swapfile vs partition, `swappiness`, `zram`, `zswap`
8. **I/O performance** — `iostat`, `iotop`, `blktrace`, I/O schedulers (`mq-deadline`, `bfq`, `none`)
9. **NFS, autofs, SMB** basics

### Labs

- [ ] Add a second virtual disk to a VM, create an LVM setup (PV → VG → LV), format as XFS, mount persistently.
- [ ] Extend the LV online without unmounting; verify with `df` and `lsblk`.
- [ ] Create an LVM snapshot, modify files on the origin, mount the snapshot read-only, compare.
- [ ] Simulate a disk failure in a software RAID 1; observe degraded state; replace and rebuild.
- [ ] Fill up the inode table of a small filesystem (create millions of empty files) — observe that `df` shows space free but writes fail.
- [ ] Benchmark `fio` with random vs sequential, direct vs buffered I/O; explain the numbers.

### Checkpoint

A user says "the server is full but `du` shows plenty of free space." List 5 possible causes and how you'd diagnose each.

---

## Phase 5 — Networking (Weeks 24–30)

**Goal**: Debug any network issue from L2 to L7 without guessing.

### Topics

1. **OSI & TCP/IP model** — frames, packets, segments, payloads
2. **Interfaces & addressing** — `ip addr`, `ip link`, `ip route`, `ip neigh`; deprecating `ifconfig`/`route`
3. **Routing** — default gateway, static routes, policy routing, `ip rule`
4. **ARP, DNS, DHCP** — how each actually works; `arp -a`, `dig`, `nslookup`, `resolvectl`, `/etc/resolv.conf`, `systemd-resolved`
5. **TCP deep dive** — 3-way handshake, state machine (SYN_SENT, TIME_WAIT, etc.), window scaling, congestion control (cubic, bbr)
6. **Sockets** — `ss`, `netstat`, listening vs established, TIME_WAIT tuning
7. **Firewalls** — `iptables`, `nftables`, `firewalld`, `ufw`; NAT, conntrack
8. **Packet capture** — `tcpdump`, Wireshark, BPF filters
9. **Traffic shaping** — `tc`, `netem` for simulating latency/loss
10. **Network namespaces** — the foundation of containers
11. **HTTP/HTTPS** — TLS handshake, SNI, cert chains, `openssl s_client`, `curl -v`

### Labs

- [ ] Capture a full TCP handshake with `tcpdump`; annotate SYN, SYN-ACK, ACK, and the first PSH-ACK with payload.
- [ ] Build two network namespaces connected by a veth pair; ping between them; add NAT so one can reach the internet.
- [ ] Simulate 200 ms latency and 2 % packet loss on an interface with `tc netem`; measure with `ping` and `iperf3`.
- [ ] Write an `nftables` ruleset that allows SSH only from your home IP, drops everything else inbound, and logs drops rate-limited.
- [ ] Debug a "connection refused" vs "connection timeout" — cause each deliberately and explain the difference at the packet level.
- [ ] Use `openssl s_client -connect example.com:443` to inspect a cert chain; identify CN, SAN, issuer, expiry.
- [ ] Diagnose a DNS issue where `ping google.com` fails but `ping 8.8.8.8` works — list 6 possible causes.

### Checkpoint

Walk through what happens from keypress to rendered page when you type `https://google.com` in a browser. Be exhaustive: DNS, TCP, TLS, HTTP, rendering.

---

## Phase 6 — systemd, Services & Boot (Weeks 31–34)

**Goal**: Master the init system that runs every modern Linux box.

### Topics

1. **Boot sequence** — BIOS/UEFI → bootloader (GRUB) → kernel → initramfs → systemd → targets
2. **systemd units** — service, socket, timer, mount, path, target, slice
3. **Writing unit files** — `[Unit]`, `[Service]`, `[Install]`; `Type=simple|forking|oneshot|notify`
4. **Dependencies** — `Requires`, `Wants`, `After`, `Before`, `Conflicts`
5. **Logging** — `journalctl`, persistent vs volatile, rate limiting, forwarding to syslog
6. **Targets & runlevels** — `multi-user.target`, `graphical.target`, `rescue.target`, `emergency.target`
7. **Socket activation** and **timers** (replacing cron for most use cases)
8. **Resource control** — integration with cgroups v2 (CPUQuota, MemoryMax, IOWeight)
9. **systemd-nspawn, systemd-networkd, systemd-resolved** (overview)

### Labs

- [ ] Write a systemd service for a Python web app; enable it; make it restart on failure with backoff.
- [ ] Convert a cron job to a systemd timer; show `systemctl list-timers` output.
- [ ] Create a socket-activated service that only starts when someone connects to its port.
- [ ] Limit a service to 50 % of one CPU and 256 MB RAM via unit file directives; verify with `systemd-cgtop`.
- [ ] Break GRUB on purpose (comment a line in `/etc/default/grub`, run `update-grub`, reboot); recover from the GRUB rescue prompt.
- [ ] Use `journalctl -u <service> --since "1 hour ago"` and `-p err`; configure persistent journals.

### Checkpoint

Compare SysV init, Upstart, and systemd. Why did systemd win? Name three legitimate criticisms of it.

---

## Phase 7 — Namespaces, cgroups & Containers (Weeks 35–40)

**Goal**: Understand containers at the kernel level — not just `docker run`.

### Topics

1. **Namespaces** — mnt, pid, net, uts, ipc, user, cgroup, time; `unshare`, `nsenter`
2. **cgroups v2** — hierarchy, controllers (cpu, memory, io, pids), delegation
3. **Capabilities** — `CAP_NET_ADMIN`, `CAP_SYS_ADMIN`, etc.; replacing root
4. **seccomp & AppArmor / SELinux** — syscall filtering
5. **Container runtimes** — runc, containerd, CRI-O; OCI spec
6. **Docker & Podman** — images, layers, `Dockerfile` best practices, multi-stage builds
7. **Image internals** — OCI image format, overlay filesystem
8. **Rootless containers**
9. **Intro to Kubernetes fundamentals** — pod, node, kubelet, kube-proxy (container context)

### Labs

- [ ] Build a "container from scratch" using `unshare`, `chroot`, `ip netns`, and cgroups — no Docker. Document every step.
- [ ] Compare the syscalls made by the same program inside and outside a container using `strace`.
- [ ] Write a multi-stage `Dockerfile` for a Go or Python app; final image under 20 MB.
- [ ] Set cgroup v2 memory and CPU limits on a shell session; run stress-ng; observe throttling and OOM.
- [ ] Inspect a running Docker container's namespaces via `/proc/<pid>/ns/*` and `nsenter` into each.
- [ ] Run a container as a non-root user with dropped capabilities; show what breaks and why.

### Checkpoint

Explain why a container is "not a VM." What does the kernel actually do differently for a containerized process vs a regular one?

---

## Phase 8 — Performance Tuning & Debugging (Weeks 41–46)

**Goal**: Diagnose any slowness methodically. Own production latency.

### Topics

1. **USE method** (Utilization, Saturation, Errors) — Brendan Gregg's framework
2. **RED method** (Rate, Errors, Duration) for services
3. **CPU profiling** — `perf top`, `perf record`, flame graphs, `pidstat`
4. **Memory profiling** — `vmstat`, `sar`, `smem`, `/proc/meminfo`
5. **Disk I/O** — `iostat -xz 1`, `iotop`, latency histograms
6. **Network** — `ss -ti`, `nstat`, `sar -n`
7. **BPF / eBPF** — `bpftrace`, `bcc-tools` (`execsnoop`, `opensnoop`, `tcpconnect`, `biolatency`)
8. **Latency tools** — `perf sched`, `runqlat`, `offcputime`
9. **Tuning knobs** — `/proc/sys/*`, `sysctl`, `tuned` (RHEL)
10. **Kernel tracing** — ftrace, tracepoints, kprobes, uprobes

### Labs

- [ ] Run the "60-second Linux performance analysis" checklist on a loaded VM; record findings.
- [ ] Use `bpftrace` to trace every file opened system-wide for 10 seconds.
- [ ] Generate a CPU flame graph of a real workload (nginx + `ab` load test).
- [ ] Cause high `iowait` deliberately (with `fio`) and identify the culprit using only iostat + pidstat.
- [ ] Tune `vm.swappiness`, `net.core.somaxconn`, `net.ipv4.tcp_tw_reuse`; measure before/after on a benchmark.
- [ ] Build a mental "cheat sheet" of what to run first for: high CPU, high memory, high I/O, high network, application latency.

### Checkpoint

You get paged: "API p99 latency is 10× normal." Walk through your first 15 minutes — exact commands, in order, and what you're looking for at each step.

---

## Phase 9 — Security Hardening & Forensics (Weeks 47–52)

**Goal**: Lock down a box and investigate it when something goes wrong.

### Topics

1. **Hardening basics** — CIS benchmarks, `lynis`, disabling unused services
2. **SSH hardening** — key-only auth, `AllowUsers`, port change (myth busted), fail2ban, `sshguard`
3. **SELinux** (RHEL) — enforcing/permissive/disabled, contexts, booleans, `audit2allow`
4. **AppArmor** (Ubuntu) — profiles, complain vs enforce
5. **PAM** — stack, common pitfalls, `pam_tally2` / `pam_faillock`
6. **Auditing** — `auditd`, `ausearch`, `aureport`
7. **File integrity** — `AIDE`, `tripwire`
8. **Secrets** — kernel keyring, `systemd-creds`, avoiding env-var leaks
9. **Forensics basics** — timeline with `mactime`, `/var/log/` triage, memory capture with LiME, `volatility`
10. **Common attack patterns** — persistence (cron, systemd, `~/.bashrc`, SUID), privilege escalation (sudo misconfig, capabilities, writable PATH)

### Labs

- [ ] Harden a fresh Ubuntu VM against CIS Level 1 benchmark; run `lynis audit system` before and after.
- [ ] Write an SELinux policy module for a custom daemon that listens on a non-standard port.
- [ ] Configure `auditd` to log every execution of `/bin/bash` and every read of `/etc/shadow`.
- [ ] Plant a persistence mechanism on a VM (cron, systemd, `.bashrc`, SUID binary); then, from a clean reboot, find all four using only manual investigation.
- [ ] Investigate a simulated compromise: given a VM with `last`, `w`, `/var/log/`, and bash history, reconstruct what the attacker did.
- [ ] Build an SSH bastion host with 2FA (google-authenticator PAM) and session recording (`tlog` or `auditd execve`).

### Checkpoint

A developer says "something weird is going on, my server feels compromised." What do you check first, in order? How do you investigate _without_ tipping off a live attacker?

---

## Phase 10 — Kernel & Deep Internals (Weeks 53–60)

**Goal**: Speak credibly about the kernel in a staff-level interview.

### Topics

1. **Kernel architecture** — monolithic vs microkernel, subsystems
2. **Building a kernel** — `make menuconfig`, modules, `dkms`
3. **Kernel modules** — `lsmod`, `modprobe`, `modinfo`, writing a hello-world module
4. **Syscall interface** — how a syscall actually crosses into kernel space (sysenter/syscall instruction, syscall table)
5. **VFS layer** — how `open()` reaches ext4 or XFS
6. **Networking stack** — netfilter hooks, socket buffers, XDP, eBPF in networking
7. **Memory management** — buddy allocator, slab, THP, NUMA
8. **Scheduler internals** — CFS runqueue, EEVDF (newer kernels), CPU isolation
9. **Debugging kernel issues** — `dmesg`, kernel panics, kdump, `crash` utility
10. **Real-time & low-latency** kernel options (PREEMPT_RT)

### Labs

- [ ] Compile a custom kernel with one config change (e.g., disable a driver); boot into it.
- [ ] Write a minimal loadable kernel module that logs "hello" on load and "goodbye" on unload.
- [ ] Trace a syscall from userspace to kernel using `perf trace` and kernel source code.
- [ ] Induce a kernel panic intentionally (`echo c > /proc/sysrq-trigger` with sysrq enabled) and analyze the trace.
- [ ] Read one kernel subsystem's source (suggest: `fs/proc/`) and write a 1-page summary of how `/proc/<pid>/status` is generated.

### Checkpoint

You can now reason from application → glibc → syscall → VFS → filesystem driver → block layer → device driver. Diagram this for a single `write(2)` call.

---

# Interview Preparation

This is organized by topic. At senior/staff level, interviewers care less about command trivia and more about **reasoning, trade-offs, and debugging under pressure**.

## Format of Real Interviews

- **Scenario-based**: "A server is slow. Walk me through." No commands are wrong if your reasoning is sound.
- **Design**: "Design a log collection system for 10,000 hosts."
- **Deep-dive**: "What happens when you run `cat /proc/<pid>/maps`?"
- **War stories**: "Tell me about the worst production incident you debugged." — have 3 stories ready.

---

## Topic 1 — Process & Memory

1. Explain the difference between a process and a thread in Linux. Is there really a difference to the kernel?
2. What is the difference between RSS, VSZ, PSS, and USS? When does each matter?
3. Process in state `D` (uninterruptible sleep) won't die even with `kill -9`. Why? How do you kill it?
4. What is a zombie process? An orphan? Who adopts orphans?
5. Walk through what `fork()` + `exec()` do at the kernel level. Why are they separate?
6. What's copy-on-write? Why does `fork()` of a 10 GB process not take 10 GB?
7. Explain the OOM killer's scoring. How do you make a process OOM-proof?
8. A process has 10 GB VSZ but the machine only has 8 GB RAM. Is that a problem?
9. What's the difference between `SIGTERM`, `SIGKILL`, `SIGHUP`, `SIGSTOP`?
10. How does `nohup` work? What does it actually do?

## Topic 2 — Filesystems & Storage

1. `df` says 100 % full, `du` says only 20 % used. What's going on?
2. Explain inodes. What happens when you run out of them despite having free space?
3. Hard link vs symbolic link — list five differences.
4. What does `fsync()` do? Why does losing it corrupt databases?
5. Explain the purpose of a journal in ext4. What does it _not_ protect?
6. You deleted a 50 GB file but `df` doesn't show the space freed. Why?
7. Walk me through creating an LVM setup from scratch. What's a VG, PV, LV?
8. When would you choose XFS over ext4? btrfs or ZFS over either?
9. What's the difference between `mount -o remount` and unmount/remount?
10. How does overlayfs work? Why is it the default for containers?

## Topic 3 — Networking

1. Walk through a TCP handshake and teardown. What's TIME_WAIT and why does it exist?
2. Difference between `ss` and `netstat`. Why is `ss` preferred?
3. A client gets "connection refused." Name 6 possible causes.
4. A client gets "connection timed out." Different causes — name 6.
5. What's the MTU? What happens when a packet exceeds it? What's PMTUD and how does it break?
6. Explain NAT. How does `conntrack` work? What's the table size and what happens when it fills?
7. How does DNS resolution work on a systemd-resolved system? What order are nameservers tried?
8. `curl https://example.com` is slow. Where in the stack could the slowness be?
9. What's SNI? Why is it needed?
10. Compare `iptables` and `nftables`. Why the migration?
11. Explain network namespaces. How does a container get network access?

## Topic 4 — systemd

1. Why did systemd replace SysV init? Give three technical reasons.
2. Difference between `Requires=` and `Wants=`. Between `After=` and `Requires=`.
3. What's socket activation? Why is it useful?
4. `Type=simple` vs `Type=forking` vs `Type=notify` — when to use each?
5. How do you make a service restart on failure with exponential backoff?
6. How does `journalctl` differ from traditional syslog? Can they coexist?
7. Your service fails to start. Walk me through debugging it with systemd tools only.
8. What's a systemd slice? How does it relate to cgroups?

## Topic 5 — Containers & Namespaces

1. What is a container, kernel-wise? Name all 8 namespaces.
2. Why is a container "not a VM"? What's shared?
3. Explain user namespaces. How do rootless containers work?
4. What's the difference between Docker and Podman at the architecture level?
5. A container can see the host's processes. What went wrong?
6. Why does a container sometimes see the host's total memory via `free`? How do you fix this?
7. Explain cgroup v1 vs v2. Why the migration?
8. What are Linux capabilities? Why drop them in containers?
9. How does overlay networking between containers on different hosts work (conceptually)?
10. What's a pause container in Kubernetes?

## Topic 6 — Performance & Debugging

1. Walk through the "USE method." When would you use it over "RED"?
2. A server's load average is 50 but CPU is idle. What's happening?
3. `top` shows 100 % CPU but `perf` shows no hot function. Where's the time going?
4. How do you capture a flame graph in production with minimal overhead?
5. What's eBPF? Give three SRE use cases.
6. A syscall `write()` is slow. List four layers where it could be slow and how to check each.
7. How does `perf` differ from `strace`? When would you choose which?
8. What's `iowait`? Is high `iowait` bad? Defend your answer.
9. The machine has 64 GB RAM. `free` shows 2 GB free. Is it a problem?
10. You suspect a memory leak in a long-running Go service. How do you confirm without restarting it?

## Topic 7 — Security & Forensics

1. A server is suspected compromised. What are your first 5 commands?
2. Explain the difference between SUID, SGID, and the sticky bit. Security implications of each?
3. How does sudo work under the hood? What's a common misconfiguration that leads to privilege escalation?
4. What's SELinux doing that DAC permissions can't?
5. You see a process running as root that you don't recognize. How do you investigate without killing it?
6. Describe 4 common Linux persistence mechanisms an attacker would use.
7. What's in `/var/log/wtmp` vs `btmp` vs `utmp`? How does an attacker cover tracks?
8. Explain PAM. Why is it powerful and dangerous?
9. What's a capability? Why is `CAP_SYS_ADMIN` called "the new root"?
10. A container escape — what kernel features prevent it, and how do they sometimes fail?

## Topic 8 — Boot, Kernel & Low-level

1. Describe the Linux boot process from power-on to login prompt.
2. What's initramfs? Why does it exist?
3. You boot and get "kernel panic — not syncing." How do you recover?
4. What's the syscall ABI? Why does it almost never change?
5. Explain virtual memory: page tables, TLB, page faults (minor vs major).
6. What happens on a page fault?
7. Compare kernel-space and user-space. What's the cost of the transition?
8. Explain the kernel's CFS scheduler in one minute. What replaced it in newer kernels?
9. What's NUMA? When does it matter for SRE?
10. What's an eBPF verifier and why does it exist?

---

## Staff-Level Scenario Questions

Prepare a 5–10 minute story-style answer for each:

1. **Debug a mystery outage**: Nginx is returning 502s intermittently. Upstream app logs look clean. CPU, memory, disk all normal. How do you approach it?
2. **Capacity planning**: Your database server's p99 write latency is creeping up over months. Design an investigation and remediation plan.
3. **Migration**: Move 500 microservices from bare-metal to Kubernetes without downtime. What's your rollout strategy? What Linux-level concerns dominate?
4. **Incident**: 3 AM page — filesystem is read-only on a production database host. What now?
5. **Cost optimization**: Your fleet is over-provisioned by 2×. How do you safely right-size at the OS level?
6. **Cross-functional**: A developer insists "the kernel is buggy." You suspect their code. How do you prove it without antagonizing them?

---

# Resources

## Books (pick one per topic, don't read them all)

- **The Linux Programming Interface** — Michael Kerrisk _(the Bible; keep for reference)_
- **How Linux Works** — Brian Ward _(best beginner → intermediate overview)_
- **Systems Performance** — Brendan Gregg _(2nd edition; Phase 8 is built from this)_
- **BPF Performance Tools** — Brendan Gregg
- **UNIX and Linux System Administration Handbook** — Nemeth et al. _(operational depth)_
- **Linux Kernel Development** — Robert Love _(concise kernel intro)_
- **The Art of Linux Kernel Design** — Lixiang Yang _(for Phase 10)_
- **Site Reliability Engineering** (free online) — Google _(culture and practice)_

## Online courses & docs

- Linux Foundation **LFS101** (free, Introduction to Linux) — do it in Phase 1.
- Linux Foundation **LFCS / LFCE** certification prep — great structured checklist.
- **RHCSA / RHCE** objectives — even if you don't sit the exam, the objective list is gold.
- **Brendan Gregg's website** (brendangregg.com) — performance chapters, flame graphs, BPF.
- **Julia Evans' zines** (wizardzines.com) — phenomenal for internals, debugging, networking.
- **`man7.org`** — Kerrisk's online man pages; definitive.
- **Arch Wiki** — best distro-agnostic reference on the internet.

## Practice platforms

- **OverTheWire: Bandit** — shell fluency (do all 34 levels in Phase 1).
- **Linux Journey** (linuxjourney.com) — structured beginner to intermediate.
- **SadServers** (sadservers.com) — real debugging scenarios, closest thing to on-call practice.
- **Katacoda archives / KillerCoda** — container and Kubernetes scenarios.
- **HackTheBox** / **TryHackMe** — security-flavored Linux practice.

## YouTube channels

- **Brendan Gregg** talks (USENIX, SREcon) — watch one per week.
- **Liz Rice** — containers from scratch.
- **Jessie Frazelle** — containers, kernel.
- **David Mahler** — networking fundamentals.

## Newsletters & blogs

- **LWN.net** — kernel and ecosystem news; subscribe.
- **Julia Evans' blog** (jvns.ca).
- **The Morning Paper** archives (for systems papers).
- **SRE Weekly**.

---

# Weekly Cadence Suggestion (5 hrs/week)

| Day | Time   | Activity                                                |
| --- | ------ | ------------------------------------------------------- |
| Mon | 1 hr   | Read theory / book chapter for current phase            |
| Wed | 1.5 hr | Hands-on lab                                            |
| Fri | 1 hr   | Continue lab / debug what broke                         |
| Sat | 1 hr   | Write journal entry, answer 3 interview questions aloud |
| Sun | 0.5 hr | Review last week, plan next                             |

---

# Self-Assessment Milestones

- **After Phase 3**: You can answer "what happens when you type `ls`" in under 5 minutes without hesitation.
- **After Phase 6**: You could run a small production Linux fleet (< 20 hosts) competently.
- **After Phase 8**: You're ready for SRE senior-level interviews at most companies.
- **After Phase 10**: You're ready for staff-level interviews at FAANG/infra-heavy companies.

---

# Final Notes

- **Be suspicious of tutorials that only show happy paths.** Break things on purpose.
- **Read the man page before Stack Overflow.** Always.
- **Write down every bug you hit and its root cause.** This becomes your interview war-story library.
- **Teach someone.** Blog posts, Stack Overflow answers, internal wiki pages — teaching exposes gaps instantly.
- **The goal is not to memorize commands. The goal is to build a mental model so accurate that new commands feel obvious.**

Good luck. See you at staff level.
