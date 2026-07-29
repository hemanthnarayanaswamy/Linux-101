# The `ssh` command 

`ssh` is used to log in to a remote machine securely.

The `sshpass` command is a utility designed to facilitate non-interactive SSH password authentication. It is particularly useful for automating SSH logins in scripts, where manual password entry is not feasible.

```bash
ssh [username]@[hostname_or_IP] # connect as a specific user

ssh -p [port] [username]@[hostname_or_IP] # connect to a non-standard port

ssh -i ~/.ssh/id_ed25519_prod deploy@app.example.com # use a specific private key (better use ssh config)

ssh ubuntu@server "df -h" # Run one command remotely
ssh user@remote_host "command_to_run"
ssh user@remote_host "cd /var/log && ls -lh" # Multi logic commands

ssh -v user@server
ssh -vv user@server
ssh -vvv user@server # Debug connection problems
```
A **TTY (teletypewriter)** represents an interactive terminal session. 

In SSH, allocating a pseudo-terminal (PTY) connects your local input and output streams to the remote shell, enabling programs to prompt for input, display interactive menus, or handle signals like Ctrl-C correctly.

Allocating a TTY in SSH allows interactive commands to run properly, while non-allocation is safer for scripts and non-interactive commands.

**Some commands expect an interactive terminal:** `-tt` Forces SSH to allocate a pseudo-terminal, called a *PTY*

```bash
#-t means: allocate a remote TTY.

# -tt in ssh forces allocation of a pseudo-terminal on the remote server.
#-tt means: force it more strongly, even when ssh thinks it is running non-interactively, such as inside a script.

ssh -tt user@server "sudo useradd rajesh"
# Run command directly into remote server

sudo
su
top
htop
vim
nano
passwd
ssh inside ssh
interactive scripts
```
**Without a PTY, they may fail with errors like**: 
```ini
sudo: no tty present
Pseudo-terminal will not be allocated because stdin is not a terminal
```
---
# The `sshpass` Command

The `sshpass` command is a utility designed to facilitate non-interactive SSH password authentication. 
    - It is particularly useful for automating **SSH logins in scripts**, where **manual password entry is not feasible**. 

> `sshpass` = automate password-based SSH
> `sshpass` is usually a bad idea. For real DevOps/SRE work, prefer SSH keys, SSO, Vault, cloud-native access, or short-lived credentials.

Normal SSH password login expects an interactive terminal prompt: `user@server's password:`. That is hard to automate in scripts. `sshpass` fakes that interaction and feeds the password to SSH.

```bash
sshpass -p 'your_password' ssh -tt username@hostname  "cd /var/log; ls -lh"
```
Password may appear in:
- shell history
- process list
- logs
- CI output
- terminal scrollback
- `ps aux`

1. Use Environment Variables 
```bash
export SSHPASS='mypassword'
sshpass -e ssh user@server

# -e: read password from SSHPASS environment variable
```
2. Use Password file
```bash
chmod 600 ssh_password.txt
sshpass -f ssh_password.txt ssh user@server

# -f reads password from a file.
```

### First Time Connection
First SSH connection may ask: `Are you sure you want to continue connecting?` which throws error in the script if its the first time. 

* use `sshpass -e ssh -o StrictHostKeyChecking=no user@server` but not recommended as this weakens security because it accepts unknown host keys automatically. 

Safe approach using `ssh-keyscan`, is a command-line utility used to gather SSH public host keys from one or multiple servers without requiring login access.
> It’s particularly useful for building or updating the `~/.ssh/known_hosts` file in automated scripts or deployment pipelines.

```bash
ssh-keyscan github.com >> ~/.ssh/known_hosts

ssh-keyscan 192.168.1.20 >> ~/.ssh/known_hosts

# specify the key type
ssh-keyscan -t ed25519 github.com >> ~/.ssh/known_hosts

# All key types
ssh-keyscan -t rsa,ecdsa,ed25519 -f test_server >> ~/.ssh/known_hosts
```