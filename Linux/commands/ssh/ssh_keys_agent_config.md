# The `ssh-keygen` Command

`ssh-keygen `is used to create and manage SSH keys.

1. generate identity keys
2. show key fingerprints
3. change key passphrases
4. recreate public keys
5. remove old host keys from known_hosts

> `ssh-keygen` creates keys, inspects keys, repairs public keys, changes passphrases, and cleans known_hosts entries.

```bash
ssh-keygen -t ed25519 -C "hemanth@email.com"

# this creates 
~/.ssh/id_ed25519 # private key
~/.ssh/id_ed25519.pub # public key
```
It supports multiple algorithms such as **RSA, ECDSA, Ed25519**

* To Generate with custom filename 

```bash
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519_work -C "hemanth@work"

ssh-keygen -t ed25519 -a 100 -f ~/.ssh/id_ed25519_prod -C "prod-access"
```

| Option | Meaning |
|---|---|
| `-t ed25519` | Key algorithm |
| `-a 100` | More rounds for protecting private key passphrase |
| `-f file` | Output file path |
| `-C comment` | Human-readable label |

* Generate Public Key from Private Key

```bash
# If you lost the .pub file
ssh-keygen -y -f ~/.ssh/id_ed25519 > ~/.ssh/id_ed25519.pub
# This recreates the public key from the private key
```

### Key Fingerprints

An **SSH host key fingerprint** is a short, unique identifier derived from a server’s public SSH host key. It’s used to verify that you are connecting to the correct server and to prevent man-in-the-middle attacks. 

* Checking a Local Server’s Host Key Fingerprint: `ssh-keygen -lf ~/.ssh/id_ed25519.pub`, `ssh-keygen -lf ~/.ssh/id_ed25519`
* Checking a Remote Server's Fingerprint without logging in: `ssh-keyscan example.com | ssh-keygen -lf -`

`-l` → list fingerprint

#### During First SSH Connecton: 
When connecting for the first time:

```bash
ssh user@example.com

# You'll see a message
The authenticity of host 'example.com' can't be established
RSA key fingerprint is SHA256:q0WFctlFbpSL2DHEPqDjCmanxpqYQBjC9jY8Cq1J5ZA
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```
Verify this fingerprint against the one obtained securely from the server admin or by checking directly on the server:

```bash
sudo ssh-keygen -lf /etc/ssh/ssh_host_rsa_key.pub
```
If it matches, type **yes** to save it in `~/.ssh/known_hosts`

#### Remove Old Host Key From `known_hosts`

When a server is rebuilt, you may see: *WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!*

If the change is expected, remove the old key: and then reconnect
- `ssh-keygen -R server.example.com` remove by hostname
- `ssh-keygen -R 192.168.1.20` remove by IP address
- `ssh-keygen -R "[server.example.com]:2222"` for a custom port

#### Generate Server Host Keys
Usually done automatically when OpenSSH server is installed.

```bash
sudo ssh-keygen -A

# This creates missing host keys under

/etc/ssh/
```
---
## How to use `ssh-keygen` to Generate a New SSH Key ?

1. Generate SSH keys on Linux/macOS
```bash
ssh-keygen -t ed25519 -C "your@email.com"

# output

Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/user/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase): [choose a strong passphrase]
Enter same passphrase again:
Your identification has been saved in /home/user/.ssh/id_ed25519
Your public key has been saved in /home/user/.ssh/id_ed25519.pub
```
2. Add Public Key to Github
    * Copy you public key: `cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard`
    * GitHub -> Settings -> SSH and GPG keys -> New SSH key
    * Paste the public key, give it a name
    * Test connection `ssh -T git@github.com`

3. Add Public Key to Server
    * Append you public key to the server's `authorized_keys`

`ssh-copy-id` copies your public key to a remote server’s. Install my public key on the server so I can log in with my private key.

```bash
ssh-copy-id ubuntu@192.168.1.20

# This copies your default public key, usually:
~/.ssh/id_ed25519.pub -> /home/ubuntu/.ssh/authorized_keys

# After that, you can log in with
ssh ubuntu@192.168.1.20
```
1. Logs in using **password** or another working method.
2. Creates `~/.ssh` if needed.
3. Sets safe permissions.
4. Appends your public key to `~/.ssh/authorized_keys`.
5. Avoids adding duplicate keys.

```bash
# Append your public key to the server's authorized_keys
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@server-ip

# Or manually:
cat ~/.ssh/id_ed25519.pub | ssh user@server-ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```
4. SSH Config File: Managing Multiple Keys
Create `~/.ssh/config` to set per-host options:

```bash
# Personal GitHub
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519

# Work GitHub (separate account)
Host github-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work

# Production server
Host prod
  HostName 203.0.113.10
  User deploy
  IdentityFile ~/.ssh/id_ed25519_vps
  Port 2222
```
---
# The `ssh-agent` Command

A `passphrase` for an SSH key adds an extra layer of security to your SSH authentication process. When you generate an SSH key pair, you have the option to add a passphrase to the private key. This passphrase is used to encrypt the private key on disk, ensuring that even if someone gains access to your private key file, they cannot use it without knowing the passphrase.

Example: ***GitHub Commit Signing Passphrase***, where we need to sign the
You can add or change the passphrase for an existing private key without regenerating the key pair by using the `ssh-keygen` command. Here is how you can do it

```bash
$ ssh-keygen -p -f ~/.ssh/id_ed25519
> Enter old passphrase: [Type old passphrase]
> Key has comment 'your_email@example.com'
> Enter new passphrase (empty for no passphrase): [Type new passphrase]
> Enter same passphrase again: [Repeat the new passphrase]
> Your identification has been saved with the new passphrase
```

If your private key has a passphrase: 
`ssh user@server` 
SSH may ask you to 
`Enter passphrase for key '~/.ssh/id_ed25519':`
That is good security, ***but annoying if you SSH many times.***

with `ssh-agent`:
1. You unlock the key once.
2. `ssh-agent` keeps it in memory.
3. Future `ssh/scp/rsync/git` commands ask the agent to sign authentication challenges.
4. You do not retype the passphrase every time.

`ssh-agent` is a background that keeps your unlocked private keys in memory if passphrase is used.
>  If your private RSA key is not encrypted with a passphrase, then ssh-agent is not necessary.

```bash
# Start the ssh-agent
eval "$(ssh-agent -s)"

# Add Key To Agent
ssh-add ~/.ssh/id_ed25519

# Then list loaded keys
ssh-add -l

# Remove one key 
ssh-add -d ~/.ssh/id_ed25519

# kill agent
ssh-agent -k

# Directly use it in the sshconfig file
Host prod
    HostName prod.example.com
    User deploy
    IdentityFile ~/.ssh/id_ed25519_prod
    IdentitiesOnly yes
    AddKeysToAgent yes
```