# Operating Systems Concepts for DevOps

### What is an Operating System?
* Interface between Hardware and Software applications/users.
* Hides hardware complexity (Read/write file storage, send/receive socket network)
* Handles resource management (CPU scheduling, Memory management)
* Provide isolation and protection (allocate different parts of memory to different applications so that applications don't overwrite other memory locations)

An Operating System is a layer of systems software that:
- directly has privileged access to the underlying hardware;
- hides the hardware complexity;
- manages hardware on behalf of one or more application according to some predefined policies.
- In addition, it ensures that applications are isolated and protected from one another.

![os](https://openstax.org/apps/image-cdn/v1/f=webp/apps/archive/20260407.195030/resources/ca3ac00c14a30b4b814bdd3caf7f38520d8af865)
---
There are 3 key elements of an operating system, 
1. **Abstractions**: corresponds to applications that OS executes i.e, ability to hide the complexities of hardware and software while providing a simple interface for users. This abstraction allows for the management of resources like memory, storage and processors without the need to deal with the underlying hardware details. 
    * *process, thread, file, socket, memory*
2. **Mechanisms**: Algorithms and processes that enable the OS to function effectively. These mechanisms are essential for managing system resources, executing programs, and providing a user interface. 
    * *(create, schedule, open, write, allocate)*
3. **Policies**:  Define how mechanisms are used to manage underlying hardware.
    * *Least Recently Used*(LRU), *Earliest Deadline First*(EDF)

```ini
Memory Management:

Abstractions: Memory page
Mechanisms: Allocate, map to a process
Policies: LRU
```
---
## Kernel in Operating System

A **kernel** is the core part of an operating system. It acts as a brdige between software applications and the hardware of a computer. 
* The kernel manages system resources, such as the CPU, memory and devices, ensuring everything works together smoothly and efficiently.
* It handles tasks like running programs, accessing files and connecting to devices like printers and keyboards.

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

* Kernel if the first part of the OS loaded into memory during boot, and it stays resident while the system is running. 
* It operates in a privileged mode (kernel mode), separate from user mode for applications; user apps can’t directly access hardware or critical resources.
* Applications make requests to the kernel via system calls (or software interrupts). The kernel handles these by switching from user mode to kernel mode.
* Kernel executes the requested operation (e.g. file I/O, process creation, memory allocation).

```ini
Think of a computer like a city. User mode is like the public streets where everyday people (applications) can move around but can’t directly control traffic lights or power grids. 
Kernel mode is like the city’s control center, where operators have full access to every system — traffic, electricity, water — and can make changes instantly.

When your computer runs, most programs operate in user mode for safety. They can’t directly touch hardware or critical system memory. 

If they need something like reading a file or sending data to a printer, they make a system call — a formal request to the kernel. 
The CPU then switches into kernel mode to perform the task, and switches back when done.
```

#### Kernel Space vs User Space
* Kernel space is the reserved memory region where the operating system kernel runs. The kernel is the core of the OS, responsible for managing hardware resources (CPU, memory, disk, network), enforcing security policies, and coordinating system-wide operations.
- Code running in kernel space has unrestricted access to the system. It can directly manipulate hardware, access any memory address, and execute privileged CPU instructions

* User space is the memory region where user applications (e.g., web browsers, text editors, games) execute. Unlike kernel space, user space is restricted: applications here cannot directly access hardware or modify critical system resources.
- Code in user space runs with limited permissions. It can only access its own allocated memory and must request kernel assistance (via "system calls") to interact with hardware or perform privileged operations (e.g., reading a file from disk).

#### Kernel Mode vs User Mode
A process alternates between two execution modes:
- **User Mode**: Most of the time, the process runs in user mode, executing application code with limited privileges.
- **Kernel Mode**: When the process needs a privileged operation (e.g., reading a file), it triggers a system call, switching to kernel mode. The kernel executes the operation and returns control to user mode.
---

## System Call

User programs cannot directly access hardware or critical OS resources because it would make the system unstable and insecure, hence they operate in *user mode*. The OS provides **system calls** -- controlled interfaces that allow user programs to request services from kernel. 

These calls act as a gateway between user mode and kernel mode. System Calls are, **A way for programs to interact with the operating system.**

![systemcalls](https://media.geeksforgeeks.org/wp-content/uploads/20250124123632394421/introduction_to_system_call.webp)

#### How do System Calls Work?
A system call is a controlled entry point that allows a user program to request a service from the operating system. Here's how it works:

* The user program executes a system call instruction (e.g., using syscall or int 0x80).
* The CPU switches from user mode → kernel mode for safe execution.
* The kernel identifies the system call number and performs the requested operation (file access, process creation, memory allocation, etc.).
* After completing the task, the kernel switches back to user mode.
* The result (success/failure/data) is returned to the program.
* Without system calls, every program would need its own way to access hardware, leading to inconsistent and insecure systems.

#### Types of System Calls

![types of system calls](https://media.geeksforgeeks.org/wp-content/uploads/20251202114956548681/frame_11.webp)

* **File System**: Used to create, open, read, write, and manage files and directories.
* **Process Control**: Used to create, execute, synchronize, and terminate processes.
* **Memory Management**: Used to allocate, deallocate, and manage memory for processes.
* **Interprocess Communication (IPC)**: Used for data exchange and communication between different processes.
* **Device Management**: Used to request and release devices, and to perform read/write operations on them.

[WHAT HAPPENDS WHEN YOU TURN ON THE COMPUTER](../../practice/concepts/What-happens-when-we-turn-on-computer.md)

---
## Process in Operating System
* A process is a program in execution. When we write source code in C or C++ and compile it, the compiler generates an executable binary file. This executable file is called a **program**. When the program is loaded into memory and executed, it becomes a `process`.

* When loaded into memory, a process is divided into sections:
1. **Text/Code Segment** – contains executable instructions. It is typically a read only section.
2. **Data Segment** – Global and static variables.
3. **Stack** -  Function calls, local variables, return addresses.
4. **Heap** – Dynamically allocated memory during runtime

### Attributes of a Process
A process has several important attributes that help the operating system manage and control it. These attributes are stored in a structure called the `Process Control Block (PCB)` (sometimes called a task control block). The PCB keeps all the key information about the process, including:

* **Process ID (PID)**: A unique number assigned to each process so the operating system can identify it.
* **Process State**: This shows the current status of the process, like whether it is running, waiting, or ready to execute.
* **Priority** and other CPU Scheduling Information: Data that helps the operating system decide which process should run next, like priority levels and pointers to scheduling queues.
* **I/O Information**: Information about input/output devices the process is using.
* **File Descriptors**: Information about open files and network connections.
* **Accounting Information**: Tracks how long the process has run, the amount of CPU time used, and other resource usage data.
* **Memory Management Information**: Details about the memory space allocated to the process, including where it is loaded in memory and the structure of its memory layout (stack, heap, etc.).

These attributes in the PCB help the operating system control, schedule, and manage each process effectively.

---
## Process Management
Process management is a core function of an Operating System (OS). It deals with creating, scheduling, and coordinating processes to ensure efficient CPU utilization and smooth system performance.

![process](https://media.geeksforgeeks.org/wp-content/uploads/20260227113344202746/tasks_of_process_managementjjh.jpg)
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

* Processes are usually independent, while threads exist as subsets of a process.
* Each process has its own memory space. Threads that belong to the same process share the same memory.
* A process is a heavyweight operation. It takes more time to create and terminate.
* Inter-thread communication is faster for threads.

#### Components of Threads
These are the basic components of the Operating System.
- **Stack Space**: Stores local variables, function calls, and return addresses specific to the thread.
- **Register Set**: Hold temporary data and intermediate results for the thread's execution.
- **Program Counter**: Tracks the current instruction being executed by the thread.

#### Types of Threads
Threads are mainly classified based on how they are managed and scheduled in an operating system. There are two primary types of threads.

![threads](https://media.geeksforgeeks.org/wp-content/uploads/20260204151901952105/user_level.webp)

1. **Kernel Threads**: Kernel threads are managed directly by the OS kernel. They are scheduled by the kernel’s process scheduler and have full access to kernel resources.
   * Kernel threads handle background tasks like flushing disk caches (pdflush in Linux) or managing network buffers.
2. **User Threads**: User threads are managed by user-space libraries (e.g., POSIX Pthreads, Windows Fibers) rather than the kernel. They are lighter and faster to create but depend on kernel threads for execution.

[Threads and Concurrency](https://applied-programming.github.io/Operating-Systems-Notes/3-Threads-and-Concurrency/)

---

## Resource
https://github.com/Tikam02/DevOps-Guide/blob/master/OS/os-concepts.md#boot-process
