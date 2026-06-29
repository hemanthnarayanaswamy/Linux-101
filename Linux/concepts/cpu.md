# Central Processing Unit (CPU)

In a computer system, processing and control are handled by a crucial component responsible for executing instructions and managing operations. It ensures that tasks like gaming, typing, and video playback run smoothly by performing calculations and making decisions.

It is installed in a socket on the motherboard, the main circuit board that connects all components of a computer. Its key functions include:
- Performing mathematical calculations
- Running applications and games
- Managing input/output (I/O) operations
- Storing and retrieving data during processing

## Main Components of CPU
The components of a CPU include the ALU (Arithmetic Logic Unit), CU (Control Unit), registers, cache, and clock.

![cpu](https://media.geeksforgeeks.org/wp-content/uploads/20250719153951741828/CPU-Components-.webp)

- **Control Unit**: The control unit manages the CPU by sending signals like clock, hold, and reset to its parts. It ensures all components work together to complete tasks. For example, it synchronizes data movement from cache memory to the ALU.
- **Arithmetic and Logic Unit (ALU)**: The ALU handles arithmetic tasks (like addition, subtraction, multiplication, division) and logical tasks (like AND, OR, comparisons). It uses addition for all calculations, e.g., solving 2×3 as 2+2+2=6.
- **Memory Unit**: The memory unit stores data and instructions. Older CPUs used *registers*, but modern ones also have fast cache memory. 

> The CPU fetches data from `RAM, ROM, or hard disks and stores it in registers or cache during tasks.`

## Functions of CPU

The functions of CPU involve processing instructions from programs and controlling all operations within the computer. This is carried out through a sequence known as the *Fetch-Decode-Execute-Store* cycle:

![function](https://media.geeksforgeeks.org/wp-content/uploads/20250506112724169099/functions_of_cpu.webp)

- **Fetch**: The CPU retrieves the instruction from main memory (RAM).
- **Decode**: The Control Unit interprets the fetched instruction to determine the required operation.
- **Execute**: The CPU performs the operation using the appropriate hardware components such as the ALU.
- **Store**: The result of the executed instruction is written back to memory or a register.

## Types of CPU

CPUs come in different configurations based on the number of cores they contain.  The core is responsible for the execution of the instructions.. A core acts like a mini processor inside the CPU, allowing it to handle multiple tasks efficiently.

- Single-Core CPU: It can only handle one task at a time, so it’s slow for modern apps like games or web browsers.
- Dual-Core CPU: Has two cores, so it can handle two tasks at once. It’s faster and better for multitasking, like listening to music while doing homework.
- Quad-Core CPU: Has four cores, making it great for heavy tasks like video editing or playing modern games. It’s very fast and common in today’s computers.
- Hexa Core CPU:  CPU has six cores. They can execute the tasks with much more speed and efficiency.
- Octa Core CPU: This CPU comprises eight cores. It basically comprises of two Quad cores.
- Deca Core CPU: In this CPU there are 10 cores. This is the most fastest CPU capable of doing multi tasking and parallel computing at an advanced level.

### CPU Make Computers Faster
Modern CPUs are designed to be super efficient. Here are a few ways they speed things up:

1. **Multiple Cores**: Many CPUs have multiple cores, which are like mini-CPUs that can work on different tasks at the same time. It’s like having several chefs in the kitchen instead of one.
2. **Faster Clocks**: The clock speed (measured in GHz, like 3.5 GHz) determines how many instructions the CPU can handle per second.
3. **Bigger Cache**: More cache means the CPU can store more data close by, reducing wait times. Bigger RAM.
4. **Pipelining**: This lets the CPU start working on the next instruction before finishing the current one, like a factory line.

---
# Metrics Measurement

**CPU Usage** refers to the percentage of the CPU’s capacity being used at a given moment. This metric tells us how much of the processing power is currently being utilized by running processes and applications.

### Single Core vs. Multi-Core Performance 
- For single-core processors, 100% or 1 CPU usage means that the one core is fully utilized. 
- In a multi-core CPU (common in modern systems), the overall CPU usage can exceed 100% when viewed as a total across multiple cores. For example, a quad-core CPU can manage a CPU usage of up to 400% or 4.

***CPU usage*** focuses on the percentage of CPU capacity engaged at a specific moment, ***CPU Load*** provides a deeper understanding of the overall demand on the CPU over a period of time. 
    * *CPU load* is often represented as a number that indicates how many processes are actively engaging the CPU and how they are queuing up to be executed.

### CPU Load Metrics
CPU load is shown as an average over three time periods: 1 minute, 5 minutes, and 15 minutes. Here's what these numbers mean:

- `1-minute load average`: The average number of processes ready to run in the last minute.
- `5-minute load average`: Smooths out short-term fluctuations, showing the average load over the last 5 minutes.
- `15-minute load average`: Provides a longer-term view, showing the average load over the last 15 minutes.

### Relation between CPU USAGE & CPU LOAD
1. **Correlation but Not Causation**: High CPU load does not automatically equate to high CPU usage. For instance, if many processes are queued but none are demanding CPU time, CPU usage may remain low despite a high load average.
2. **Time-Based vs. Moment-Based**: CPU usage is a snapshot of current performance, while CPU load is a rolling average over time. As such, trends in load can indicate potential issues, even when current usage appears normal.
3. **Monitoring Both Metrics**: For effective system monitoring, IT professionals often examine both CPU usage and load together. Combined analysis can yield insights into system health, underutilization of resources, or potential bottlenecks.

### Key Characteristics of CPU Load:

1. **Load Average**
CPU load is commonly expressed in terms of load averages, which indicate how many processes are either actively running or waiting for CPU time over a given interval (usually 1, 5, and 15 minutes).
2. **Understanding Load & Time**
A load average of 1.0 on a single-core CPU means that the CPU is fully utilized over that interval. On a dual-core CPU, a load average of 2.0 indicates the same full utilization across both cores. If the load average exceeds the number of CPU cores, it suggests that there are more processes demanding CPU resources than can be handled simultaneously.
3. **Latency and System Performance**
High CPU load can lead to increased latency and slow down system responsiveness. *Even if CPU usage is low, a high CPU load may suggest that processes are competing for limited resources.*

### Practical Load Thresholds Per CPU Core 

1. **Idle to Low Load** `0.0 to 0.5` per core: 
    * Indicates the CPU is underutilized or idle most of the time.

2. **Moderate Load** `0.5 to 1.0` per core: 
    * Suggests the CPU is handling a moderate workload and performing optimally.

3. **High Load** `1.0 to 2.0` per core: 
    * Indicates the CPU is heavily loaded but still within manageable limits. Systems can still perform well, but there might be some slowdowns.

4. **Overloaded** `> 2.0` per core: 
    * Indicates the CPU is overloaded, leading to significant performance degradation. This often results in longer response times and potential system instability.

### Implications of High CPU Usage and CPU Load

1. **Performance Degradation**
High CPU usage or load can lead to noticeable performance degradation. The operating system may become sluggish, applications can freeze or lag, and system responsiveness diminishes. Users might encounter delays while trying to start applications, open files, or switch between tasks.

2. **Resource Bottlenecks**
Persistent high CPU load can indicate resource bottlenecks. If applications consistently demand more CPU resources than are available, it might be time to either optimize the current system configuration or consider an upgrade to more powerful hardware with additional cores or threads.

### Troubleshooting High CPU Usage and Load

#### 1. Application Performance
- **Identify Resource Hogs**: Use monitoring tools to identify applications that are hogging CPU resources. Sometimes, poorly optimized software can consume excessive CPU, leading to high usage and load levels.
- **Update Applications**: Ensure that all applications are up-to-date. Developers often release performance improvements to fix known issues that may impact CPU usage.
  
#### 2. System Performance Settings
- **Adjust Power Settings**: In some cases, power settings can impact CPU performance. Make sure to set the system to "High Performance" mode if necessary.
- **Reduce Background Processes**: Many applications run background processes that consume CPU resources. Identifying and disabling non-essential background applications can free up valuable CPU capacity.

#### 3 Hardware Considerations
- **Upgrade Options**: If after optimization the system continues to experience high CPU load, it may be time to consider hardware upgrades such as adding more RAM or upgrading the CPU to a multicore processor.
- **Cooling Solutions**: Overheating can lead to throttling, where the CPU reduces its speed to cool down. Ensure that the CPU cooling solution is functioning correctly and that dust build-up is minimized.

---
# CPU Threads vs Cores

CPU cores and threads are fundamental elements that define how a processor executes tasks. Cores are the physical processing units that carry out instructions, while threads allow each core to manage multiple tasks simultaneously.

![tc](https://davescomputertips.com/wp-content/uploads/2022/01/Cores-versus-Threads-640x336.jpg)

CPU cores and threads work in tandem to maximize processing efficiency. Cores handle the actual execution of instructions, while threads allow each core to manage multiple tasks concurrently.

**For example**, a user wants to run a video rendering application, a browser with multiple tabs, and a background antivirus scan simultaneously.
    - To do that, each *core must handle a main task, such as rendering a video frame or processing a browser tab*. Threads allow these cores to divide subtasks and remain active.

![working](https://phoenixnap.com/kb/wp-content/uploads/2025/08/how-cpu-threads-and-cores-work.jpg)

1. **Task splitting**: The operating system or application breaks each main task into smaller subtasks that run independently. For example, a video frame is divided into multiple sections to be processed simultaneously.
2. **Thread Assignment**:  Each subtask is assigned to a thread. If a core supports multiple threads, it handles more than one subtask concurrently.
3. **Time slicing**: The CPU schedules each thread in short intervals, rapidly switching between them when one subtask is waiting for data or resources.
4. **Resource sharing** Threads share the core's execution units, cache, and registers, allowing efficient use of the core's hardware while executing multiple instruction streams.
5. **Parallel completion** As threads complete subtasks, the results are combined, completing the overall main task faster than if the core processed each subtask sequentially.

This mechanism allows cores and threads to work together efficiently, which improves performance and maintains smooth operation when multiple applications run simultaneously.

## What Is the Difference Between Physical and Logical Cores?
Physical and logical cores differ in whether the hardware is real silicon or a software-visible execution slot:

- **Physical cores** are independent silicon processing units, each with its own execution units, registers, and L1 cache.
- **Logical cores** are operating-system-visible execution slots, numbering ***one or two per physical core*** depending on simultaneous multithreading.
- Thread count equals the *logical core count* and reports the maximum number of instruction streams running concurrently.
- Shared resources mean two logical cores on one physical core split that core’s cache and execution ports rather than duplicating them.

***A processor advertised as `8 cores and 16 threads` has `8 physical cores and 16 logical cores`; the two logical cores on each physical core share its execution units and cache, so they do not deliver the throughput of two separate physical cores.***

<table><thead><tr><th>Use Case</th><th>Recommended Cores</th><th>Reason</th></tr></thead><tbody><tr><td>Office and web</td><td>4 to 6 cores</td><td>Light multitasking and browsing rarely exceed a few active threads</td></tr><tr><td>Gaming</td><td>6 to 8 cores</td><td>Most game engines use 6 to 8 threads with high per-core speed</td></tr><tr><td>Streaming while gaming</td><td>8 to 12 cores</td><td>Encoding the stream adds threads alongside the game</td></tr><tr><td>Content creation</td><td>12 to 16 cores</td><td>Video and 3D rendering scale with core count</td></tr><tr><td>Workstation and HEDT</td><td>16 to 32 cores</td><td>Heavy compilation and simulation reward maximum parallelism</td></tr></tbody></table>

## Single Core Running Multiple Threads
A single CPU core can run two or more threads, but not in the same way multiple cores run threads truly in parallel. This is achieved through `Simultaneous Multithreading (SMT)` or `time-slicing techniques`.

#### How it works:
- **Time-Slicing**: The operating system rapidly switches the core’s execution between threads, giving each a small time slice. This happens so fast that it appears the threads are running simultaneously, though only one is actively executing at any instant without SMT.
- **Simultaneous Multithreading (SMT) / Hyper-Threading**: Some CPUs (e.g., Intel with Hyper-Threading) allow a single physical core to execute two threads at the same time by sharing execution units. This improves utilization when one thread is stalled (e.g., waiting for memory).

**Example:** A 4-core CPU with Hyper-Threading can handle 8 logical threads — 2 per core — without waiting for one to finish before starting the other.

##### When it helps:
1. **I/O-bound tasks**: If threads spend time waiting for disk or network, extra threads keep the CPU busy.
2. **Mixed workloads**: One thread can handle computation while another manages user input or background tasks.
3. **Web servers / UI apps**: Responsiveness improves when multiple threads share a core.

##### When it doesn’t help:
1. **Pure CPU-bound tasks**: If both threads need the same execution units constantly, they compete for resources, and performance gains are minimal.
2. **Too many threads**: Excess threads cause context switching overhead, reducing performance.

***Practical tip:*** For CPU-bound workloads without SMT, 1 thread per core is often optimal. With SMT, up to 2 threads per core can be beneficial, but the exact number should be determined by benchmarking your specific application.
