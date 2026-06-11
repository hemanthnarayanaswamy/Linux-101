# Job Control
In the Linux operating system, *jobs refer to processes that are running in the background or foreground*. 

Job control refers to the ability to *manipulate these processes*, including suspending, resuming, and terminating them. This feature enables users to manage multiple tasks efficiently and debug process-related issues.

Job control is made possible by the `shell`

- **Foreground jobs** Run interactively in the current terminal session and block further input until completion
- **Background jobs** Run independently without blocking the terminal, allowing users to continue entering commands

### Job States
Jobs reported by the [jobs](../commands/jobs.md) command can be in one of these states:

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