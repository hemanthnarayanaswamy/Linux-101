# Understand Operating System and User Environment

### 1.1 Distribution Information 

```bash
cat /etc/os-release
```
Standard OS identification file. Include key fields like *Distribution Name*, *Version*.

### 1.2 Host and OS details

```bash
hostnamectl
```
```ini
Static hostname: ubuntu-host
Icon name: computer-container
Chassis: container ☐
Machine ID: 05ee70c460a9dd015c31410c674044a5
Boot ID: 779e1f2610d44f9b87a8da66ae37fd67
Virtualization: container-other
Operating System: Ubuntu 24.04.1 LTS              
Kernel: Linux 6.8.0-124-generic
Architecture: x86-64
```

### 1.3 Kernel Information 

```bash
uname -a # Shows complete system information
Linux ubuntu-host 6.8.0-124-generic #124~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Tue May 26 21:05:19 UTC  x86_64 x86_64 x86_64 GNU/Linux

uname -r # shows kernel RELEASE
6.8.0-124-generic
```
### 1.4 System Architecture
Shows CPU architecture (e.g., x86_64, aarch64).

```bash
arch      # x86_64
uname -m  # aarch64
```
---
### 2.1 Current User Identity

```bash
whoami # displays user name

id # display UID, GID, and group memberships
uid=0(root) gid=0(root) groups=0(root)
```
### 2.2 Logged-in User

```bash
users # show logged-in users
who # shows login sessions
```
---
### 3.1 Hostname
Idenfiies the system on the network. That Appears in logs and monitoring systems.
```bash
hostname
```
### 3.2 System Uptime
* Display System Running Time. From the last reboot/power-on
* Useful for detecting reboots and system stress
* Load Average

```bash
$ uptime
08:24:37 up 207 days, 11:10, 0 users, load average: 0.00, 0.03, 0.05
```
- `08:24:37` → Current system time
- `207 days, 11:10` → System uptime
- `0 users` → Logged-in users
- **load average** → CPU load over last *1, 5, and 15* minutes

---
### 4.1 Shell Environment Variables

```bash
env
```
* Display all active environment variables 

---
### 5. Help and Documentation
Linux provides built-in documentation for nearly all commands. 

```bash
man command
command --help
info command
```