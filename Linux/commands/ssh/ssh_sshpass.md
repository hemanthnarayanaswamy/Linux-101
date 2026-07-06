
The `sshpass` command is a utility designed to facilitate non-interactive SSH password authentication. It is particularly useful for automating SSH logins in scripts, where manual password entry is not feasible.

```bash
sshpass -p 'your_password' ssh -tt username@hostname  "cd /var/log; ls -lh"

ssh user@remote_host "command_to_run"

ssh user@remote_host "cd /var/log && ls -lh"
```

A **TTY (teletypewriter)** represents an interactive terminal session. 

In SSH, allocating a pseudo-terminal (PTY) connects your local input and output streams to the remote shell, enabling programs to prompt for input, display interactive menus, or handle signals like Ctrl-C correctly.

Allocating a TTY in SSH allows interactive commands to run properly, while non-allocation is safer for scripts and non-interactive commands.

```bash
#-t means: allocate a remote TTY.

# -tt in ssh forces allocation of a pseudo-terminal on the remote server.
#-tt means: force it more strongly, even when ssh thinks it is running non-interactively, such as inside a script.

ssh -tt user@server "sudo useradd rajesh"
# Run command directly into remote server
```