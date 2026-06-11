# Signals in Depth

A signal is an asynchronous notification -- **essentailly a software interrupt** used to notify a *process* that a specific event has occurred. They can be sent by the *OS, other processes, or the process* itself.

Each signal has a `number`, a `name` and a default `action`. For most signals, a process can do one of three things
* Let the default happen
* Catch it (run a handler function to do cleanup or custom logic) -- using [trap](../concepts/signals.md)
* Ignore it
* It can also temporarily block(queue) a signal. Blocked signals remain pending until unblocked.

![signal](https://blog.paolorossi.net/assets/images/post/linux/Linux-Signals.png)

### Different Actions of Signals

1. `Term` = **terminate process**
2. `Core` = **terminate and create core dump**
3. `Ign`  = **ignored by default**
4. `Stop` = **pause/suspend process**
5. `Cont` = **continue a stopped process**

## Important Signals and Usecases

| Signal | Value | Default Action | What It Does In Simple Terms | SRE/DevOps Use |
|:-------|:-----:|:---------------|:-----------------------------|:---------------|
| SIGHUP | 1 | Term | Terminal/session closed; many daemons treat it as reload | Reload config or reopen logs without full restart |
| SIGINT | 2 | Term | Interrupt from keyboard, usually Ctrl+C | Stop a foreground command safely |
| SIGQUIT | 3 | Core | Quit and dump debug info/core | Debug stuck processes; JVM uses kill -3 for thread dump |
| SIGILL | 4 | Core | Illegal CPU instruction | Bad binary, wrong CPU architecture, corrupted program |
| SIGTRAP | 5 | Core | Debugger trap/breakpoint | Used by debuggers, tracing, breakpoints |
| SIGABRT | 6 | Core | Process aborts itself | App crashed intentionally after fatal error/assertion |
| SIGBUS | 7 | Core | Bad memory access at hardware/filesystem level | mmap errors, disk-backed memory problems, alignment issues |
| SIGFPE | 8 | Core | Arithmetic error | Divide-by-zero or floating-point exception |
| SIGKILL | 9 | Term | Force kill immediately | Last resort; process cannot clean up or catch it |
| SIGUSR1 | 10 | Term | User-defined signal 1 | App-specific action; often log reopen, metrics dump, reload |
| SIGSEGV | 11 | Core | Invalid memory access | Segmentation fault; common native crash signal |
| SIGUSR2 | 12 | Term | User-defined signal 2 | App-specific action; sometimes graceful upgrade/reload |
| SIGPIPE | 13 | Term | Write to closed pipe/socket | Happens when client disconnects or pipeline reader exits |
| SIGALRM | 14 | Term | Timer expired | Used for timeouts inside programs |
| SIGTERM | 15 | Term | Polite request to stop | Standard graceful shutdown signal; use before SIGKILL |
| SIGCHLD | 17 | Ign | Child process exited/stopped | Supervisors use it to reap child processes and avoid zombies |
| SIGCONT | 18 | Cont | Resume a stopped process | Continue a process paused by SIGSTOP or Ctrl+Z |
| SIGSTOP | 19 | Stop | Force pause immediately | Uncatchable pause; useful for emergency freezing/debugging |
| SIGTSTP | 20 | Stop | Terminal suspend, usually Ctrl+Z | Pause foreground jobs in a shell |
| SIGWINCH | 28 | Ign | Terminal window size changed | Terminal apps resize; some daemons use it for custom control |

> `SIGKILL` and `SIGSTOP` cannot be caught, blocked, or ignored.

## Usage Methods

The kill command sends a signal to specified *processes or process groups*, causing them to act according to the signal. 
    * When the signal is not specified, it defaults to `-15` (`-TERM`). 

The basic syntax is:
```bash
kill -<signal> <pid>

kill -SIGTERM 1234   # Specify the signal name
kill -15 1234        # You can also use the signal number instead of name
```

The most commonly used signals are:
- `1 (HUP)` - Hangup, often used by daemons to reload configuration.
- `9 (KILL)` - Kill a process immediately.
- `15 (TERM)` - Gracefully stop a process.

> Refer to go in detail about [kill](../commands/kill.md)
---
#### Why `kill -9` actually bypasses

`SIGKILL` is handled entirely by the kernel; the target process never gets to execute a single instruction in response. 
- So `kill -9` bypasses the program's own cleanup: no handler runs, so buffers aren't flushed (data loss), files and network connections aren't closed cleanly, locks and lock files aren't released (stale locks), shared memory and semaphores can be orphaned, and child processes can be left running. 
- It also bypasses any attempt by the process to ignore or block the signal. 

#### It doesn't bypass Kernel itself
`SIGKILL` cannot remove a process stuck in **uninterruptible sleep (`D state`)**.

Example: Blocked inside a kernel call waiting on a hung disk or dead NFS mount. 

The kill is recorded as pending and is only delivered when the proceess leaves `D state`, which may be never.
> This is precisely why people say **I run `kill -9` and nothing happened.**.

The correct habit is escalation: **Send `SIGTERM`, give it a few seconds, and only then resort to `SIGKILL`** - which is exactly what *Init* systems do internally.

---
## Common Practices

#### Terminating a Process Gracefully
When you need to terminate a process, it is recommended to send a `SIGTERM` signal first. 
* This allows the process to perform any necessary cleanup operations before terminating. 
* If the process does not respond to `SIGTERM` within a reasonable time, you can send a `SIGKILL` signal to force it to terminate.

```bash
# Send SIGTERM
kill -SIGTERM <pid>
# Wait for a few seconds
sleep 5
# If the process is still running, send SIGKILL
kill -SIGKILL <pid>
```
#### Find and Kill Zombie Processes
A process in Linux can have one of the following states:

```bash
D = uninterruptible sleep
I = idle
R = running
S = sleeping
T = stopped by job control signal
t = stopped by debugger during trace
Z = zombie
```

```bash
ps -axo stat,pid,comm | grep -w Z

#lists all processes (-a for all users, -x for processes without a controlling terminal) and 
# displays the process status (stat), process ID (pid), and command name (comm).
# grep -w Z filters the output to show only processes with a status of Z, which indicates a zombie process.
```
**you cannot directly kill a zombie process because it is already dead. However, you can kill its parent process, which will cause the init process (PID 1) to adopt the zombie processes and clean them up.**

First, identify the `parent process ID (PPID)` of the zombie process using the [ps](../commands/ps.md) command, The use [kill](../commands/kill.md) to send a singal to the parent process. 

```bash
kill -TERM <parent_pid>  # a polite termination request

kill -KILL <parent_pid>  # If parent process does not respond use forcefull termination
```