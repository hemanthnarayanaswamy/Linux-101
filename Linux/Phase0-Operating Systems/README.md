# [Operating Systems Concepts for DevOps](https://applied-programming.github.io/Operating-Systems-Notes/)

### What is an Operating System?

- Interface between Hardware and Software applications/users.
- Hides hardware complexity (Read/write file storage, send/receive socket network)
- Handles resource management (CPU scheduling, Memory management)
- Provide isolation and protection (allocate different parts of memory to different applications so that applications don't overwrite other memory locations)

An Operating System is a layer of systems software that:

- directly has privileged access to the underlying hardware;
- hides the hardware complexity;
- manages hardware on behalf of one or more application according to some predefined policies.
- In addition, it ensures that applications are isolated and protected from one another.

## ![os](https://openstax.org/apps/image-cdn/v1/f=webp/apps/archive/20260407.195030/resources/ca3ac00c14a30b4b814bdd3caf7f38520d8af865)

There are 3 key elements of an operating system,

1. **Abstractions**: corresponds to applications that OS executes i.e, ability to hide the complexities of hardware and software while providing a simple interface for users. This abstraction allows for the management of resources like memory, storage and processors without the need to deal with the underlying hardware details.
   - _process, thread, file, socket, memory_
2. **Mechanisms**: Algorithms and processes that enable the OS to function effectively. These mechanisms are essential for managing system resources, executing programs, and providing a user interface.
   - _(create, schedule, open, write, allocate)_
3. **Policies**: Define how mechanisms are used to manage underlying hardware.
   - _Least Recently Used_(LRU), _Earliest Deadline First_(EDF)

```ini
Memory Management:

Abstractions: Memory page
Mechanisms: Allocate, map to a process
Policies: LRU
```

---

## Kernel in Operating System

A **kernel** is the core part of an operating system. It acts as a brdige between software applications and the hardware of a computer.

- The kernel manages system resources, such as the CPU, memory and devices, ensuring everything works together smoothly and efficiently.
- It handles tasks like running programs, accessing files and connecting to devices like printers and keyboards.

> An Operating System includes the kernel as its core, but also provides a user interface, file system management, network services and various utility applications that allow users to interact with the system

#### Functions of Kernel

1. **Process Management**: Scheduling and Execution of processes.
2. **Memory Management**: Allocation and deallocation of memory space, managing virtual memory, handling memory protection and sharing.
3. **Device Management**: Managing input/output devices, providing a unified interface for hardware devices and handling device driver communication.
4. **File System Management**: Managing file operations and providing a file system interface to applications.
5. **Resource Management**: Managing system resources (CPU time, disk space, network bandwidth). It allocating and deallocating resources as needed.
6. **Security and Access Control**: Enforcing access control policies like user permissions and authentication.
7. **Inter-Process Communication**: Facilitating communication between processes by providing mechanisms like message passing and shared memory.

#### Working of Kernel

- Kernel if the first part of the OS loaded into memory during boot, and it stays resident while the system is running.
- It operates in a privileged mode (kernel mode), separate from user mode for applications; user apps can’t directly access hardware or critical resources.
- Applications make requests to the kernel via system calls (or software interrupts). The kernel handles these by switching from user mode to kernel mode.
- Kernel executes the requested operation (e.g. file I/O, process creation, memory allocation).

```ini
Think of a computer like a city. User mode is like the public streets where everyday people (applications) can move around but can’t directly control traffic lights or power grids.
Kernel mode is like the city’s control center, where operators have full access to every system — traffic, electricity, water — and can make changes instantly.

When your computer runs, most programs operate in user mode for safety. They can’t directly touch hardware or critical system memory.

If they need something like reading a file or sending data to a printer, they make a system call — a formal request to the kernel.
The CPU then switches into kernel mode to perform the task, and switches back when done.
```

#### Kernel Space vs User Space

- Kernel space is the reserved memory region where the operating system kernel runs. The kernel is the core of the OS, responsible for managing hardware resources (CPU, memory, disk, network), enforcing security policies, and coordinating system-wide operations.

* Code running in kernel space has unrestricted access to the system. It can directly manipulate hardware, access any memory address, and execute privileged CPU instructions

- User space is the memory region where user applications (e.g., web browsers, text editors, games) execute. Unlike kernel space, user space is restricted: applications here cannot directly access hardware or modify critical system resources.

* Code in user space runs with limited permissions. It can only access its own allocated memory and must request kernel assistance (via "system calls") to interact with hardware or perform privileged operations (e.g., reading a file from disk).

#### Kernel Mode vs User Mode

A process alternates between two execution modes:

- **User Mode**: Most of the time, the process runs in user mode, executing application code with limited privileges.
- **Kernel Mode**: When the process needs a privileged operation (e.g., reading a file), it triggers a system call, switching to kernel mode. The kernel executes the operation and returns control to user mode.

---

## System Call

User programs cannot directly access hardware or critical OS resources because it would make the system unstable and insecure, hence they operate in _user mode_. The OS provides **system calls** -- controlled interfaces that allow user programs to request services from kernel.

These calls act as a gateway between user mode and kernel mode. System Calls are, **A way for programs to interact with the operating system.**

![systemcalls](https://media.geeksforgeeks.org/wp-content/uploads/20250124123632394421/introduction_to_system_call.webp)

#### How do System Calls Work?

A system call is a controlled entry point that allows a user program to request a service from the operating system. Here's how it works:

- The user program executes a system call instruction (e.g., using syscall or int 0x80).
- The CPU switches from user mode → kernel mode for safe execution.
- The kernel identifies the system call number and performs the requested operation (file access, process creation, memory allocation, etc.).
- After completing the task, the kernel switches back to user mode.
- The result (success/failure/data) is returned to the program.
- Without system calls, every program would need its own way to access hardware, leading to inconsistent and insecure systems.

#### Types of System Calls

![types of system calls](https://media.geeksforgeeks.org/wp-content/uploads/20251202114956548681/frame_11.webp)

- **File System**: Used to create, open, read, write, and manage files and directories.
- **Process Control**: Used to create, execute, synchronize, and terminate processes.
- **Memory Management**: Used to allocate, deallocate, and manage memory for processes.
- **Interprocess Communication (IPC)**: Used for data exchange and communication between different processes.
- **Device Management**: Used to request and release devices, and to perform read/write operations on them.

[WHAT HAPPENDS WHEN YOU TURN ON THE COMPUTER](../../practice/concepts/What-happens-when-we-turn-on-computer.md)

---

## Process in Operating System

- A process is a program in execution. When we write source code in C or C++ and compile it, the compiler generates an executable binary file. This executable file is called a **program**. When the program is loaded into memory and executed, it becomes a `process`.

- When loaded into memory, a process is divided into sections:

1. **Text/Code Segment** – contains executable instructions. It is typically a read only section.
2. **Data Segment** – Global and static variables.
3. **Stack** - Function calls, local variables, return addresses.
4. **Heap** – Dynamically allocated memory during runtime

### Attributes of a Process

A process has several important attributes that help the operating system manage and control it. These attributes are stored in a structure called the `Process Control Block (PCB)` (sometimes called a task control block). The PCB keeps all the key information about the process, including:

- **Process ID (PID)**: A unique number assigned to each process so the operating system can identify it.
- **Process State**: This shows the current status of the process, like whether it is running, waiting, or ready to execute.
- **Priority** and other CPU Scheduling Information: Data that helps the operating system decide which process should run next, like priority levels and pointers to scheduling queues.
- **I/O Information**: Information about input/output devices the process is using.
- **File Descriptors**: Information about open files and network connections.
- **Accounting Information**: Tracks how long the process has run, the amount of CPU time used, and other resource usage data.
- **Memory Management Information**: Details about the memory space allocated to the process, including where it is loaded in memory and the structure of its memory layout (stack, heap, etc.).

These attributes in the PCB help the operating system control, schedule, and manage each process effectively.

### Context switching

Context Switiching in Linux refers to the process where the CPU stops executing one process, saves its current state, and loads the saved state of another process. This mechanism is essential for multitasking, allowing multiple processes to share the CPU efficiently. It ensures that processes appear to run simultaneously, even though the CPU executes only one process at a time.

- Context switches in the Linux kernel can lead to performance issues due to the overhead involved in saving and restoring the state of the CPU.
- Every time the kernel performs a context switch — moving execution from one thread to another — the CPU flushes registers, updates the program counter, and reloads process metadata. This is not “free multitasking.” It burns thousands of CPU cycles.
  When the switch rate spikes, cache locality is destroyed and scheduler overhead snowballs, especially in high-load systems.

### Zombie Processes 

In Linux, every process goes through a lifecycle: *creation, execution, and termination*. When a parent process spawns a child using `fork()`, the child executes its task and eventually calls `exit()` to terminate. At this point, the child process is no longer running, but the kernel retains its process table entry to store the exit status for the parent to collect. This temporary state is called a **zombie or defunct process**

The parent process is responsible for "reaping" the child by calling `wait() or waitpid()`. This system call retrieves the child's exit status and allows the kernel to remove the process table entry. If the parent fails to perform this step—either due to ignoring the `SIGCHLD` signal or *programming errors—the* child remains in the zombie state 

When a child process terminates in Linux, it enters a "**zombie**" state. In this state, the process has completed its execution, but its entry in the process table remains until the parent process retrieves its exit status. The main reason for this mechanism is to allow the parent process to obtain information about how the child process exited, such as the exit code.
- It is not an active process and does not consume CPU or memory but it can still occupy an entry in the process table.
- Zombie processes are identified by the Z state in the process table. They consume minimal system resources, mainly just an entry in the process table. 
- However, if a large number of zombie processes accumulate, they can exhaust the available process IDs, preventing new processes from being created.

![zombie](https://media.geeksforgeeks.org/wp-content/uploads/20250116110406721449/zmb.webp)

The main reason zombie processes exist is to allow the parent process to obtain information about the child's termination, such as success or error codes. This mechanism supports error handling, resource accounting, and proper system management

Although zombie processes themselves consume very few resources, a large number of them can lead to problems:
1. **Resource Exhaustion**: As mentioned earlier, they can exhaust the available process IDs, which are a finite resource in the system.
2. **Monitoring and Debugging**: They can clutter the process list, making it difficult to monitor and debug other processes.

* If the parent process is unresponsive or has a bug, you can kill it. When the parent process dies, the zombie processes are adopted by the init process (PID 1), which automatically cleans them up.

> `Orphan Process`: A process whose parent process no more exists i.e. either finished or terminated without waiting for its child process to terminate is called an orphan process.
> `Orphaned Zombies`: If the parent process exits before reaping the child, the child is adopted by the `init` process (PID 1), which automatically reaps it to prevent indefinite zombie accumulation

---
## Process Management

Process management is a core function of an Operating System (OS). It deals with creating, scheduling, and coordinating processes to ensure efficient CPU utilization and smooth system performance.

![process](https://media.geeksforgeeks.org/wp-content/uploads/20260227113344202746/tasks_of_process_managementjjh.jpg)
![management](https://applied-programming.github.io/Operating-Systems-Notes/images/processlifecycle.png)

---

## Threads in Operating System

Thread is a smallest unit of execution within a process. It enables a program to perform multiple tasks concurrently while sharing the same memory and resources. Threads improve application performance and responsiveness in multitasking environments. Threads provide a way to improve application perfromance through parallelism.
**Each thread belongs to exactly one process and no thread can exist outside a process.**

Multiple threads in the same process can share the same resources but work on different tasks at the same time.
This makes programs faster and more responsive — for example:

- In a web browser, one thread can load a webpage while another plays a video.
- In a game, one thread can handle graphics while another processes user input.

#### Program vs Process vs Threads

- A Program is an executable file containing a set of instructions and passively stored on disk. One program can have multiple processes. For example, the Chrome browser creates a different process for every single tab.
- A Process means a program is in execution. When a program is loaded into the memory and becomes active, the program becomes a process. The process requires some essential resources such as **registers, program counter, and stack.**
- A Thread is the smallest unit of execution within a process.

```ini
In simple terms, a thread in an operating system is like a small worker inside a program that does part of the job.

A program is like a company.
A process is like the company running and using resources (memory, files, etc.).
A thread is like an employee in that company — it works on a specific task.

- The program contains a set of instructions.
- The program is loaded into memory. It becomes one or more running processes.
- When a process starts, it is assigned memory and resources. A process can have one or more threads. For example, in the Microsoft Word app, a thread might be responsible for spelling checking and the other thread for inserting text into the doc.
```

![ppt](https://assets.bytebytego.com/diagrams/0304-program-process-thread.png)

- Processes are usually independent, while threads exist as subsets of a process.
- Each process has its own memory space. Threads that belong to the same process share the same memory.
- A process is a heavyweight operation. It takes more time to create and terminate.
- Inter-thread communication is faster for threads.

#### Components of Threads

These are the basic components of the Operating System.

- **Stack Space**: Stores local variables, function calls, and return addresses specific to the thread.
- **Register Set**: Hold temporary data and intermediate results for the thread's execution.
- **Program Counter**: Tracks the current instruction being executed by the thread.

#### Types of Threads

Threads are mainly classified based on how they are managed and scheduled in an operating system. There are two primary types of threads.

![threads](https://applied-programming.github.io/Operating-Systems-Notes/images/kernelvuserthread.png)

1. **Kernel Level Threads**: Kernel threads are managed directly by the OS kernel. They are scheduled by the kernel’s process scheduler and have full access to kernel resources.
   - Kernel threads handle background tasks like flushing disk caches (pdflush in Linux) or managing network buffers.
2. **User Level Threads**: User threads are managed by user-space libraries (e.g., POSIX Pthreads, Windows Fibers) rather than the kernel. They are lighter and faster to create but depend on kernel threads for execution.

---

## [Concurrency vs Parallelism vs Multithreading](https://medium.com/evrekadev/understanding-concurrency-parallelization-and-multithreading-9923acfa07cb)

1. **Concurrency** is about managing multiple tasks at the same time, but not necessarily executing them simultaneously. On a single-core CPU, this is achieved through context switching, where the processor rapidly switches between tasks, giving the illusion of simultaneous execution. It’s useful for improving responsiveness, such as handling multiple user requests in a server.

```ini
Imagine a restaurant with one chef (a single-threaded process) handling multiple customer orders (tasks).
Each order represents a different task that needs to be cooked.

The chef can prepare ingredients for Order 1, then while Order 1 is cooking, he can start preparing ingredients for Order 2.
As Order 2 starts cooking, the chef can move to Order 3 and begin preparing it.
The chef is switching between orders, making progress on each without having to wait for one to be completely finished.
```

2. **Parallelization** takes the idea of concurrency further by executing multiple tasks simultaneously. This is possible with the use of multiple processors or cores. Parallelization is about dividing a large task into smaller sub-tasks and processing them at the same time to reduce overall completion time.

```ini
Imagine a large kitchen with multiple chefs, each dedicated to a different order.
Each chef is responsible for preparing and cooking a different dish at the same time.

Chef 1 works on Order 1, Chef 2 works on Order 2, and Chef 3 works on Order 3.
```

3. **Multithreading** is a specific type of concurrency where multiple threads run within a single process, sharing the same memory space. Threads are lightweight processes that can run concurrently, allowing multiple operations to be performed in the same application environment.

```ini
Imagine a kitchen where a single chef is working with multiple pots on the stove.
The chef switches between different tasks but operates in the same space, sharing tools and ingredients.

The chef might stir one pot (Thread 1), then check the oven (Thread 2), and finally prepare a salad (Thread 3), all within the same cooking area.
The chef switches rapidly between these tasks, making progress on each.
```

![img](https://miro.medium.com/v2/resize:fit:720/format:webp/0*M-68jf0c7Qp5kkO2)

---

## `root` in Linux

In Unix-like OS, **root** can mean two related but different things: `Root User` or `Root Directory`

- The Root User is _superuser_ account with **UID = 0** and can perform any operation on the system.
- The Root Directory (`/`) is **top level directory** in the filesystem hierarchy. All files and directories branch from `/`
- _Not to be confused_ with `/root`, which is the home directory of the **root user**.

---

## `chroot` in Linux

- But each process has its own idea of what the root directory is. By default, it is actual system `root (/)` but we can change this by using **chroot()** system call.
- **chroot** is both a system call and a command-line utility that changes the apparent root directory (/) for a running process and its children.
- Once inside a chroot environment, the process cannot access files outside the new root directory — effectively creating a **sandbox**.
- Normally `/` is the system root. chroot makes a specified directory act as `/` for the process. chroot is not a complete security mechanism — privileged users (root) can break out.

---

## CGROUPS in Linux

Though Linux is excellent at handling and sharing available resources between processes, sometimes we want better control over resources.We want to allocate or guarantee a certain amount of resources to a group of processes. We do this with `cgroups`. This isolates an application/group’s resources.

- `Control Groups (cgroups)` are a Linux kernel feature that allows administrators to allocate and limit system resources such as _CPU, memory, and I/O_ to processes. By organizing processes into hierarchical groups, cgroups enable fine-grained control over how much of each resource a _process or group of processes_ can use.

```ini
Suppose we have an application we want to isolate usage for. Lets call it A1. Lets call rest of system as S. We will create a control group and assign resource limits on it: say 3GB of memory limit and 70% of CPU. Then we can add requisite application’s process id to the group and application resource usage now is throttled. Though the application may exceed the limits in normal scenarios, it will be throttled back to pre set limits in case system is facing resource crunch. This makes even more sense when we are handling many VMs running on a machine-have a cgroup for VMs and throttle them individually to a set limit when resource contention happens.
```

#### Core Features of Cgroups:

1. **Resource Limitation**: Cgroups allow administrators to define hard and soft limits for _CPU, memory, disk I/O_, and other resources, ensuring no process or container can exceed its allocated resources.
2. **Resource Prioritization**: Different processes or containers can be assigned varying resource priorities, allowing critical applications to receive more processor time than others.
3. **Resource Isolation**: Cgroups isolate resource usage, ensuring that one process or container does not affect the performance of others, even on the same host.
4. **Monitoring and Accounting**: Cgroups enable tracking of resource usage, helping administrators monitor performance and consumption across groups.
5. **Process Freezing and Thawing**: You can suspend (freeze) and resume (thaw) processes within a cgroup, useful for managing resource-intensive jobs.

#### Subsystems (Controllers) in Cgroups

Cgroups operate through controllers, each managing a specific resource:

1. **cpu**: Controls CPU access and limits.
2. **memory**: Limits memory usage and manages memory reclaiming (swap).
3. **blkio**: Regulates disk I/O bandwidth.
4. **cpuset**: Binds processes to specific CPUs and memory nodes.
5. **devices**: Restricts access to devices.
6. **freezer**: Suspends or resumes the execution of processes.

These controllers ensure processes remain within their resource boundaries, preventing any single process from starving others of necessary resources.

## [Linux Namespaces](https://linuxhandbook.com/namespaces/)

Kernel namespaces are a Linux kernel feature that provide process isolation by creating virtual boundaries around system resources. They ensure that a process can only see and interact with the resources assigned to its namespace, even though multiple processes share the same physical machine.

When a namespace is created, the kernel gives processes inside it their own isolated view of a specific resource. For example, in a PID namespace, processes have their own process ID space — PID 1 inside one namespace is unrelated to PID 1 in another. This isolation is similar to giving each group of processes its own “private rehearsal room” where they can’t see or affect others.

#### Types of Namespaces

![types](https://digitalpress.fra1.cdn.digitaloceanspaces.com/bzg4z45/2025/07/linux-namespaces-types.webp)
Depending on which system component we want to restrict processes view from, we need to use specific namespaces types.

The types of namespaces available on the system can be listed as follows: `lsns -o TYPE # list all namespaces`

<ul>
<li><strong>PID Namespace</strong>: Isolates process IDs, allowing processes in different containers to have independent PID hierarchies.</li>
<li><strong>Network Namespace</strong>: Isolates network interfaces, IP addresses, routing tables, and port numbers.</li>
<li><strong>Mount Namespace</strong>: Isolates mount points, giving each container its own view of the file system, thus preventing interference.</li>
<li><strong>UTS Namespace (UNIX Time-Sharing System)</strong>: Isolates system identifiers like hostname and domain name, allowing each container to have its own hostname.</li>
<li><strong>IPC Namespace (Inter-Process Communication)</strong>: Isolates IPC resources like message queues and shared memory.</li>
<li><strong>User Namespace</strong>: Allows processes to have different user and group IDs inside a container, enhancing security.</li>
<li><strong>Cgroup Namespace</strong>: isolates inter-process communication resources, enhancing security by hiding resource constraints from processes in different namespaces.</li>
</ul>

#### The Role of Cgroups and Namespaces in Containerization

![container](https://setcreed.oss-cn-shanghai.aliyuncs.com/images/202311050932278.png)

- **Cgroups for Resource Management**: Cgroups ensure that containers cannot exceed their allocated resources, preventing a single container from monopolizing system resources and degrading performance for others. For instance, Docker containers can have their CPU or memory usage limited, ensuring they do not negatively impact the host or other containers.

- **Namespaces for Isolation**: Namespaces provide essential isolation, making each container feel like it's running on its own independent system. When Docker creates a container, it establishes separate namespaces for processes, networking, and file systems.

## Linux `init`

The primary role of the `init` process is to bring the system from a boot-up state to a fully operational state. It does this by reading a configuration file and spawning other processes according to the instructions in that file. These processes can include system daemons, user login shells, and other essential services.

- which is the first process started by the Linux kernel `(PID 1)` and remains active until shutdown. It orchestrates the boot sequence, manages services, handles dependencies, and maintains system stability.

> There are many `init` systems but `systemd` is most widely used `init` system in Linux Distributions.

- In systemd, the `systemctl` command is used to manage services.

```ini
# Start a service
sudo systemctl start httpd.service
# Stop a service
sudo systemctl stop httpd.service
# Restart a service
sudo systemctl restart httpd.service
# Enable a service to start at boot
sudo systemctl enable httpd.service
# Disable a service from starting at boot
sudo systemctl disable httpd.service
# check Status of service
systemctl status httpd.service
```

---

## [Linux Signals](https://www.bogotobogo.com/Linux/linux_process_and_signals.php)

A signal may be sent from the kernel to a process, from a process to another process, or from a process to itself. Signal typically alert a process to some event

Linux kernel implements about `30` different signals. Each signal identified by a number, from 1 to 31. Signals don't carry any argument

When the signal occurs, the process has to tell the kernel what to do with it. There can be three options through which a signal can be disposed :

1. The signal can be ignored. But not all signals can be ignored. Two signals `SIGKILL` and `SIGSTOP` cannot be ignored. This is because these two signals provide a way for root user or the kernel to kill or stop any process in any situation .The default action of these signals is to terminate the process. Neither these signals can be caught nor can be ignored.
2. Accept the default action, which may be to terminate the process, terminate and coredump the process, stop the process, or do nothing, depending on the signal.
3. Handled signals cause the execution of a user-supplied _signal handler function_. The program jumps to this function as soon as the signal is received, and the control of the program resumes at the previously interrupted instructions.

```ini
trap in bash is used to handle the signals

In python, The term raise is used to indicate the generation of a signal, and the term catch is used to indicate the receipt of a signal.
```

In Linux, every signal has a name that begins with characters `SIG`. For example:

- A `SIGINT` signal that is generated when a user presses _ctrl+c_. This is the way to terminate programs from terminal.
- A `SIGALRM` is generated when the timer set by _alarm_ function goes off.
- A `SIGABRT` signal is generated when a process calls the _abort_ function. etc

**If a process receives signals such as `SIGFPE, SIGKILL`, etc., the process will be terminated immediately, and a `core dump` file is created. The `core file` is an image of the process, and we can use it to debug.**

---

## Linux Scheduler

The Linux scheduler is the heart of the Linux operating system's ability to manage system resources efficiently. It determines which processes or threads should run at any given time, balancing the needs of different types of tasks such as CPU-bound and I/O-bound processes or runs on which CPU core and for how long.

**The kernel uses `scheduling classes` to manage different types of workloads efficiently.**

Linux uses multiple scheduling policies, including the default `Completely Fair Scheduler (CFS)` for normal tasks and `real-time policies` like **SCHED_FIFO** and **SCHED_RR** for time-sensitive tasks.

---

## Linux Memory Management

Memory management is the process of controlling and organising a computer’s memory by allocating memory to different executing programmes to improve the overall system performance.

### Different Types of Memories

1. **Physical Memory**: also known as `Random Access Memory RAM`, is the hardware compnent on the mother-borad that stores data and programs running by the system use. Linux divides physical memoery into pages. These pages are used to store various types of data, such as executable code, data structures and buffers.
   _ Used as Temporary storage for active programs and data. Its not the same as Hard Disk
   Location On motherboard
   _ There are two main types of RAM: **Dynamic RAM** is used for the main memory of the computer. Requires constant refreshing to maintain data. \* **Static RAM** is often used as cache memory. does not reqire refreshing and is faster and more expensive.
2. **Virtual Memory**: Virtual memory is a memory management technique used by operating systems to give the appearance of a large, continuous block of memory to applications, even if the physical memory (RAM) is limited and not necessarily allocated in contiguous manner. It is an [abstraction layer](existing-in-thought-or-as-an-idea-but-not-having-a-physical/concrete-existence) that allows each process to have it own address space.
   - Linux uses a technique called `paging` to map virtual memory addresses to physical memory pages.
   - When a process accesses a virtual address that is not currently in physical memory, a `page fault` occurs, and the operating system fetches the required page from disk (usually from a `swap space`).
   - Used in both kernel and user space
3. **Swap Space**: Swap space is a portion of the hard disk that is used as an extension of physical memory.
   - `Swapping` is a memory management technique where processes are temporarily moved between main memory and secondary storage to free up memory for other processes.
   - When the system runs out of physical memory, Linux moves less frequently used pages from physical memory to the swap space.
   - This allows the system to continue running processes even when there is not enough physical memory available.
4. **Caching**: Linux uses a significant amount of physical memory for caching.
   - The kernel caches frequently accessed data, such as file system blocks and disk I/O buffers, to improve system performance.
   - Cached data can be quickly retrieved from memory without having to access the disk again.
5. **Hard Disk**: hard disks serve as the primary storage medium for data, applications, and system files, enabling efficient data management and access.
   - Hard disks are essential for storing the operating system, applications, and user data.
   - Hard disks can be divided into multiple logical sections called partitions.
   - MOUNTING To access data on a hard disk, partitions must be mounted to the Linux file system. This process integrates the partition into the directory tree, allowing users to read and write data. UNMOUNTING is necessary before physically.
6. **Buffer**: a buffer is a reserved area of RAM used as temporary storage for data being transferred between two locations — typically between an application and a device, or between two devices.
   - Its main purpose is to compensate for speed differences between the sender and receiver, ensuring smooth and efficient data flow.
   - A buffer holds raw data exactly as received before it is processed or written, and it can also store data waiting to be sent. For example, when printing a large document, the system places the data into a buffer so the CPU can continue working without waiting for the printer to finish.

### Common Memory Issues in Linux

1. **Memory Leaks**: A memory leak occurs when a program fails to release allowed memory after use. Over time, this reduces available RAM, causing applications to slow down, crash, or require a system restart. Continuous memory leaks can also increase swap usage, further degrading system performance.
2. **Swap Thrashing**: Swap thrashing happens when the system frequently moves data between RAM and swap space due to insufficient physical memory. This can cause severe slowdowns, unresponsiveness, and excessive disk `I/O`.
3. **Out-of_Memory (OOM)**: When Linux runs out of memory, the OOM killer may terminate processes to free RAM. This can result in unexpected application crashes resulting in Error.

---

## Linux I/O

Kernel I/O Subsystem is a key component of the operating system kernel responsible for managing communication between the CPU and I/O devices (e.g., printers, disks, keyboards, network interfaces). It translates high-level I/O requests from applications into low-level hardware commands, handling concurrency, synchronization, error handling, buffering, caching, spooling and protection mechanisms.

![Device Access](https://applied-programming.github.io/Operating-Systems-Notes/images/typicaldeviceaccess.png)

I/O operation is any transfer of data between a computer’s memory (RAM/CPU) and external devices (e.g., disks, keyboards, printers, networks).

- In Linux, **I/O is file-centric** — the system abstracts/assumes almost all devices and data sources as “files,” making it easy to interact with them using consistent tools and APIs. Linux I/O is managed by the kernel

- **Input (I)**: Data flowing into the system (e.g., typing on a keyboard, reading a file from disk, receiving a network packet).
- **Output (O)**: Data flowing out of the system (e.g., displaying text on a screen, writing to a file, sending data over the network).

### Types of I/O in Linux

1. **Block Device**: These devices transfer data in fixed-size blocks, such as hard drives, solid-state drives (SSDs), and USB flash drives. Block devices are suitable for storing large amounts of data and are typically used for file systems.
2. **Character Device**: These devices transfer data one character at a time, such as terminals, serial ports, and printers. Character devices are often used for interactive input and output.
3. **Network I/O**: Network I/O involves data transfer over networks (e.g., HTTP, FTP, SSH). Unlike block/character devices, network I/O is connection-oriented (e.g., TCP) or connectionless (e.g., UDP) and uses sockets (software endpoints) instead of traditional files.

### [File Descriptors](https://notes.kodekloud.com/docs/Advanced-Bash-Scripting/Streams/File-descriptors/page)

Linux uses **file descriptors** (FDs) to identify I/O resources like files, sockets, and pipes. Each process typically has three standard file descriptors:

- `stdin (0)`, `stdout (1)`, and `stderr (2)`

<table><thead><tr><th>File Descriptor</th><th>Name</th><th>Purpose</th><th>Default Device</th></tr></thead><tbody><tr><td><code>0</code></td><td><code>stdin</code></td><td>Standard input</td><td>Keyboard (<code>/dev/stdin</code>)</td></tr><tr><td><code>1</code></td><td><code>stdout</code></td><td>Standard output</td><td>Terminal (<code>/dev/stdout</code>)</td></tr><tr><td><code>2</code></td><td><code>stderr</code></td><td>Standard error</td><td>Terminal (<code>/dev/stderr</code>)</td></tr></tbody></table>

Linux also allows I/O redirection, enabling commands to read input from files or write output to files instead of the default terminal. Operators like `<`, `>`, and `>>` are used to redirect standard input, output, and error streams

### Device Controllers

![cpu interaction](https://applied-programming.github.io/Operating-Systems-Notes/images/iointeractions.png)

- Device drivers are software modules that can be plugged into an OS to handle a particular device. Operating System takes help from device drivers to handle all I/O devices.
- The Device Controller works like an interface between a **device and a device driver**. I/O units (Keyboard, mouse, printer, etc.) typically consist of a mechanical component and an electronic component where electronic component is called the device controller.
- There is always a device controller and a device driver for each device to communicate with the Operating Systems. A device controller may be able to handle multiple devices. As an interface its main task is to convert serial bit stream to block of bytes, perform error correction as necessary.
- Any device connected to the computer is connected by a plug and socket, and the socket is connected to a device controller. Following is a model for connecting the CPU, memory, controllers, and I/O devices where CPU and device controllers all use a common bus for communication.

### I/O Modes

I/O models define how a process interacts with input/output devices or network sockets. They determine whether the process waits, polls, or is notified when data is ready, and how data is transferred between the kernel and user space.

1. **Synchronous I/O**: In synchronous I/O, the process issuing the I/O request waits until the operation is completed before continuing. This is the simplest and most straightforward way of performing I/O, but it can lead to significant performance degradation if the I/O operation takes a long time.
2. **Asynchronous I/O**: In asynchronous I/O, the process issuing the I/O request continues to execute while the I/O operation is being performed in the background. When the operation is completed, the process is notified. Asynchronous I/O can improve performance by allowing the process to continue doing other tasks while waiting for the I/O operation to finish.
3. **Non-blocking I/O**: In non-blocking I/O, the process issuing the I/O request does not wait for the operation to complete. Instead, it immediately returns with a status indicating whether the operation was successful or if it would have blocked. Non-blocking I/O is often used in combination with polling or event-driven programming to handle multiple I/O operations efficiently.

### I/O Buffering and Caching

I/O operations can be categorized as buffered or unbuffered based on how data is transferred between user space and the kernel:

1. **Buffering**: Buffering involves storing data in a buffer before it is written to or read from an I/O device.
   - This reduces the number of actual I/O operations by allowing the system to transfer data in larger chunks.
   - For example, when writing data to a file, Data is first written to the buffer; when the buffer is full (or explicitly flushed), it is sent to the kernel in one go, or in this case, the data is written to the disk.
2. **Caching**: Caching involves storing frequently accessed data in memory so that it can be retrieved more quickly.
   - For example, the Linux kernel maintains a buffer cache that stores recently accessed disk blocks in memory. When a process requests a disk block, the kernel first checks the buffer cache to see if the block is already in memory. If it is, the block can be retrieved from the cache without having to access the disk.

- Unbuffered I/O: Data is transferred directly between the application and kernel (no intermediate buffer in user space). Less efficient for small, frequent operations because each call requires a kernel context switch.

### Usage Methods

Linux provides several system calls for performing I/O operations.

Common system calls for performing I/O include `read()`, `write()`, `open()`, and `close()`.

- For example, `read(fd, buffer, size)` reads data from a file descriptor into memory, while `write(fd, buffer, size)` sends data from memory to a device or file

### Troubleshooting I/O issues

1. **Permission Denied**: You lack _read/write_ access to a _file/device_.
2. **Disk Full**: No space left to write data.
3. **I/O Errors**: Hardware problems (e.g., failing disk, loose cable).
4. **Slow I/O**: High Disk usage or inefficient operations.

Understanding I/O is crucial for system performance, programming, and effective use of Linux commands and applications.

---

## Virtualization

Virtualization is the process of running a virtual instance of a computer system in a layer abstracted from the actual hardware. Most commonly, it refers to running multiple operating systems on a computer system simultaneously. To the applications running on top of the virtualized machine, it can appear as if they are on their own dedicated machine, where the operating system, libraries, and other programs are unique to the guest virtualized system and unconnected to the host operating system which sits below it.

A `hypervisor`, also known as a **virtual machine monitor (VMM)**, is a crucial component in Linux virtualization. It is software/program that creates and runs virtual machines (VMs) on a physical host. There are two types of hypervisors:

1. **Type 1 (Bare-Metal Hypervisor)**: Runs directly on the host's hardware. Examples include VMware ESXi and Microsoft Hyper-V. In the Linux ecosystem, Xen can act as a Type 1 hypervisor.
2. **Type 2 (Hosted Hypervisor)**: Runs on a host operating system. For example, VirtualBox is a Type 2 hypervisor that can be installed on Linux, Windows, or macOS.

- **Host System**: The physical computer system that runs the hypervisor. It provides the underlying hardware resources such as CPU, memory, and storage to the virtualized environments.
- **Guest System**: A virtualized operating system instance that runs on top of the hypervisor. Each guest system has its own operating system, applications, and can function independently of other guests.

![virtualization](https://www.manageengine.com/network-monitoring/images/virtualization-architecture.png)

## Containerization

Containerization is a form of virtualization that allows you to package an application and its dependencies together in a "container." _Unlike virtual machines, containers share the host operating system kernel but maintain isolation_ for processes, libraries, and files. This lightweight isolation provides a portable, consistent environment across development, testing, and production.

![container](https://miro.medium.com/max/1156/1*beEpmEvS4ed56O0jbEjIzQ.jpeg)

The two primary kernel mechanisms enabling containerization are:

1. **`NAMESPACES` (Isolation Layer)**: Namespaces isolate different aspects of the operating system for each container, ensuring processes run in their own environment without interfering with others. Key namespaces types include:
   - `PID` – Separate process ID space so each container has its own process tree.
   - `NET` – Independent network stack with its own interfaces and routing tables.
   - `MNT` – Isolated filesystem mount points.
   - `IPC` – Separate inter-process communication resources.
   - `UTS` – Unique hostname and domain name per container.
   - `USER` – Distinct user and group IDs, allowing root inside a container without root on the host.
2. **`Control Groups` (Resource Management Layer)**: Cgroups limit, prioritize, and account for resource usage per container.
   - `CPU` – Limit processing power.
   - `Memory` – Restrict RAM usage.
   - `Block I/O` – Manage disk throughput.
   - `Network` – Control bandwidth allocation.

---

## Linux Daemons & Services

Services and daemons are the invisible workforce of your Linux system, they're background processes that quietly handle critical tasks like managing network connections, running web servers, scheduling automated jobs, and maintaining system logs without requiring any user interaction.

A `daemon/daemon process` is a long-running background process that is designed to perform specific tasks without user intervention. Daemons are typically started at system boot time and continue to run until the system is shut down. Daemon processes are started by the root user or root shell and can be stopped only by the root user.

### How Daemon Processes Work ?

When a daemon is started, it goes through a process called **"daemonization."** This involves detaching the process from the controlling terminal, changing its working directory to the root directory `(/)`, and redirecting standard input, output, and error streams to `/dev/null`. This ensures that the daemon runs in the background without being affected by user actions in the terminal.

![daemon](https://media.geeksforgeeks.org/wp-content/uploads/20250115122617728744/daemon.webp)

1. [Fork](it-duplicates-itself) a child process from the parent process. The parent process then exits.
2. Create a new session for the child process using the `setsid()` system call. This makes the child process the session leader and detaches it from the controlling terminal.
3. Change directory to / & Reset permissions: Prevents filesystem locking and sets proper file access using umask
4. Redirect I/O to /dev/null: Closes stdin, stdout, stderr to eliminate terminal interaction
5. Daemon runs: Process now operates independently in background without user interaction

> A daemon is usually created either by a process forking a child process and then immediately exiting, thus causing init to adopt the child process.
> Daemon can also be created by the init process directly launching the daemon.

Daemons in Linux usually follow a naming convention where the name ends with the letter **"d"**.

```ini
systemd: Manages system and service manager for Linux.
sshd: Handles incoming SSH connections.
httpd: Manages HTTP services for web servers.
crond: Schedules and executes tasks at specified times.
```

#### Services

A service is a broader term that describes any managed background program providing specific functionality to the system or users. Services can run continuously like daemons, or _they can be triggered on-demand and exit when their task completes_.​

- Provide specific functions like networking, printing, logging, or firewalls​
- Can start automatically at boot or be started manually​
- May run persistently or only when needed (event-driven)​
- Managed by service managers like systemd (modern Linux)

```ini
nginx.service : Web server service​
bluetooth.service : Only runs when Bluetooth devices are detected​
systemd-resolved.service : Handles DNS queries on-demand
```

<table><thead><tr><th style="text-align: left;"><span>Aspect</span></th><th style="text-align: left;"><span>Daemon</span></th><th style="text-align: left;"><span>Service</span></th></tr></thead><tbody><tr><td style="text-align: left;"><span>Definition</span></td><td style="text-align: left;"><span>Background process running continuously</span></td><td style="text-align: left;"><span>Managed background program (continuous or on-demand)</span></td></tr><tr><td style="text-align: left;"><span>Behavior</span></td><td style="text-align: left;"><span>Always runs in the background</span></td><td style="text-align: left;"><span>May run continuously or be triggered as needed</span></td></tr><tr><td style="text-align: left;"><span>User Interaction</span></td><td style="text-align: left;"><span>No user interaction, detached from terminal</span></td><td style="text-align: left;"><span>No direct user interaction, managed by service manager</span></td></tr><tr><td style="text-align: left;"><span>Examples</span></td><td style="text-align: left;"><span>sshd, httpd, crond</span></td><td style="text-align: left;"><span>nginx.service, bluetooth.service, one-shot tasks</span></td></tr><tr><td style="text-align: left;"><span>Terminology</span></td><td style="text-align: left;"><span>Unix/Linux term</span></td><td style="text-align: left;"><span>Universal term, used by systemd</span></td></tr></tbody></table>

---

## [File System Management](https://linux-kernel-labs.github.io/refs/heads/master/lectures/fs.html)

A fileystem is a way to organize files and directories on storage devices such as hard disks, SSDs or flash memory. There are many types of filesystems (e.g. FAT, ext4, btrfs, ntfs) and on one running system we can have multiple instances of the same filesystem type in use.

While filesystems use different data structures to organizing the files, directories, user data and meta (internal) data on storage devices there are a few common abstractions that are used in almost all filesystems:

- **superblock**: abstraction contains information about the filesystem instanc esuch as the block size, the root inode, filesystem size. It is present both on storage and in memory(for caching purpose).
- **file**: file abstraction contains information about an opened file such as the current file pointer. It only exists in memory.
- **inode**: is identifying a file on disk. It exists both on storage and in memory (for caching purposes). An inode identifies a file in a unique way and has various properties such as the file size, access rights, file type, etc.
- **dentry**: The **dentry associates a name with an inode**. It exists both on storage and in memory (for caching purposes).

![file](https://linux-kernel-labs.github.io/refs/heads/master/_images/ditaa-29f54aaa1a85b819ff29cb7d101a4d646b3b0b06.png)

---

## Distributed File System

A **Distributed File System (DFS)** allows files to be stored across multiple servers while appearing as a single unified storage to users and applications. This architecture improves scalability, redundancy, and fault tolerance, making it ideal for large-scale data environments.

A distributed file system is a client/server-based application that allows clients to access and process data stored on the server as if it were on their own computer. When a user accesses a file on the server, the server sends the user a copy of the file, which is cached on the user’s computer while the data is being processed and is then returned to the server.

## ![dfs](https://softwaresystemdesign.com/assets/images/distributed-file-systems-078275f1761b3511d4c373f3e33d4788.png)

#### Startup Management (init.d)

`init.d` is a daemon, which is the first process (PID 1) for a Linux system. Other processes, services, daemons, and threads are started by init. You can write your own scripts in /etc/init.d to start on the system boot.

#### Service Management (systemd):

- It replaces the `sysvinit` process to become the first process with PID=1. It replaces `init.d`. It uses `systemctl` command to perform related operations.

---

## Summary

![arch1](../../resources/assets/arch3.png)

Linux is built as a set of concentric layers, and there is exactly one hard wall running through the middle of that stack — the boundary between user space and kernel space. Nothing in user space is allowed to touch hardware directly. The only way across that wall is through a narrow, guarded doorway called the system call interface.

![arch1](../../resources/assets/arch1.png)

1. very top sit applications — the programs you actually want to run.
2. Just below them are the shell and system utilities.
   - The shell (bash, zsh) is itself just an ordinary user program whose special job is to read your commands and launch other programs; the GNU core utils _(ls, cp, grep)_ and background daemons like `systemd` and `sshd` live at this same level.
   - This is a crucial realization: _the shell is not part of the kernel_ and has no special powers. It's a regular citizen of user space, which is why you can swap bash for zsh or fish without touching the operating system itself.
   - People often say "Linux" to mean this whole assembly, but strictly speaking, **Linux is only the kernel** — the shell, the utilities, and the libraries are the GNU project's contribution, which is why purists call the system "GNU/Linux."
3. Below the utilities are the **system libraries**, chiefly the GNU C Library (`glibc GNU C Library`).
   - It serves as the critical interface between _user-space applications and the Linux kernel_, providing essential functions for memory allocation, input/output operations, string handling, and process control.
   - When your program calls _printf() or malloc(), read(), write()_, it's calling into `glibc`. The library does the convenient bookkeeping and, when it actually needs the kernel to do something privileged — like reading a file or allocating memory pages — it issues the appropriate system call.
   - Most programs never invoke a raw system call directly; they go through the library, which is the polite, portable wrapper.
4. The system call interface is the single controlled entry point into the kernel. Above it, code runs in ring 3 (user mode), an unprivileged CPU state where a program can only touch its own memory and cannot execute privileged instructions.
   - When a program makes a system call, the CPU performs a controlled trap: it switches from ring 3 to ring 0, jumps to a fixed, kernel-defined entry point, does the privileged work, and then switches back.
   - This is the entire reason a buggy or malicious application can't crash the machine or read another program's memory: it literally lacks the CPU privilege to do so, and the only way it can ask for privileged work is by knocking on this one door, where the kernel checks every request.
5. Kernel is a collection of cooperating subsystems, each owning one category of resource. Linux uses a monolithic kernel design, meaning all of these subsystems run together in the same privileged address space.

![arch1](../../resources/assets/arch2.png)

- The **process scheduler** is the kernel's time-sharing brain. Your CPU can physically run only one process per core at a time, yet you have hundreds running. The scheduler decides which process gets the CPU next and for how long, switching between them so fast (a context switch) that they all appear to run simultaneously.
- The **network stack** implements the TCP/IP protocols and the socket abstraction. When your browser opens a connection, it creates a socket; the kernel handles the layered work of packets, routing, retransmission, and delivery, then hands the data stream up to your program as if it were a file you can read and write.
- The **virtual file system (VFS)** is one of the most elegant ideas in Linux. It's an abstraction layer that presents a single, uniform interface — open, read, write, close — regardless of what's actually underneath. Below the VFS sit the real filesystem drivers: `ext4, Btrfs, XFS, FAT, NFS`for network shares, and so on. Because everything goes through the VFS, the same `read()` call works whether the bytes come from an _SSD, a USB stick, or a remote server_. This is also the machinery behind the `"everything is a file"` philosophy — devices, pipes, and even kernel state in `/proc` are all exposed through this same file interface.
- **Inter-process communication** (IPC) is the set of mechanisms that let isolated processes — which, remember, can't see each other's memory — cooperate when they need to. This includes _pipes_ (the magic behind `ls | grep foo`), signals (like the `SIGINT` sent when you press Ctrl-C), shared memory segments, and message queues.
- **Device drivers** are the kernel's translators to the physical world. Each driver knows how to speak the specific protocol of a piece of hardware — a disk controller, a GPU, a network card — and exposes it to the rest of the kernel through standard interfaces. This is the layer that actually touches the metal, and it's the largest part of the kernel by code volume.

Learn about [What Happens when you Type the Command ?](../../practice/concepts/What-happens-when-you-execute-command.md)

---

## What is core Difference between the Linux and Linux Distro ?

Linux is just the kernel: the core that manages the CPU, memory, devices, and processes. By itself it's not usable; you can't do anything with a bare kernel.

A distribution (distro) is the kernel plus everything bundled around it to make a complete, usable operating system: the GNU utilities and shell, system libraries, a package manager, default configuration, and often a desktop environment. So Ubuntu, Fedora, Debian, and Arch **all run essentially the same Linux kernel** — what differs is the packaging on top.

- "Linux is the kernel; a distribution is the kernel combined with the tools, libraries, and software needed for a working OS."

#### How Distro differ from each other

- **Package manager / format** — `apt` (.deb) on Debian/Ubuntu, `dnf` (.`rpm`) on Fedora/RHEL, `pacman` on Arch.
- **Release model**— fixed/stable releases (Debian, Ubuntu LTS) vs. rolling release (Arch).
- Target audience & defaults — preinstalled software, default desktop environment, and philosophy (e.g. Ubuntu for ease of use, Arch for control, RHEL for enterprise).

> The kernel is the same engine; distributions are different cars built around it.

---
