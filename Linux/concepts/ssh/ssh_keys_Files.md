# SSH KEYS

An **SSH key file** is a cryptographic credential used in the **SSH (Secure Shell)** protocol to authenticate users or systems without relying on passwords. 

It comes in *pairs* — a private key (kept secret on the client) and a public key (shared with servers). Together, they enable secure, encrypted, and automated access to remote systems.

The ssh key-pair is generated on the **CLIENT SIDE**, where `private key` stays in clients system, while the `public key` should be shared with the server and be stored on the server.

![key](https://media.geeksforgeeks.org/wp-content/uploads/20260120121640390794/public_key.webp)

#### 1. Public Key

A public key is the part of an SSH key pair stored on the _server_ to authorize a user’s access. It works with the private key to verify the client’s identity, enabling secure, password less authentication.

- Stored in the server’s `~/.ssh/authorized_keys` file.
- Cannot decrypt data or log in by itself, ensuring security even if exposed.
- Used during authentication to validate the client without transmitting sensitive information.

#### 2. Private Key

A private key is kept securely on the client device and is used to prove the user’s identity to the server. It must remain secret, as possession of the private key allows access to servers authorized for that key.

- Generates cryptographic signatures during authentication.
- Never transmitted over the network to maintain security.
- Provides the “unlocking” capability for the server’s public key “lock.”

![ssh1](../../../resources/assets/ssh1.png)

## Key File Types:

1. **Identity Keys** – Private keys stored locally (e.g., `~/.ssh/id_rsa`).
2. **Authorized Keys** – Public keys stored on the server `~/.ssh/authorized_keys`.
3. **Host Keys** – Identify servers to prevent man-in-the-middle attacks.

---
### 1. IDENTITY KEYS

In SSH authentication, *identity keys* and *identity files* are closely related but refer to different concepts.
- An `identity key` is the key your SSH client uses to prove who you are to a remote server. It belongs to the **user/client**, not the server.
- An `identity file` is simply the file that stores the private key.

```bash
identity key -> proves the client/user identity
host key     -> proves the server identity
```

 It is generated (along with its matching public key) using tools like `ssh-keygen`. The public key is placed on the server (in `~/.ssh/authorized_keys`), while the private key remains securely on the client machine. 

```bash
ssh-keygen -t ed25519 -C "hemanth@email.com"

~/.ssh/id_ed25519      -> Private Identity key
~/.ssh/id_ed25519.pub  -> Public Identity key

# Different algorithms can be used and which intern generate different keys styles

~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub

~/.ssh/id_rsa
~/.ssh/id_rsa.pub

~/.ssh/id_ecdsa
~/.ssh/id_ecdsa.pub

~/.ssh/id_ed25519_sk
~/.ssh/id_ed25519_sk.pub

# Moder practical choice ed25519

# The Private key looks like this:

-----BEGIN OPENSSH PRIVATE KEY-----
...
-----END OPENSSH PRIVATE KEY-----

# The Public Key looks like this: 
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI... hemanth@email.com

# parts: ssh-ed25519
# key type: AAAAC3...
# Actual public key data: hemanth@email.com
```

By default, `OpenSSH` looks for identity files in `~/.ssh/` with names like **id_rsa, id_ecdsa, id_ed25519, etc**. If your key is stored under a different name or location, you must explicitly tell SSH where to find it using the `-i` option or the Identity.

```bash
ssh -i ~/.ssh/my_custom_key user@host
```
### 2. AUTHORIZED KEYS
*Authorized_keys* is a server-side file that lists which ***public keys*** are allowed to log in as a specific Linux user.

SSH authorized_keys is a file located in the user’s home directory on a remote server that is used to specify public keys that are authorized for logging in to the user’s account.

When a user attempts to log in to a remote server using SSH key-based authentication, the server **checks the user’s public key against the authorized_keys file** to verify that the user is authorized to access the server.

```ini
/home/ubuntu/.ssh/authorized_keys
/home/deploy/.ssh/authorized_keys
/root/.ssh/authorized_keys

ssh deploy@server
# openssh checks /home/deploy/.ssh/authorized_keys

1. Client offers a public key.
2. Server checks deploy's ~/.ssh/authorized_keys.
3. If that public key is listed, server sends a challenge.
4. Client signs the challenge using the private key.
5. Server verifies the signature using the public key.
6. If valid, login succeeds.
```

### 3. HOST KEYS

A host key is the SSH server’s identity key. ***Host keys live on the SSH SERVER***

![host key](https://www.howtouselinux.com/wp-content/uploads/2023/12/ssh-host-key.png)

```bash
ssh ubuntu@server

# Your identity key proves you are allowed to log in.
# The server host key proves the server is really that server.

/etc/ssh/

/etc/ssh/ssh_host_ed25519_key # private host key must stay secret on the server
/etc/ssh/ssh_host_ed25519_key.pub # public host key can be shared with clients.

/etc/ssh/ssh_host_rsa_key
/etc/ssh/ssh_host_rsa_key.pub

/etc/ssh/ssh_host_ecdsa_key
/etc/ssh/ssh_host_ecdsa_key.pub

1. Client connects to server.
2. Server presents its host public key.
3. Server proves it owns the matching private host key.
4. Client checks this host key against known_hosts.
5. If trusted, SSH continues.
6. Then user authentication happens using your identity key/password/etc.
```
Each server in the SSH world has a one-of-a-kind identifier called a `host key`, similar to a digital fingerprint.

When you connect for the first time, the server sends its unique key to your device. Your device stores this key in a file named `“known_hosts“`.

On subsequent connections, the server again sends its key. Your device checks this key against the one stored in `“known_hosts”`. If they match, you’re good to go – the connection is secure.

##### First Connection Prompt

The first time you connect, you may see:

```ini
The authenticity of host 'server' can't be established.
ED25519 key fingerprint is SHA256:abc123...
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
- Your client has never seen this server's host key before.
- If you type: yes
- SSH saves the server’s public host key into:  `~/.ssh/known_hosts`
- After that, future connections are checked against that saved key.

##### KNOWN HOSTS
`known_hosts` is on the client machine. It stores public host keys of servers you have connected to. *known_hosts = servers this client remembers and trusts*

```bash
~/.ssh/known_hosts # user level
/etc/ssh/ssh_known_hosts # syste-wide known hosts
```
| File | Location | Purpose |
|---|---|---|
| `id_ed25519` | Client | Your private identity key |
| `id_ed25519.pub` | Client/server | Your public identity key |
| `authorized_keys` | Server user account | Public keys allowed to log in as that user |
| `ssh_host_ed25519_key` | Server | Server private host key |
| `ssh_host_ed25519_key.pub` | Server | Server public host key |
| `known_hosts` | Client | Server public keys remembered by client |

##### Host Key Changed Warning

If a server’s host key changes, or A mismatch between the keys triggers the warning. SSH may show: **WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!**

1. server was reinstalled
2. server was replaced
3. IP/hostname now points to a different machine
4. load balancer points to a different backend
5. DNS changed
6. known_hosts has old data
7. man-in-the-middle attack

> Usually host keys are generated automatically when OpenSSH server is installed.
> `sudo ssh-keygen -A` creates missing default host keys under `/etc/ssh`

---


## SSH Configuration 

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

#### 1. Client Config `~/.ssh/config`

This file controls how your SSH CLIENT connects to SERVERS.

```bash
$: cat ~/.ssh/config

Host bastion
    HostName bastion.example.com
    User ubuntu
    IdentityFile ~/.ssh/id_ed25519_bastion

Host private-app
    HostName 10.0.2.15
    User deploy
    IdentityFile ~/.ssh/id_ed25519_prod
    ProxyJump bastion
    IdentitiesOnly yes

# Now instead of typing:
ssh -i ~/.ssh/id_ed25519_prod deploy@10.0.1.20
ssh private-app
```
Common Options:

| Option | Meaning |
|---|---|
| `Host` | Alias/pattern you type after `ssh` |
| `HostName` | Real hostname or IP |
| `User` | Login user |
| `Port` | SSH port |
| `IdentityFile` | Private key to use |
| `IdentitiesOnly yes` | Use only the configured key |
| `ProxyJump` | Connect through a jump/bastion host |
| `ForwardAgent` | Forward SSH agent, usually keep `no` |
| `LocalForward` | Local port forwarding |
| `StrictHostKeyChecking` | Host key verification behavior |

#### 2. System Client Config: `/etc/ssh/ssh_config` 
This is the global client config for all users. It sets default SSH client behavior for the whole machine.

```bash
Host *
    ForwardAgent no
    ServerAliveInterval 60
    ServerAliveCountMax 3

# Apply these settings to all SSH client connections. 
```
> User config usually overrides system config.

```bash
# SSH Config order
1. command-line options
2. ~/.ssh/config
3. /etc/ssh/ssh_config

# To insepct final client config
ssh -G host_name_declared_on_config_file
```
#### 3. Server Config `/etc/ssh/sshd_config`
This controls the SSH server daemon: `sshd`

```bash
sshd 

cat /etc/ssh/sshd_config

Port 22
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PermitEmptyPasswords no
AllowUsers deploy ubuntu
```
| Option | Meaning |
|---|---|
| `Port` | Port SSH server listens on |
| `PermitRootLogin` | Allow or block root SSH login |
| `PasswordAuthentication` | Allow password login |
| `PubkeyAuthentication` | Allow key-based login |
| `AuthorizedKeysFile` | Where to find allowed public keys |
| `AllowUsers` | Only allow listed users |
| `DenyUsers` | Block listed users |
| `AllowGroups` | Only allow users from listed groups |
| `X11Forwarding` | Allow GUI forwarding |
| `PermitTunnel` | Allow SSH tunnels |
| `ClientAliveInterval` | Server-side keepalive interval |

###### SAFE SERVER CONFIG WORKFLOW

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak # backup before editing

sudo sshd -t # syntax check

sudo systemctl reload sshd # reload SSH
```
#### 4. Config Snippet Directories

These are split config files

```bash
/etc/ssh/ssh_config.d/*.conf
/etc/ssh/sshd_config.d/*.conf
```
Example: `/etc/ssh/sshd_config.d/99-hardening.conf`

```ini
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes

Thisk eep scustom changes separate from the package-managed main config.
```
#### 5. Important SSH Data Files

1. `authorized_keys` Server-side listing public keys allowed to log in as that user `~/.ssh/authorized_keys`
2. `known_hosts` Client-side file remembering server host keys. `~/.ssh/known_hosts`
3. `identity keys` Client-side user login keys. `~/.ssh/id_ed25519`, `~/.ssh/id_ed25519.pub`
4. `host keys` Server identity keys `/etc/ssh/ssh_host_ed25519_key`, `/etc/ssh/ssh_host_ed25519_key.pub`
