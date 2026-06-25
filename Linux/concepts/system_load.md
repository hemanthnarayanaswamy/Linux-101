# System Load

system load is a measure of the amount of computational work that a computer system is performing. It represents the average number of processes that are either actively using the cpu or waiting to use the cpu.

cpu demand refers to the amount of processing power required by the tasks and processes running on a computer.

it’s directly linked to **system load**.

when cpu demand is high, the **system load** increases, indicating that the cpu is working hard to process all the requests.

## The Components of System Load
system load isn’t just about cpu usage; it’s influenced by various components that contribute to the overall workload on the system.

### 1. CPU Usage
cpu usage is the percentage of time the cpu is actively processing tasks.

High cpu usage directly contributes to system load, indicating that the cpu is busy executing instructions.

### 2. Memory Usage
memory usage refers to the amount of `RAM` being used by the system.

when ram is fully utilized, the system may start using the hard drive as **virtual memory (swap space)**, which significantly slows down performance.

*high memory usage increases system load because the system has to manage the swapping of data between ram and the hard drive.*

### 3. Disk I/O
`disk i/o (input/output)` refers to the rate at which data is being read from or written to the hard drive.

high disk i/o can create bottlenecks, especially if the hard drive is slow.

this contributes to system load because processes may have to wait for disk operations to complete.

### 4. Network activity
network activity includes sending and receiving data over the network.

high network activity can increase system load, especially if the cpu is involved in processing network packets or handling network-related tasks.

each of these components affects overall system performance by contributing to the workload on the cpu and other system resources.

## case studies: systems under high load conditions

- **web server under ddos attack**: a web server experiencing a distributed denial-of-service (ddos) attack may face extremely high system load due to the large number of incoming requests. this can cause the server to **become unresponsiv**e, leading to service disruptions.
- **database server processing complex queries**: a database server processing complex queries may experience high cpu and disk i/o load, resulting in slow query execution times and reduced overall database performance.
- **gaming pc running demanding games**: a gaming pc running a graphically intensive game may encounter high cpu and gpu load, leading to frame rate drops and stuttering if the hardware is not capable of handling the demand.

`top/htop` command is a command-line utility that provides a real-time view of the system’s processes, including cpu usage, memory usage, and system load averages.

---
## CPU Info on Linux

The simplest way to determine what type of CPU you have is by reading the `/proc/cpuinfo` virtual file.

```bash
$ cat /proc/cpuinfo

processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 142
model name	: Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz
stepping	: 10
microcode	: 0x96
cpu MHz		: 700.120
cache size	: 6144 KB
physical id	: 0
siblings	: 8
core id		: 0
cpu cores	: 4
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge ...
bugs		: cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf
bogomips	: 3600.00
address sizes	: 39 bits physical, 48 bits virtual
```

* **processor** - unique identifying number for each logical CPU, starting from 0
* **model name** - full name of the processor including brand and speed
* **cpu MHz** - current operating frequency of this logical CPU
* **cpu cores** - number of physical cores in the processor chip
* **siblings** - total logical CPUs in the same physical package (cores x threads per core)
* **physical id** - identifier for the physical processor chip; systems with multiple sockets show different values here
* **flags** - CPU feature flags indicating supported instruction sets and capabilities

```bash
# To get number of logical cpu core
cat /proc/cpuinfo | grep processor | wc -l
# 16

# To get the CPU Model
cat /proc/cpuinfo | grep -m 1 "model name"
# model name      : AMD Ryzen 7 PRO 8700GE w/ Radeon 780M Graphics

nproc	
# Print number of available processing units

lscpu	
# Display CPU architecture summary
	
# Show per-CPU core and thread layout
root@ubuntu-host ~ ➜  lscpu -e
CPU NODE SOCKET CORE L1d:L1i:L2:L3 ONLINE    MAXMHZ   MINMHZ       MHZ
  0    0      0    0 0:0:0:0          yes 5184.0000 400.0000 5071.5161
  1    0      0    1 1:1:1:0          yes 5184.0000 400.0000 5043.7998
  2    0      0    2 2:2:2:0          yes 5184.0000 400.0000 3073.3640
  3    0      0    3 3:3:3:0          yes 5184.0000 400.0000 4009.9780
  4    0      0    4 4:4:4:0          yes 5184.0000 400.0000 4967.1099
  5    0      0    5 5:5:5:0          yes 5184.0000 400.0000 4900.6270
  6    0      0    6 6:6:6:0          yes 5184.0000 400.0000 2839.3550
  7    0      0    7 7:7:7:0          yes 5184.0000 400.0000 2853.3220
  8    0      0    0 0:0:0:0          yes 5184.0000 400.0000 4045.3430
  9    0      0    1 1:1:1:0          yes 5184.0000 400.0000 3503.8291
 10    0      0    2 2:2:2:0          yes 5184.0000 400.0000 4998.4922
 11    0      0    3 3:3:3:0          yes 5184.0000 400.0000 2875.1631
 12    0      0    4 4:4:4:0          yes 5184.0000 400.0000 4997.8081
 13    0      0    5 5:5:5:0          yes 5184.0000 400.0000 3019.2070
 14    0      0    6 6:6:6:0          yes 5184.0000 400.0000 2862.5630
 15    0      0    7 7:7:7:0          yes 5184.0000 400.0000 4984.1680
```
---
## RAM information in Linux

The simplest way to check the RAM memory usage is to display the contents of the `/proc/meminfo` virtual file. This file is used by the `free`, `top`, `ps` , and other system information commands.

```bash
cat /proc/meminfo

MemTotal:        4030592 kB
MemFree:          401804 kB
MemAvailable:    2507504 kB
```

***Why does Linux show very little free memory?***
- Linux uses unused RAM for buffers and page cache to speed up the system. This memory is shown in **buff/cache** but can be reclaimed when applications need it. Look at the available column instead of only free.

***Installed RAM does not match the total shown by Linux***
- A small portion of physical memory is reserved by the kernel, firmware, integrated graphics, or hardware devices. That is why the reported total can be slightly lower than the RAM installed in the system.