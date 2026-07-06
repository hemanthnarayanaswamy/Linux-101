# Introduction to SSH

`SSH`, or Secure Shell, is a cryptographic network protocol primarily used for secure network services over an unsecured network. SSH provides strong encryption and integrity checks. It is widely used for:

- Remote command-line login
- Remote command execution
- Secure file transfer using `SCP` _(Secure Copy Protocol)_ or `SFTP` _(Secure File Transfer Protocol)_
- Tunneling and forwarding of ports
- GIT authentication and Ansible Automation

> SSH runs over `TCP`, usually on port `22`

Before SSH login happens, TCP conection must work.

```bash
nc -vz <server-ip> 22
```

SSH has two sides

1. **SSH client**
   - This is the machine from where you connect. `ssh ubuntu@10.0.1.20`
2. **SSH server**
   - This runs on the remote Linux machine. `sudo systemctl status sshd`
