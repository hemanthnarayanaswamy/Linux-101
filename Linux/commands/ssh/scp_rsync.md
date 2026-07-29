# The `scp` Command

The `scp` (secure copy) command in Linux is used to securely copy files and directories between(to/from) two locations using SSH for authentication and encryption.

- `SCP` relies on SSH for data transfer. You need either an SSH key or a password to authenticate on the remote system.
- The colon (`:`) is how scp distinguishes between local and remote paths. A path without a colon is treated as local.
- You must have read permission on the source and write permission on the destination.
- SCP overwrites files without warning when the source and destination share the same name.
- When transferring large files, run the scp command inside a screen or tmux session to keep the transfer running if your terminal disconnects.

```bash
scp [OPTIONS] [[user@]host:]source [[user@]host:]destination

# [[user@]host:]source — Source path. Include the username and hostname (or IP address) when the file is on a remote machine.
# [[user@]host:]destination — Destination path. Same format as the source.
```
<ul><li><code>-P</code> — Remote host SSH port (uppercase P)</li><li><code>-p</code> — Preserve modification time, access time, and mode</li><li><code>-r</code> — Copy directories recursively</li><li><code>-C</code> — Compress data during transfer</li><li><code>-q</code> — Suppress the progress meter and non-error messages</li><li><code>-i</code> — Path to the SSH private key (identity file)</li><li><code>-l</code> — Limit bandwidth in Kbit/s</li><li><code>-o</code> — Pass an SSH option (e.g., <code>-o StrictHostKeyChecking=no</code>)</li><li><code>-3</code> — Route traffic between two remote hosts through the local machine</li><li><code>-O</code> — Force the legacy SCP protocol instead of SFTP</li></ul>

### 1. Copy a Local File to a Remote System

To copy a file from the local machine to a remote server, run:

```bash
scp file.txt remote_username@10.10.0.2:/remote/directory
scp file.txt remote_username@10.10.0.2:/remote/directory/newfilename.txt # overwrite file

# You will be prompted to enter the user password, and the transfer process will start:

# -r to recuressive copy the files in directory
scp -r /local/directory remote_username@10.10.0.2:/remote/directory
```

When you want to copy multiple local files that match a pattern, let the local shell expand the ***wildcard*** before `scp` runs.

In the following example, all .txt files from the local Projects directory are copied to the remote Projects directory:
```bash
scp "$HOME"/Projects/*.txt remote_username@10.10.0.2:/home/user/Projects/
```
* To preserve the file ***metadata*** `-p`
```bash
scp -p file.txt remote_username@10.10.0.2:/remote/directory/
```

### 2. Copy a Remote File to the Local System

```bash
# copy file
scp remote_username@10.10.0.2:/remote/file.txt /local/directory

# copy directory
scp -r remote_username@10.10.0.2:/remote/directory /local/directory
```
### 3. Copy Files Between Two Remote Systems
you do not need to log in to either server to transfer files between two remote machines. 

```bash
scp user1@host1.com:/files/file.txt user2@host2.com:/files

# Here the traffic will be routed from source to destination

# To route the traffic through the local machine instead, use the -3 option:
scp -3 user1@host1.com:/files/file.txt user2@host2.com:/files
```
### 4. Using `SSH Config File`
If you regularly connect to the same hosts, defining them in the SSH config file simplifies your scp commands. Create or edit the file at `~/.ssh/config`

```bash
Host myserver
    HostName 10.10.0.2
    User leah
    Port 2222
    IdentityFile ~/.ssh/id_ed25519

scp file.txt myserver:/remote/directory # alias instead of complete login
```

Use `scp` when:
- you need a simple one-time file copy
- you are copying one file or a small directory
- you already have SSH access
- you do not need ***sync/resume*** behavior

---
# The `rsync` Command

[rsync](https://linuxize.com/post/how-to-use-rsync-for-local-and-remote-data-transfer-and-synchronization/) (*remote sync*) is a versatile command-line utility for synchronizing files and directories between two locations over a remote shell or from/to a remote Rsync daemon. It is commonly used for mirroring data, incremental backups, and copying files between systems.

Use `rsync` when you need:
- directory sync
- copy only changed files
- resume interrupted transfers
- preserve permissions/timestamps
- delete destination files that no longer exist in source
- efficient backups/deployments

> For serious file movement, `rsync` is usually better than `scp`.

`rsync` needs to be installed `sudo apt install rsync`

1. Local to Remote `rsync file.txt user@server:/tmp/`
2. Remote to Local `rsync user@server:/var/log/app.log .`

| Option | Meaning |
|---|---|
| `-a` | Archive mode; preserves permissions, timestamps, symlinks, etc. |
| `-v` | Verbose |
| `-z` | Compress during transfer |
| `-h` | Human-readable output |
| `--progress` | Show transfer progress |
| `--delete` | Delete files at destination not present in source |
| `--dry-run` | Show what would happen without changing anything |
| `-e ssh` | Use SSH transport or custom SSH options |

3. Directory to Remote `rsync -av ./project/ user@server:/srv/project/`
4. Common Practical combo `rsync -avh --progress source/ user@server:/path/`

---
| Need | Better Tool |
|---|---|
| Quick one-file copy | `scp` |
| Sync whole directory repeatedly | `rsync` |
| Copy only changed files | `rsync` |
| Resume partial transfer | `rsync` |
| Delete stale destination files | `rsync --delete` |
| Simple ad hoc transfer | `scp` |
| Backup/deploy/migration | `rsync` |
