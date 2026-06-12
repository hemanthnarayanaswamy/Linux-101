# Job Control
In the Linux operating system, *jobs refer to processes that are running in the background or foreground*. 

Job control refers to the ability to *manipulate these processes*, including suspending, resuming, and terminating them. This feature enables users to manage multiple tasks efficiently and debug process-related issues.

Job control is made possible by the `shell`

- **Foreground jobs** Run interactively in the current terminal session and block further input until completion
- **Background jobs** Run independently without blocking the terminal, allowing users to continue entering commands

### Job States
Jobs reported by the [jobs](../../commands/process-management/jobs.md) command can be in one of these states:

- **Running** — the process is actively executing in the background.
- **Stopped** — the process has been suspended, typically by pressing Ctrl+Z.
- **Done** — the process has finished. This state appears once and is then cleared from the list.
- **Terminated** — the process was killed by a signal.

When a background job finishes, the shell displays its Done status the next time you press Enter or run a command.

### Job Control Commands

<table>
<thead>
<tr>
<th>Command</th>
<th>Purpose</th>
<th>Example Usage</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>jobs</code></td>
<td>List active jobs</td>
<td><code>jobs -l</code></td>
</tr>
<tr>
<td><code>fg</code></td>
<td>Bring job to foreground</td>
<td><code>fg %1</code></td>
</tr>
<tr>
<td><code>bg</code></td>
<td>Send job to background</td>
<td><code>bg %1</code></td>
</tr>
<tr>
<td><code>kill</code></td>
<td>Terminate job</td>
<td><code>kill %1</code></td>
</tr>
<tr>
<td><code>Ctrl+Z</code></td>
<td>Suspend foreground job</td>
<td>Interactive keystroke</td>
</tr>
<tr>
<td><code>Ctrl+C</code></td>
<td>Terminate foreground job</td>
<td>Interactive keystroke</td>
</tr>
</tbody>
</table>

### Job Identification / Job Specification
Job specifications (job specs) let you target a specific job when using commands like `fg, bg, kill , or disown`. They always start with a percent sign `(%)`:

- `%1`, `%2`, … — refer to a job by its number.
- `%%` or `%+` — the current job (the most recently backgrounded or suspended job, marked with +).
- `%-` — the previous job (marked with -).
- `%string` — the job whose command starts with string. 
    * For example, `%ping` matches a job started with `ping google.com`.
- `%?string` — the job whose command contains string anywhere. 
    * For example, `%?google` also matches `ping google.com.`

### Detail Analysis 
A foreground job owns the terminal: It receives your keystrokes, and the shell waits for it. Only one job can be foreground at a time.
- The shell lets you juggle multiple programs from one terminal by moving them between three state. 

In Linux `shell` job control, a `job` is a command or pipeline that your shell is tracking.

```bash
sleep 100 # This is one foreground job.

sleep 100 & # This is one background job.
```
* A job may contain one process or multiple processes in a pipeline:
```bash
sleep 100 # one process

cat file.txt | grep error | sort  # Multiple Process 
# The whole pipeline is treated as one shell job
```
#### Foreground vs Background
* Foreground Means: This job owns the terminal right now.
    * Only the foreground job receives keyboard input from the terminal.
* Background Means: This job is still running, but it does not own the terminal.
    * If a job is in the background, your shell keeps control of the terminal.

```ini
A process is a running program.
A process group is a group of related processes.
A session is a group of process groups, usually tied to one login terminal.
A controlling terminal is the terminal connected to your shell.
The shell uses these to manage jobs.

For a foreground command:
  shell gives terminal to job
  shell waits
  job exits/stops
  shell takes terminal back

For a background command:
  shell starts job
  shell does not give terminal to job
  shell immediately gives you prompt back
```

#### Common Commands 

```bash
jobs ## show shell jobs

bg %1  ## Move a stopped job to the background

fg %1 ## Move job to foreground

Ctrl+Z  ## stop the foreground job

kill %1  # Kill a job
```
* When you press `ctrl+z` the terminal sends the signal `SIGTSTP` to stop/suspend this process. The process/job gets paused. Its not killed. Its frozen. 
* The shell then records: `job 1 is stopped` and that is why `jobs` command can show it. 
* with `bg %1` the shell sends `SIGCONT` to that job. Continue running. But the shell does not give the terminal back to that job. So the job runs in the background. 
* With `fg %1` the shell gives terminal control to that job and sends `SIGCONT` if the job was stopped. `fg` make this job the terminal owner again

### CPU priority 

> Foreground/background is about terminal control. Dosn't mean one gets more CPU then other.

For **CPU Priorities**, Linux uses things like
```bash
nice
renice
cgroups
systemd slices
```

### Debug Commands

Run this to see process groups and terminal ownership:
```bash
ps -o pid,ppid,pgid,sid,tpgid,stat,tty,cmd
```

* **PID**    process ID
* **PPID**   parent process ID
* **PGID**   process group ID
* **SID**    session ID
* **TPGID**  foreground process group ID of the terminal
* **STAT**   process state
* **TTY**    terminal
* **CMD**    command

> If `PGID == TPGID` then its process group is currently in the foreground for that terminal.

## Handling the Background jobs after logout

Background job does not mean independent job. The Background jobs are still connected to:
    * your shell
    * you login shell
    * your terminal 
    * stdin/stdout/stderr
**So when you log out, it may still die**

```ini
terminal/SSH session
    |
    shell: bash/zsh
        |
        foreground/background jobs

when logged out:
- terminal disconnects
- shell exits or receives SIGHUP
- shell sends SIGHUP to its jobs
- jobs may terminate
Most processes terminate when they receive SIGHUP, unless they handle or ignore it.
```
**Assume if a background job keeps writing to your terminal and you log out, its output target disappers. Even if it survives `SIGHUP`, it may still fail when writing to `stdout, stderr`. That is why long-running background jobs should redirect output. `python script.py > script.log 2>&1 &`**

> `nohup` is proactive

It starts a command while telling it to ignore `SIGHUP`
    * Now after logout, It ignore `SIGHUP` and if the redirect output is not provided then automatically redirects outputs to `nohup.out`
    * `nohup ./backup.sh > backup.log 2>&1 &`

> `disown` is reactive
It removes a job from the shell’s job table. So when the shell exits, it will not send `SIGHUP` to that job. and `jobs` command no longer lists it. 

|Tool	|What It Does	|When To Use|
|-------|---------------|---------------|
|nohup	|Starts a command that ignores SIGHUP	|Use before starting a long command|
|disown|	Removes an already-running job from shell job control|	Use after starting a job|
|&	|Runs command in background	|Does not protect from logout by itself|
|output redirection|	Detaches stdout/stderr from terminal|	Needed for reliable logout survival|
