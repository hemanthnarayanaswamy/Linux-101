# Introduction to SSH

`SSH`, or Secure Shell, is a cryptographic network protocol primarily used for secure network services over an unsecured network. SSH provides strong encryption and integrity checks. It is widely used for:

- Remote command-line login
- Remote command execution
- Secure file transfer using `SCP` _(Secure Copy Protocol)_ or `SFTP` _(Secure File Transfer Protocol)_
- Tunneling and forwarding of ports
- GIT authentication and Ansible Automation

> SSH runs over `TCP`, usually on port `22`

Before SSH login happens, TCP conection must work. 
- Conventional methods that transfer data in plain text, such as Telnet, FTP, and login, can be safely replaced with SSH. File transfers, network service tunneling, and remote administration are among its common uses.

```bash
nc -vz <server-ip> 22
```

#### SSH has two sides

1. **SSH client**
   - The SSH client is the user’s local machine from which the connection to the remote server is initiated.
   - This is the machine from where you connect. `ssh ubuntu@10.0.1.20`
2. **SSH server**
   - The SSH server is the remote system that accepts incoming connections and provides access to its resources.
   - This runs on the remote Linux machine. `sudo systemctl status sshd`

### What Does SSH do ? 

1. **Secure Communication:** A client and a server can communicate securely thanks to SSH. To prevent unwanted access to the data, it encrypts all information sent over the network, including passwords, usernames, and other private data.
2. **Authentication:** SSH offers methods for confirming the legitimacy of the client and server. To confirm the parties’ identities, it makes use of cryptographic keys. 
3. **Data Transfer via Encryption:** SSH encrypts all data transferred between the client and server to prevent bad actors from listening in on it or altering it.
4. SSH facilitates safe **file transfers** between computers by using programs such as Secure Copy Program `(SCP)` and SSH File Transfer Protocol `(SFTP)`. With the help of these tools, users can safely move data between two remote servers or between a local computer and a distant server.
5. **Tunneling:** The ability to build secure channels for the transmission of other network protocols over SSH is made possible by the functionality for tunneling provided by SSH.
6. **Port Forwarding** – By mapping a client’s port to the server’s remote ports, SSH helps secure other network protocols, such as TCP/IP.

To Understand the working on how the ssh works refer to [SSH DEEP DIVE](../concepts/ssh/ssh_deepdive.md)

Main SSH Config Files

| File | Side | Purpose |
|---|---|---|
| `~/.ssh/config` | Client | Per-user SSH client config |
| `/etc/ssh/ssh_config` | Client | System-wide SSH client config |
| `/etc/ssh/ssh_config.d/*.conf` | Client | Extra client config snippets |
| `/etc/ssh/sshd_config` | Server | Main SSH server config |
| `/etc/ssh/sshd_config.d/*.conf` | Server | Extra server config snippets |
| `~/.ssh/authorized_keys` | Server | Public keys allowed to log in as that user |
| `~/.ssh/known_hosts` | Client | Server host keys remembered by the client |
| `/etc/ssh/ssh_known_hosts` | Client | System-wide trusted host keys |
| `/etc/ssh/ssh_host_*_key` | Server | Server private host keys |
| `/etc/ssh/ssh_host_*_key.pub` | Server | Server public host keys |

To Understand the details for different keys and ssh files [SSH Keys and SSH config files](../concepts/ssh/ssh_keys_Files.md)

All important SSH Commands. 

| Order | Command | Purpose |
|---:|---|---|
| 1 | `ssh` | Connect to remote Linux servers |
| 2 | `ssh-keygen` | Create/manage SSH keys |
| 3 | `ssh-copy-id` | Copy your public key to a server |
| 4 | `ssh-agent` | Hold unlocked private keys in memory |
| 5 | `ssh-add` | Add/list/remove keys in `ssh-agent` |
| 6 | `scp` | Copy files over SSH |
| 7 | `rsync` | Efficient file sync over SSH |
| 8 | `sshpass` | Password-based automation, usually avoid |

![cheat sheet](https://www.howtouselinux.com/wp-content/uploads/2023/12/SSH-Configuration-Cheat-Sheet-new.png)



